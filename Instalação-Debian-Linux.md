
# Conteúdo de cada arquivo
readme = # Servidor Debian: Instalação, DNS e DHCP

Documentação prática para configurar um servidor Debian com DNS (BIND9) e DHCP (ISC DHCP Server).

## 📄 Documentação

- [Instalação do Debian](docs/instalacao.md)
- [Servidor DNS com BIND9](docs/dns.md)
- [Servidor DHCP com ISC DHCP Server](docs/dhcp.md)
'''

instalacao = # Instalação do Debian em Máquina Virtual

## Requisitos de Hardware

- **Sem ambiente gráfico:** RAM 256 MB (mínimo), 512 MB (recomendado); 4 GB de disco
- **Com ambiente gráfico:** RAM 1 GB (mínimo), 2 GB (recomendado); 10 GB de disco

## Pré-requisitos

- VirtualBox instalado
- Arquivo `.iso` do Debian

## Passo a passo

1. Novo > selecione a `.iso`
2. Configure espaço em disco
3. Inicie e escolha "Instalação Gráfica"
4. Escolha idioma, hostname (letras minúsculas)
5. Defina senha root
6. Crie usuário e senha
7. Use disco inteiro e particionamento para iniciantes
8. Não ler mídias adicionais
9. Use `debian.org`
10. Marque: Web, SSH, XFCE, Ambiente Debian, Sistema padrão
'''

dns = '''# Configuração do Servidor DNS (BIND9)

## Pré-requisitos

- Debian 12 com IP fixo (ex: 192.168.1.151)
- Domínio: empresa.local | Rede: 192.168.1.0/24

## Instalação

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install bind9 bind9utils bind9-doc dnsutils -y
Configuração
named.conf.options
conf
copiar
Editar
acl "rede-privada" { 192.168.1.0/24; };
options {
  directory "/var/cache/bind";
  recursion yes;
  allow-recursion { localhost; rede-privada; };
  listen-on { 127.0.0.1; 192.168.1.151; };
  allow-transfer { none; };
  forwarders { 8.8.8.8; 8.8.4.4; };
  dnssec-validation auto;
  listen-on-v6 { any; };
};
named.conf.local
conf
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
forward.empresa.local
dns
Copiar
Editar
$TTL 604800
@ IN SOA ns.empresa.local. admin.empresa.local. (
  3 604800 86400 2419200 604800 )
@ IN NS ns.empresa.local.
ns IN A 192.168.1.151
www IN A 192.168.1.190
mail IN A 192.168.1.191
ftp IN CNAME www.empresa.local.
@ IN MX 10 mail.empresa.local.
reverse.empresa.local
dns
Copiar
Editar
$TTL 604800
@ IN SOA ns.empresa.local. admin.empresa.local. (
  2 604800 86400 2419200 604800 )
@ IN NS ns.empresa.local.
151 IN PTR ns.empresa.local.
190 IN PTR www.empresa.local.
191 IN PTR mail.empresa.local.
'''

dhcp = '''# Configuração do Servidor DHCP (ISC DHCP Server)

Instalação
bash
Copiar
Editar
sudo apt update
sudo apt install isc-dhcp-server -y
Interface de rede
conf
Copiar
Editar
# /etc/default/isc-dhcp-server
INTERFACESv4="enp0s3"
INTERFACESv6=""
Configuração principal
conf
Copiar
Editar
# /etc/dhcp/dhcpd.conf
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
Teste
bash
Copiar
Editar
sudo dhcpd -t -cf /etc/dhcp/dhcpd.conf
sudo systemctl restart isc-dhcp-server
sudo ufw allow 67/udp && sudo ufw allow 68/udp
'''

