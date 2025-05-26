# 🧰 Guia Completo: Instalação e Configuração do Windows Server 2008

---

## 🔹 Adquirindo e Configurando o SO no VirtualBox

1. Baixe a ISO do **Windows Server 2008** pelo site oficial da Microsoft.
2. No **VirtualBox**, clique em **"Novo"** para criar a máquina virtual.
3. Dê um nome à máquina, selecione a ISO e defina:
   - Tipo: Microsoft Windows
   - Versão: Windows 2008 (32 ou 64 bits)
4. Defina a quantidade de **memória RAM** (preferencialmente na faixa verde).
5. Configure o **disco rígido**:
   - Criar novo, usar existente ou nenhum.
   - Escolher tipo de disco (VDI, VHD, etc).
   - Escolha **"Dinamicamente alocado"**.
   - Defina o **tamanho do disco** (ou mantenha o padrão).
6. Clique em **"Finalizar"** para concluir a criação da VM.

---

## 🪟 Instalando o Windows Server 2008

1. Inicie a máquina virtual. Se necessário, selecione a ISO manualmente.
2. Escolha o **idioma** (Português ABNT2 recomendado).
3. Selecione a versão de **32 ou 64 bits**.
4. Aceite os **termos de licença**.
5. Escolha **Instalação Avançada**.
6. Avance nas próximas telas e aguarde a instalação.
7. Defina:
   - Nome da máquina
   - Nome de usuário
   - Senha de administrador

---

## 🌐 Instalação e Configuração do DNS

### Instalação:
1. Vá em **Iniciar → Ferramentas Administrativas → Server Manager**.
2. Vá em **Funções do Servidor → Adicionar Função**.
3. Selecione **DNS Server** e prossiga com a instalação.
4. Após instalado, acesse novamente via **Ferramentas Administrativas → DNS**.

### Configuração:
#### 🔸 Zona Direta:
- Clique com o botão direito em **"Zona de pesquisa direta"** → **Nova Zona**.
- Selecione **Zona Primária** e nomeie como `exemplo.local`.

#### 🔸 Registro A:
- Clique com o botão direito em `exemplo.local` → **Novo Host (A ou AAAA)**.
- Informe o nome do host e IP.

#### 🔸 CNAME:
- Clique com o botão direito em `exemplo.local` → **Novo Alias (CNAME)**.
- Atribua nome original e nome alternativo.

#### 🔸 MX:
- Clique com o botão direito → **Novo Mail Exchanger (MX)**.
- Deixe "Host or child domain" vazio e insira o FQDN (ex: `mail.empresa.local`).

#### 🔸 Zona Inversa:
- Clique com o botão direito em **"Zona de pesquisa inversa"** → Nova Zona.
- Relacione com a rede e vincule à zona direta criada.

---

## 🔧 Testes de DNS

- `nslookup servidor.empresa.local` – Verifica registro A.
- `nslookup -type=mx empresa.local` – Lista servidores MX.
- `dig empresa.local MX` (requer instalação do BIND no Windows).

---

## 📡 Instalando e Configurando o DHCP

1. Vá em **Iniciar → Ferramentas Administrativas → Server Manager**.
2. Clique em **Funções → Adicionar Função**.
3. Selecione **DHCP Server** e clique em Próximo.
4. Escolha sua **interface de rede**.
5. Configure:
   - Nome do domínio
   - Servidor DNS (use o DNS configurado anteriormente)
   - Servidor WINS (se necessário)
6. Configure escopo:
   - Intervalo de IPs (início/fim)
   - Máscara
   - Gateway
   - Tipo de sub-rede
7. Ative e finalize a instalação.
8. Após instalado, acesse novamente **DHCP** nas ferramentas.
9. Personalize escopos clicando com o botão direito em **IPv4/IPv6 → Escopo**.

### Teste:
```bash
ipconfig /all
ping 192.168.0.1
##
````
## 🏢 Instalando e Configurando o Active Directory (AD)
Com o DNS e DHCP configurados, vá em:

Iniciar → Ferramentas Administrativas → Server Manager

Adicione a função: Active Directory Domain Services

Após instalação, execute dcpromo.exe pela barra de aviso ou via "Executar".

Selecione "Criar novo domínio", ex: cuscuz.local.

Escolha a versão do Windows.

Habilite o DNS e clique em "Sim" nas mensagens de alerta.

Defina a senha de administrador do AD.

Finalize e reinicie o sistema.

Acessar o AD:
Vá em Ferramentas Administrativas → Active Directory Users and Computers

Adicione usuários: clique com botão direito em Users → Novo → Usuário

nslookup          # Verifica resolução de nomes e servidor DNS padrão
start //cuscuz.local   # FQDN do domínio
ipconfig /all     # Verifica IP e configurações de rede
ping [IP]         # Testa conectividade


---

