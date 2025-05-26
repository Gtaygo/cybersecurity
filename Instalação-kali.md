# 🐱‍💻 Guia de Instalação do Kali Linux

## ✅ Requisitos de Sistema

Os requisitos de instalação do Kali Linux variam de acordo com sua finalidade:

- 🔹 **Instalação mínima (sem interface gráfica)**  
  - RAM: 128 MB (512 MB recomendado)  
  - Espaço em disco: 2 GB  
  - Uso: servidor SSH básico

- 🔹 **Instalação padrão com desktop Xfce4 e metapacote `kali-linux-default`**  
  - RAM: mínimo 2 GB  
  - Espaço em disco: mínimo 20 GB

- 🔹 **Uso avançado com ferramentas pesadas (ex: Burp Suite)**  
  - RAM: mínimo 8 GB  
  - Recomendado para grandes aplicações web ou multitarefa

---

## 🛠️ Pré-Requisitos

Este guia assume as seguintes condições:

- Imagem **amd64 Installer** do Kali Linux
- Suporte a inicialização por **CD/DVD ou USB**
- Instalação em **disco único**
- Rede com **DHCP e DNS ativos**, com acesso à internet
- **Secure Boot desabilitado** no UEFI (o kernel do Kali não é assinado)

---

## 📥 Etapas de Instalação

1. **Baixe o Kali Linux**  
   Recomendado: [Imagem Installer](https://www.kali.org/get-kali/)

2. **Crie a mídia de instalação**  
   - Grave a ISO em um DVD **ou**
   - Crie um pendrive bootável com ferramentas como [Rufus](https://rufus.ie/)

3. **Backup dos dados**  
   Faça backup de todos os arquivos importantes em um local seguro.

4. **Configure o boot no BIOS/UEFI**  
   - Inicialize a partir do CD/DVD/USB  
   - Desative o **Secure Boot**

---

## 🚀 Instalação Passo a Passo

1. **Inicie com a mídia escolhida**  
   - Escolha **Instalação Gráfica** na tela de boot

2. **Escolha o idioma**  
   - Ex: Português

3. **Defina o nome do host**  
   - Ex: `kali`

4. **Configuração de rede (opcional)**  
   - Se não houver DHCP, configure manualmente ou pule

5. **Informe o nome de usuário**

6. **Defina uma senha segura**

7. **Particionamento do disco**  
   - Para iniciantes, escolha **usar disco inteiro**

8. **Esquema de particionamento**  
   - Recomendado: **Tudo em uma única partição**

9. **Rede pode ser pulada**  
   - Será configurada mais tarde, se necessário

10. **Escolha dos softwares**  
    - Marque:
      - Primeira opção (Desktop Xfce)
      - Terceira opção (kali-linux-default)
      - Três últimas opções (como ferramentas adicionais)

11. **Concluir a instalação**  
    - Aguarde a instalação terminar  
    - Remova a mídia e reinicie

---

## 📌 Pós-Instalação

- Atualize o sistema:
  ```bash
  sudo apt update && sudo apt upgrade -y

sudo nmtui


---
