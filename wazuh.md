# 🚨 Guia de Instalação do Servidor Wazuh com Filebeat

Este guia cobre a instalação de um nó do servidor **Wazuh** com **Filebeat** para coleta e envio de alertas ao indexador Wazuh.

---

## 🧩 Componentes

- **Wazuh Manager**: Coleta, analisa dados e gera alertas.
- **Filebeat**: Encaminha eventos do Wazuh para o indexador (ex: Elasticsearch).

---

## ⚙️ Pré-requisitos

- Sistema operacional baseado em Debian/Ubuntu
- Acesso root
- Internet ativa

---

## 🔧 Etapa 1: Instalação do Nó do Servidor Wazuh

### 1. Instale pacotes básicos

```bash
apt-get install gnupg apt-transport-https

curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | \
gpg --no-default-keyring --keyring gnupg-ring:/usr/share/keyrings/wazuh.gpg --import && \
chmod 644 /usr/share/keyrings/wazuh.gpg

echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main" | \
tee -a /etc/apt/sources.list.d/wazuh.list

apt-get update

apt-get -y install wazuh-manager

apt-get -y install filebeat

curl -so /etc/filebeat/filebeat.yml https://packages.wazuh.com/4.12/tpl/wazuh/filebeat/filebeat.yml

output.elasticsearch:
  hosts: ["10.0.0.1:9200"]
  protocol: https
  username: ${username}
  password: ${password}

filebeat keystore create

echo admin | filebeat keystore add username --stdin --force
echo admin | filebeat keystore add password --stdin --force

curl -so /etc/filebeat/wazuh-template.json https://raw.githubusercontent.com/wazuh/wazuh/v4.12.0/extensions/elasticsearch/7.x/wazuh-template.json
chmod go+r /etc/filebeat/wazuh-template.json

curl -s https://packages.wazuh.com/4.x/filebeat/wazuh-filebeat-0.4.tar.gz | \
tar -xvz -C /usr/share/filebeat/module

NODE_NAME=<SERVER_NODE_NAME>
mkdir /etc/filebeat/certs
tar -xf ./wazuh-certificates.tar -C /etc/filebeat/certs/ ./$NODE_NAME.pem ./$NODE_NAME-key.pem ./root-ca.pem
mv -n /etc/filebeat/certs/$NODE_NAME.pem /etc/filebeat/certs/filebeat.pem
mv -n /etc/filebeat/certs/$NODE_NAME-key.pem /etc/filebeat/certs/filebeat-key.pem
chmod 500 /etc/filebeat/certs
chmod 400 /etc/filebeat/certs/*
chown -R root:root /etc/filebeat/certs

echo '<INDEXER_USERNAME>' | /var/ossec/bin/wazuh-keystore -f indexer -k username
echo '<INDEXER_PASSWORD>' | /var/ossec/bin/wazuh-keystore -f indexer -k password

<indexer>
  <enabled>yes</enabled>
  <hosts>
    <host>https://10.0.0.1:9200</host>
    <!-- Para múltiplos nós -->
    <!-- <host>https://10.0.0.2:9200</host> -->
  </hosts>
  <ssl>
    <certificate_authorities>
      <ca>/etc/filebeat/certs/root-ca.pem</ca>
    </certificate_authorities>
    <certificate>/etc/filebeat/certs/filebeat.pem</certificate>
    <key>/etc/filebeat/certs/filebeat-key.pem</key>
  </ssl>
</indexer>

systemctl daemon-reload
systemctl enable wazuh-manager
systemctl start wazuh-manager
systemctl status wazuh-manager

systemctl daemon-reload
systemctl enable filebeat
systemctl start filebeat
filebeat test output

elasticsearch: https://127.0.0.1:9200...
  parse url... OK
  connection... OK
  handshake... OK
  version: 7.10.2

🖥️ Próximos Passos
 Repetir os passos para todos os nós do cluster

 Configurar o cluster Wazuh (próxima etapa)

 Instalar o Painel Web do Wazuh

 # Execute após instalar todos os componentes

📚 Referências
Documentação oficial do Wazuh

GitHub do Wazuh

Downloads do Wazuh


---
