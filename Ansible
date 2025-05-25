#Instalando e configurando o ansible em um ubuntu. 
 apt-update #para evitar problemas futuros.
apt-add-repository --yes --update ppa:ansible/ansible #para adquirir o repositorio do ansible.
sudo apt install ansible #instalando o ansible.
cd / etc/ansible #diretorio
ls
cd roles/ #é o diretorio de funções
cat hosts /diretorio de hosts
vi ansible.cnfg #se quiser configurar o ansible
ssh-keygen -t rsa -b 4096 #para criar uma key ssh.. é a chave de comunicação.
/etc/ansible/keys/key #local pra criar a key
#não coloque uma senha.
ssh-copy-id 192.168.0.5 #para copiar isso para os outros hosts.
   #adicionando as chaves em outro host:
ls
cat .ssh/authorized_keys #realize estes comandos e adicione a chave no outro host.

#voltando ao servidor principal:
#teste de chave sshs
ssh 192.168.0.5

#para criar um grupo de hosts:
sudo vim /etc/ansible/hosts #acessar o diretorio
[nomedogrupo] 
192.168.0.5
192.168.0.6
192.168.0.7
#adicione isso abaixo da linha Ex 2 para adicionar seus hosts ao grupo.

#teste com um ping os servidores deste grupo, no server master.
-m ping nomedogrupo #exemplo do comando ping.

#criando playbook (conjunto de comandos) do ansible
sudo vim /etc/ansible/roles/httpd.yaml #acesse o diretorio
Name: instalar o apache
Command: /urs/bin/yum -y install httpd
become: yes
#adicionar abaixo da linha tasks.  #executar o comando via ansible.

ansible playbook /etc/ansible/roles/httpd.yaml #pode demorar para eles se comunicarem via ssh e executar a tarefa.

#teste no host hipotetico

systemctl status httpd #para procurar o arquivo instalar.
