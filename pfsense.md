# Instalação do pfSense 2.4.4 no VirtualBox

## Etapa 1: Baixe o pfSense
Vamos usar o VirtualBox para instalar o **pfSense 2.4.4**.  
Certifique-se de baixar o arquivo de imagem de CD (ISO) em:  
👉 [https://www.pfsense.org/download/](https://www.pfsense.org/download/)

---

## Etapa 2: Criando a VM

**Passo 1: Configurações iniciais**
- **Memória:** 1024 MB (ou mais, se desejar)
- **Disco rígido:** Criar disco rígido virtual agora
- **Tipo de arquivo:** VDI
- **Armazenamento:** Alocado dinamicamente

**Passo 2: Tamanho e Local**
- **Tamanho:** 8 GB
- Escolha o local de armazenamento do disco.

---

## Etapa 3: Configurar as Interfaces de Rede

Vamos adicionar **mais 2 interfaces de rede** para simular um ambiente mais realista.

1. Clique com o botão direito na VM criada > **Configurações**.
2. Vá até a guia **Rede**.
   - No **Adaptador 1**, mude de `NAT` para `Rede Interna`.
   - Se quiser acessar o pfSense a partir do host, use **Adaptador somente para host**.
3. Ative **Adaptador 2** e **Adaptador 3** como `Rede Interna`.
4. Clique em **OK**.

> ⚠️ Com "Rede Interna", você precisará de outra VM (como Ubuntu) para acessar o pfSense via navegador.

---

## Etapa 4: Inicializar a VM

1. Inicie a VM.
2. Quando solicitado, selecione o ISO:  
   `pfSense-CE-memstick-2.4.4-DEVELOPMENT-amd64-latest.iso`

---

## Etapa 5: Processo de Instalação

1. Pressione **Enter** na tela de aviso de direitos autorais.
2. Escolha o **mapa de teclado** (ex: `br-abnt2`) e pressione Enter.
3. Selecione `Automático (UFS)` como método de particionamento.
4. Após a instalação, escolha **Não** para configuração manual.
5. Confirme a **reinicialização**.

---

## Etapa 6: Remover a Imagem ISO

1. Vá até **Dispositivos > Unidades ópticas > Remover disco da unidade virtual**
2. Confirme com **Forçar desmontagem**
3. Em **Máquina > Redefinir**, reinicie a VM

---

## Etapa 7: Acesso ao pfSense

Na primeira inicialização:

- Aguarde a detecção e configuração automática das interfaces.
- Quando o menu aparecer, anote o IP da interface LAN (geralmente `192.168.1.1`).
- Com uma segunda VM (na mesma `Rede Interna`), abra o navegador e acesse:

http://192.168.1.1

- Use o **assistente de configuração** do pfSense para finalizar.

---

## Etapa 8: (Opcional) Acesso à Internet

Para dar acesso à Internet via pfSense:

- Configure o **Adaptador 1** como `Placa em modo Bridge (Bridged Adapter)`
- O pfSense obterá um IP automaticamente pelo DHCP da sua rede física.

---

## 📌 Observações Finais

- Esse ambiente é seguro para testes, pois isola sua rede real.
- Você pode adicionar outras VMs para testar regras de firewall, VPN, NAT, etc.

---

## 🔧 Requisitos

- VirtualBox instalado
- ISO do pfSense 2.4.4
- Conhecimentos básicos de redes

---

## 📚 Referências

- [Documentação oficial do pfSense](https://docs.netgate.com/pfsense/en/latest/)
- [Download do pfSense](https://www.pfsense.org/download/)
