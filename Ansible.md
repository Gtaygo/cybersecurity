# 🛠️ Instalação e Configuração do Ansible no Ubuntu

Este guia descreve o processo de instalação do **Ansible** em um sistema Ubuntu, bem como a configuração básica para gerenciamento remoto de hosts.

---

## 📦 Etapa 1: Instalando o Ansible

### Atualize os pacotes do sistema:

```bash
sudo apt update

````
##
```bash
sudo apt-add-repository --yes --update ppa:ansible/ansible

```
##
```bash
sudo apt install ansible -y
```
##

## 📁 Etapa 2: Estrutura de Diretórios e Arquivos
Diretório principal do Ansible:
```bash
cd /etc/ansible
ls
```
hosts: Arquivo de inventário padrão onde os hosts gerenciados são listados.

ansible.cfg: Arquivo de configuração principal.

roles/: Diretório onde ficam as funções e playbooks.
##

## 🔐 Etapa 3: Configurando Acesso SSH
Gere uma chave SSH sem senha:
```bash
ssh-keygen -t rsa -b 4096

```
🗂️ Salve a chave em /etc/ansible/keys/key (ou em outro local que desejar).


Copie a chave para os hosts remotos:

```bash
ssh-copy-id usuario@192.168.0.5
```

Verifique se a chave foi copiada com sucesso no host remoto:
```bash
cat ~/.ssh/authorized_keys

```
Teste a conexão SSH:
```bash
ssh usuario@192.168.0.5
````
## 🖧 Etapa 4: Criando Grupos de Hosts
```bash
sudo vim /etc/ansible/hosts

````
Adicione seu grupo de hosts no final do arquivo:
```
[webservers]
192.168.0.5
192.168.0.6
192.168.0.7

````
## 🧪 Etapa 5: Testando a Conexão com os Hosts
```bash
ansible webservers -m ping
````
💡 Substitua webservers pelo nome do grupo definido no arquivo hosts

## 📜 Etapa 6: Criando um Playbook
```bash
# /etc/ansible/roles/httpd.yaml

---
- name: Instalar o Apache
  hosts: webservers
  become: yes
  tasks:
    - name: Instalar o pacote httpd
      command: /usr/bin/yum -y install httpd
````
Execute o playbook:
```bash
ansible-playbook /etc/ansible/roles/httpd.yaml
```
 Nota: O comando acima usa o yum, que é padrão em distribuições baseadas em RHEL/CentOS.
Se estiver usando Ubuntu nos hosts, substitua por apt:
```bash
command: /usr/bin/apt -y install apache2
````
## ✅ Etapa 7: Verificação no Host

````
systemctl status httpd   # Em RHEL/CentOS
systemctl status apache2 # Em Ubuntu/Debian


