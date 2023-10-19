# Desafio 2 - Crie uma máquina virtual 

**Abra seu Virtual Box para dar inicio a esse passo**

1. Clique no botão **NOVO** ou **NEW** na barra superior do lado direito.
2. Assim que clicar irá surgir um nova janela para nomear a sua máquina virtual, pode nomear como "Oracle Linux"
3. Na Opção **IMAGEM ISO**, selecione a o arquivo ISO que baixou 
4. Por último clique em **Próximo**, como na imagem abaixo:

![VM Instructions](./images/vm-instruction.png)

## Configurando a sua máquina virtual
1. Logo depois que clicar em Próximo no último item do passo anterior
2. Coloque um nome de **usuário** e **senha**
3. Na opção **NOME DO SERVIDOR**, coloque um nome simples sem espaços só para identificar o seu servidor, e clique em **Próximo**.

![settings](./images/settings-vm.png)

## Hardware
1. Selecione **4096MB**, que equivalem a **4GB** 
2. Clique em **Próximo**

![Vm RAM](./images/VM-RAM.png)

### Disco rigido
- Deixe selecionado a opção **CRIAR UM NOVO DISCO RÍGIDO VIRTUAL AGORA**
- No item **TAMANHO DO DISCO**, selecione no mínimo **12GB**
- Clique em **PROXIMO**
- E **FINALIZAR**

![disk](./images/disk-vm.png)

## Adicione o SO na Virtual Machine
1. Selecione a opção **CONFIGURAÇÕES** na barra superior a direita, isso vai abrir um menu de opções.
2. Selecione a opção **ARMAZENAMENTO**, clique no espaço vazio abaixo de **Controladora:IDE**
3. Clique no icone de **CD**, no item **DRIVE OPTICO**, e selecione a Imagem ISO que baixou, no meu caso está salvo como *"OracleLinux-R8-U8-X86_64-dvd.iso"*, em seguida clique em **OK**.

![storage](./images/storage-vm.png)

### Instalando o Linux na VM
1. No canto superior a direita, clique em **INICIAR**, isso vai fazer com que inicie a VM e comece a instalação do Linux
2. Pressione **Enter**
3. Aguarde finalizar a instalação

![instalation](./images/starting-vm.png)

### Configurando 
- Selecione o Idioma e **CONTINUAR**
- Depois será redirecioado para a tela de **RESUMO DE INSTALAÇÃO**, que contém diversas opções de configuração diferentes. Vamos atualizar cada um deles, especialmente as opções com marcas de alerta, antes de iniciar o processo de instalação.

![config](./images/config.png)

- Na Seção **SISTEMA** selecione **REDE E NOME DO HOST**, e o ligue depois clique em **PRONTO**
- Ainda na seção **SISTEMA**, selecione **DESTINO DA INSTALAÇÃO**, selecione o disco, e na seção **CONFIGURAÇÃO DE ARMAZENAMENTO**, deixe marcada a opção **AUTOMÁTICA**
- Na seção **SISTEMA** selecione **KDUMP**, e selecione a opção **ATIVAR KDUMP**, e pronto.
- A opção mais importante na seção **PROGRAMA** é a opção **FONTE DE INSTALAÇÃO**, clique nessa opção e selecione a opção **MÍDIA DE INSTALAÇÃO DETECTADA AUTOMATICAMENTE**. Depois disso, clique no botão **VERIFICAR**.
- Abra a opção **Seleção de programas** na seção **Programas**
 - Selecione a opção **MINIMAL INSTALL**, e selecione a opção **FERRAMENTAS DE DESENVOLVIMENTO** ou **Development tools** no menu a direita, por fim clique em pronto, está opção instalará uma versão sem ***Interface Gráfica***. 

![software](./images/software-wogui.png)

 8. Agora, na Seção **CONFIGURAÇÕES DO USUARIO**, selecione a opção **SENHA ROOT**, e coloque uma senha, depois clique em **PRONTO**.
 9. Depois de atualizar todas essas informações pode dar inicio a instalação do Software, no botão **INICIAR A INSTALAÇÃO** e aguarde a instalação ser feita. 
 10. Quando finalizado a instalção clique em **REINICIAR O SISTEMA**.

 ## Configurar IP Fixo
 ### Depois de iniciar a máquina faça login com seu usuário e senha e se localize

 ![login](./images/login01.png)

 **Primeiro é importante verificar o IP que a máquina está utilizando**

 - Use o comando 
 ```
  ip address
 ```

 - Isso irá retorna algo como:

 ![verifyIP](./images/verifyip02.png)

 - Depois teste a rede usando o comando:

***O -c5 foi usado para enviar apenas cinco pacotes de ping***
 ```
 ping -c5 8.8.8.8
 ```

### Precisamos editar o arquivo de configuração da interface de rede
 - Primeiro vamos buscar o nome desse arquivo:

 *Use o comando:*
 ```
 ls /etc/sysconfig/network-scripts/
 ```
- Agora que sabemos o nome do arquivo vamos edita-lo:

*Use o comando, para entrar no modo de edição do vim (Pode ser pedido a sua senha de usuário):*
```
sudo vi /etc/sysconfig/network-scripts/ifcfg-NOME_DA_SUA_INTERFACE
```

![network-file](./images/networkfile03.png)

- Quando estiver dentro do arquivo aperte **I** para entrar no modo **INSERT**

![editfile](./images/editfile04.png)

- Edite o seu arquivo com as novas informações:
```
BOOTPROTO=static
IPADDR=SEU_IP
NETMASK=SUA_MASCARA
GATEWAY=SEU_GATEWAY
```

- Depois de editar, *Aperte Esc (para entrar no modo comando), e use o comando para sair e salvar:*

```
:wq
```
- Reinicie o serviço de rede para aplicar as alterações *(Pode ser pedido a senha)*:
```
sudo systemctl restart NetworkManager
```
- Verifique se as novas informações foram atualizadas:

*Use o comando:*

```
ip address
```
- Voce pode verificar que o endereço IP foi atualizado em ***Inet***

![restart-network](./images/restart06.png)