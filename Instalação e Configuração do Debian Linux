# Documentação
# Instalação
-Verifique requisitos de hardware:
    -tipo de instalação 
      sem desktop: minímo de RAM (256 megabytes), recomendado (512 megabytes), disco rígido (4 gigabytes)
      com desktop: minímo de RAM (1 gigabytes), recomendado ( 2 gigabytes), disco rígido (10 gigabytes)
  -Para conseguir baixar é preciso que vc tenha instalado o Virtual Box para conseguir utilizar a máquina virtual e tenha seu arquivo .iso instalado
    passo a passo:
    1.clique em novo e escolha o arquivo .iso do seu debian, 
    2.escolha o espaço que vc vai designar para a sua máquina,
    3.agora é só instalar, quando abrir escolha a opção de configurar o debian pelo gráfico,
    4.coloque a liguagem de sua preferença e depois digite o nome do seu hostname com letra minúscula,
    5.coloque sua senha root ( nunca a esqueça),
    6.coloque seu nome de usuário e a senha,
    7.utilize o disco inteiro e faça as mudanças nele,
    8.utilize a separação para iniciantes,
    9.Não ler midias adicionais
    10.continue utilizando debian.org
    11.na seleção de softwares escolha: servidor we e ssh, xfce, ambiente de trabalho do debian e sistema padrão
      
# DNS
 -Pré-requisitos:
    • Sistema Debian 12 atualizado.
    • Privilégios de sudo ou acesso root.
    • Endereço IP estático configurado no servidor (por exemplo: 192.168.1.151).
    • Domínio de exemplo: empresa.local.
    • Rede local: 192.168.1.0/24. 
-Passo a Passo:
1. Instalação do BIND9
   Passo 1: Atualizar o Sistema
    sudo apt update && sudo apt upgrade -y
   Passo 2: Instalar o Pacote BIND9
    sudo apt install bind9 bind9utils bind9-doc dnsutils -y
   Passo 3: Verificar a Versão e Gerenciar o Serviço
    named -v # Exibe a versão do BIND (por exemplo: BIND 9.18.x)
    sudo systemctl start named
    sudo systemctl enable named
    sudo systemctl status named # Confirme o status "active (running)"
2. Configuração Básica do BIND9
  Os arquivos de configuração do BIND9 estão localizados em /etc/bind/.
    Passo 1: Configurar Opções Globais
    Edite o arquivo /etc/bind/named.conf.options:
     sudo nano /etc/bind/named.conf.options
    Adicione ou ajuste as seguintes configurações:
     acl "rede-privada" {
      192.168.1.0/24;
     };
    options {
    directory "/var/cache/bind";
    recursion yes;
    allow-recursion { localhost; rede-privada; };
    listen-on { 127.0.0.1; 192.168.1.151; }; # Substitua pelo IP do seu
    servidor
    allow-transfer { none; }; # Restringe transferências de zona
   forwarders {
     8.8.8.8;
     8.8.4.4;
    };
  dnssec-validation auto;
  listen-on-v6 { any; };
  };

Passo 2: Definir Zonas no named.conf.local
 Edite o arquivo /etc/bind/named.conf.local

 sudo nano /etc/bind/named.conf.local

Adicione as definições das zonas:
// Zona de Pesquisa Direta (Forward)

zone "empresa.local" {
 type master;
 file "/etc/bind/forward.empresa.local";
 allow-update { none; };
};
// Zona de Pesquisa Reversa (Reverse)
zone "1.168.192.in-addr.arpa" {
 type master;
 file "/etc/bind/reverse.empresa.local";
 allow-update { none; };
};

Passo 3: Verificar Sintaxe das Configurações
sudo named-checkconf # Se não houver erros, não haverá saída

3. Configuração das Zonas DNS
 Passo 1: Criar Arquivos de Zona
 Navegue até o diretório /etc/bind e copie os templates padrão:

  cd /etc/bind
  sudo cp db.empty forward.empresa.local
  sudo cp db.empty reverse.empresa.local

 Passo 2: Configurar Zona Direta (forward.empresa.local)
 Edite o arquivo:
  sudo nano forward.empresa.local

 Insira o seguinte conteúdo:
  $TTL 604800
  @ IN SOA ns.empresa.local. admin.empresa.local. (
              3 ; Serial
         604800 ; Refresh
          86400 ; Retry
        2419200 ; Expire
       604800 ) ; Negative Cache TTL
                ; Registros NS e A

@ IN NS ns.empresa.local.
ns    IN A 192.168.1.151; Outros Registros
www   IN A 192.168.1.190
mail  IN A 192.168.1.191
ftp   IN CNAME www.empresa.local.
@ IN MX 10 mail.empresa.local.

Passo 3: Configurar Zona Reversa (reverse.empresa.local)
 Edite o arquivo:
  sudo nano reverse.empresa.local

Insira o seguinte conteúdo:

$TTL 604800
@ IN SOA ns.empresa.local. admin.empresa.local. (
             2 ; Serial
        604800 ; Refresh
         86400 ; Retry
       2419200 ; Expire
      604800 ) ; Negative Cache TTL

; Registros da Zona Reversa
@ IN NS ns.empresa.local.
151 IN PTR ns.empresa.local.
190 IN PTR www.empresa.local.
191 IN PTR mail.empresa.local.

Passo 4: Verificar Sintaxe das Zonas

sudo named-checkzone empresa.local forward.empresa.local
sudo named-checkzone 1.168.192.in-addr.arpa reverse.empresa.local

Saídas esperadas: mensagens indicando que as zonas foram carregadas com sucesso.

Passo 5: Reiniciar o BIND9

sudo systemctl restart bind9



# DHCP
Passo 1: Instalação do Pacote
  sudo apt update
  sudo apt install isc-dhcp-server -y
Passo 2: Configurar a Interface de Rede
 Edite o arquivo `/etc/default/isc-dhcp-server` para definir em qual interface o DHCP servirá: 
  sudo nano /etc/default/isc-dhcp-server 
 Altere para:
  INTERFACESv4="enp0s3" # Substitua "enp0s3" pela sua interface de rede (use `ip a` para
  verificar)
  INTERFACESv6="" # Deixe vazio se não usar IPv6 

Passo 3: Configurar o Arquivo `dhcpd.conf`
 Edite o arquivo principal de configuração:
  sudo nano /etc/dhcp/dhcpd.conf 
 Substitua o conteúdo por:

# Configurações Globais
default-lease-time 600; # Tempo padrão de concessão (10 minutos)
max-lease-time 7200; # Tempo máximo de concessão (2 horas)
authoritative; # Define este servidor como autoritativo para a rede
option domain-name "empresa.local";
option domain-name-servers 192.168.10.10; # Substitua pelo IP do seu DNS

# Configuração da Sub-rede
subnet 192.168.10.0 netmask 255.255.255.0 {
 range 192.168.10.100 192.168.10.200; # Faixa de IPs para clientes
 option routers 192.168.10.1; # Gateway padrão
 option broadcast-address 192.168.10.255;
 option subnet-mask 255.255.255.0;
}

//Notas importantes:
- O parâmetro `authoritative` é essencial para evitar conflitos com outros servidores DHCP.
- O IP `192.168.10.10` deve ser substituído pelo IP real do seu servidor DNS (ex: BIND9)

Passo 4: Verificar Sintaxe da Configuração
 Antes de reiniciar, valide a sintaxe para evitar erros: 
 sudo dhcpd -t -cf /etc/dhcp/dhcpd.conf
 //Saída esperada
  Configuration file syntax is okay. 
Passo 5: Reiniciar o Serviço e Habilitar no Boot
  sudo systemctl restart isc-dhcp-server
  sudo systemctl enable isc-dhcp-server
//Verifique o status:
 sudo systemctl status isc-dhcp-server 
//Saída desejada:
 - `Active: active (running)`
 - Sem erros em vermelho. 

Passo 6: Configurar Firewall (UFW)
Libere as portas do DHCP (67/UDP e 68/UDP): 
  sudo ufw allow 67/udp
  sudo ufw allow 68/udp
  sudo ufw reload 

Passo 7: Testar o Funcionamento
  sudo dhclient -v -r # Libera o lease atual
  sudo dhclient -v # Solicita um novo IP 

//Verifique o IP recebido
  ip a show dev <interface> 

Verifique os logs do DHCP Server:
  sudo tail -f /var/log/syslog | grep dhcpd 
