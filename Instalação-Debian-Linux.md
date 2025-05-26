
Documentação Técnica – Instalação, DNS e DHCP no Debian
1. Instalação do Debian (em Máquina Virtual)
Requisitos de Hardware
Instalação sem ambiente gráfico:

RAM mínima: 256 MB | Recomendado: 512 MB

Disco rígido: 4 GB

Instalação com ambiente gráfico:

RAM mínima: 1 GB | Recomendado: 2 GB

Disco rígido: 10 GB

Pré-requisitos
Oracle VirtualBox instalado.

Imagem .iso do Debian disponível.

Passo a Passo
No VirtualBox, clique em Novo, selecione a imagem .iso do Debian.

Defina o tamanho do disco e recursos (RAM, CPU).

Inicie a instalação e escolha a opção de instalação gráfica.

Configure:

Idioma e localização.

Hostname (em minúsculas).

Senha do root (anote com segurança).

Nome de usuário e senha.

Escolha “usar disco inteiro” e a opção de particionamento para iniciantes.

Não adicionar mídias adicionais.

Continuar usando o repositório debian.org.

Na seleção de software, marque:

Servidor Web

Servidor SSH

XFCE

Ambiente de trabalho Debian

Sistema padrão

2. Configuração de Servidor DNS com BIND9
Pré-requisitos
Debian 12 atualizado

Acesso root ou sudo

IP estático: 192.168.1.151

Domínio: empresa.local

Rede: 192.168.1.0/24

2.1 Instalação do BIND9
bash
Copiar
Editar
sudo apt update && sudo apt upgrade -y
sudo apt install bind9 bind9utils bind9-doc dnsutils -y
named -v   # Verifica a versão
sudo systemctl start named
sudo systemctl enable named
sudo systemctl status named
2.2 Configuração do BIND9
Editar /etc/bind/named.conf.options
bash
Copiar
Editar
sudo nano /etc/bind/named.conf.options
conf
Copiar
Editar
acl "rede-privada" {
  192.168.1.0/24;
};

options {
  directory "/var/cache/bind";
  recursion yes;
  allow-recursion { localhost; rede-privada; };
  listen-on { 127.0.0.1; 192.168.1.151; };
  allow-transfer { none; };
  forwarders {
    8.8.8.8;
    8.8.4.4;
  };
  dnssec-validation auto;
  listen-on-v6 { any; };
};
Editar /etc/bind/named.conf.local
bash
Copiar
Editar
sudo nano /etc/bind/named.conf.local
c
Copiar
Editar
zone "empresa.local" {
  type master;
  file "/etc/bind/forward.empresa.local";
  allow-update { none; };
};

zone "1.168.192.in-addr.arpa" {
  type master;
  file "/etc/bind/reverse.empresa.local";
  allow-update { none; };
};
Verificar sintaxe
bash
Copiar
Editar
sudo named-checkconf
2.3 Configuração das Zonas
Criar Arquivos de Zona
bash
Copiar
Editar
cd /etc/bind
sudo cp db.empty forward.empresa.local
sudo cp db.empty reverse.empresa.local
Editar forward.empresa.local
bash
Copiar
Editar
sudo nano forward.empresa.local
dns
Copiar
Editar
$TTL 604800
@ IN SOA ns.empresa.local. admin.empresa.local. (
            3 ; Serial
       604800 ; Refresh
        86400 ; Retry
      2419200 ; Expire
       604800 ) ; Negative Cache TTL

@ IN NS ns.empresa.local.
ns     IN A 192.168.1.151
www    IN A 192.168.1.190
mail   IN A 192.168.1.191
ftp    IN CNAME www.empresa.local.
@ IN MX 10 mail.empresa.local.
Editar reverse.empresa.local
bash
Copiar
Editar
sudo nano reverse.empresa.local
dns
Copiar
Editar
$TTL 604800
@ IN SOA ns.empresa.local. admin.empresa.local. (
            2 ; Serial
       604800 ; Refresh
        86400 ; Retry
      2419200 ; Expire
       604800 ) ; Negative Cache TTL

@ IN NS ns.empresa.local.
151 IN PTR ns.empresa.local.
190 IN PTR www.empresa.local.
191 IN PTR mail.empresa.local.
Validar arquivos de zona
bash
Copiar
Editar
sudo named-checkzone empresa.local forward.empresa.local
sudo named-checkzone 1.168.192.in-addr.arpa reverse.empresa.local
Reiniciar o serviço
bash
Copiar
Editar
sudo systemctl restart bind9
3. Servidor DHCP com ISC DHCP Server
3.1 Instalação
bash
Copiar
Editar
sudo apt update
sudo apt install isc-dhcp-server -y
3.2 Configurar Interface
bash
Copiar
Editar
sudo nano /etc/default/isc-dhcp-server
conf
Copiar
Editar
INTERFACESv4="enp0s3"  # Substitua pela sua interface de rede
INTERFACESv6=""
3.3 Editar Arquivo Principal
bash
Copiar
Editar
sudo nano /etc/dhcp/dhcpd.conf
conf
Copiar
Editar
default-lease-time 600;
max-lease-time 7200;
authoritative;
option domain-name "empresa.local";
option domain-name-servers 192.168.10.10;

subnet 192.168.10.0 netmask 255.255.255.0 {
  range 192.168.10.100 192.168.10.200;
  option routers 192.168.10.1;
  option broadcast-address 192.168.10.255;
  option subnet-mask 255.255.255.0;
}
Verificar sintaxe
bash
Copiar
Editar
sudo dhcpd -t -cf /etc/dhcp/dhcpd.conf
3.4 Reiniciar Serviço e Ativar no Boot
bash
Copiar
Editar
sudo systemctl restart isc-dhcp-server
sudo systemctl enable isc-dhcp-server
sudo systemctl status isc-dhcp-server
3.5 Configurar o Firewall
bash
Copiar
Editar
sudo ufw allow 67/udp
sudo ufw allow 68/udp
sudo ufw reload
3.6 Testar DHCP
bash
Copiar
Editar
sudo dhclient -v -r
sudo dhclient -v
ip a show dev <interface>
sudo tail -f /var/log/syslog | grep dhcpd
