---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-11"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Instalando o cliente

## Docker
{: #docker}

Docker é uma plataforma
de terceiro que fornece uma abordagem de contêiner para instalar aplicativos
de forma fácil e rápida com a necessidade de pouca ou nenhuma configuração. O serviço {{site.data.keyword.SecureGateway}} fornece uma imagem do Docker a ser usada após a instalação
do utilitário Docker em sua estação de trabalho.  Para instalar o Docker, veja o website de instalação do Docker e siga as instruções para seu sistema.

### Instalando e atualizando o cliente
{: #docker-install}

A instalação e a atualização da imagem do Docker do cliente {{site.data.keyword.SecureGateway}} usam o mesmo comando:

```
docker pull ibmcom/secure-gateway-client
```
{: pre}

### Iniciando uma sessão interativa do cliente
{: #docker-run}

Para executar o contêiner do Docker para o IBM {{site.data.keyword.SecureGateway}}, emita o comando a seguir:

```
docker run -it ibmcom/secure-gateway-client <gateway ID> -t <security token>
```
{: pre}

Normalmente, ele usa dois parâmetros, seu ID do gateway do {{site.data.keyword.SecureGateway}} e o token de segurança do gateway, ambos disponíveis por meio do Painel do {{site.data.keyword.SecureGateway}}.

### Comandos do Docker suportados
{: #docker-commands}

O cliente {{site.data.keyword.SecureGateway}} suporta somente os comandos `pull` e `run` para manipular o contêiner.

Retorne para [Introdução - Incluindo um cliente](./securegateway_client.html).

## Mac OS X
{: #mac}

### Requisitos para execução no Mac OS X
{: #mac-requirements}

Os pré-requisitos a seguir são necessários antes de executar o cliente no Mac OS X.
 1. Instale as ferramentas de desenvolvimento do Xcode, pois elas são requeridas por alguns dos módulos do nó nessa plataforma.

### Procedimento de instalação do Mac OS X
{: #mac-install}

Você pode requerer privilégios administrativos para executar essa instalação, dependendo da configuração de segurança de seu sistema.

 1. Monte a imagem DMG que foi transferida por download por meio da IU do {{site.data.keyword.SecureGateway}}, normalmente 'clicando duas vezes' nela.
 2. Uma nova janela 'localizador' deve aparecer. Essa janela deve conter um ícone "atalho" de Aplicativos, arraste e solte o aplicativo no atalho. Se não, 'clique duas vezes' no volume montado e arraste e solte o ícone do aplicativo para o ícone Aplicativos na barra lateral do Localizador.

### Iniciando uma sessão interativa do cliente
{: #mac-run}

Para iniciar o cliente, execute o arquivo `secgw.command` localizado no local de instalação padrão: `/Applications/ibm/`.

Retorne para [Introdução - Incluindo um cliente](./securegateway_client.html).

## Linux
{: #linux}

A instalação inclui o cliente {{site.data.keyword.SecureGatewayfull}}, bem como uma versão segura do pacote nodejs da IBM. Ambos são instalados sob o diretório /opt/ibm no sistema. O instalador criará ou atualizará o seguinte:

| Diretório | Nome de arquivo | Descrição          |
| ------------- | ------------- | ----------- |
| /etc/ibm | sgenvironment.conf | arquivo de configuração para upstart |
| /etc/ibm | passwd | cria o usuário: secgwadmin:x:501:501::/home/secgwadmin:/bin/bash |
| /etc/ibm | group | cria o grupo: secgwadmin:x:501: |
| /etc/init | securegateway_client.conf | arquivo upstart |
| /opt/ibm | node-v&lt;version&gt;-linux-x64 | Instalação do Node JS da IBM |
| /opt/ibm | securegateway | Instalação do cliente {{site.data.keyword.SecureGateway}} |
| /usr/local/bin | node | symlink criado para o executável do nó da IBM |
| /usr/local/bin | npm | symlink criado para o executável npm da IBM |
| /var/log/securegateway | client_console.log | arquivo de log criado ao executar o cliente como um processo de autoinicialização |

Privilégios raiz ou administrativos serão requeridos para executar essa instalação.

### Instalação do Ubuntu/PowerPC
{: #debian-install}

 1. Instale o cliente {{site.data.keyword.SecureGateway}}. Por exemplo, se você estiver usando um pacote Debian, emita o comando a seguir.

    ```
    sudo dpkg -i <secure-gateway-debian-package>
    ```
    {: pre}

 2. Quando o instalador do cliente for iniciado, você será solicitado a fornecer as informações a seguir.

    <b>Processo de autoinicialização Sim/Não</b>
        Se esse é um upgrade ou uma reinstalação, deve-se escolher se você deseja que o cliente existente seja interrompido enquanto
        o processo de instalação está em execução. Escolha S/s para parar o cliente existente. Caso contrário, o pacote do cliente
        é submetido a upgrade sem reiniciar o cliente, o que significa que é possível aguardar uma janela Atualização de software
        apropriada para executar uma reinicialização. Caso escolha N/n, a instalação continua e deve-se reiniciar o cliente
        manualmente.

    <b>ID do gateway</b>
        Configure o ID do gateway para o cliente. O ID do gateway é definido ao criar um serviço {{site.data.keyword.SecureGateway}}. Se o
        cliente falhar ao se conectar, será possível mudar sua seleção editando o arquivo .conf.

    <b>Token de segurança</b>
        Se o ID do gateway está ativado para verificar um token de segurança, deve-se fornecê-lo agora. Se o
ID do gateway não requerer um token de segurança, deixe isso em branco.

    <b>Nível de criação de log</b>
        A configuração padrão é INFO. Outros valores válidos são TRACE, DEBUG e ERROR.

    <b>Lista de Controle de Acesso</b>
        Insira o caminho absoluto do nome do arquivo que contém a lista de controle de acesso sobre o que tem acesso aos
        recursos no local. Para obter mais informações, veja o arquivo README.md em /opt/ibm/securegateway ou a Lista de controle de acesso.

    <b>IU do cliente</b>
        Escolha se você deseja usar a IU do cliente. Se sim, o usuário pode mudar a porta para a IU. O valor padrão é 9003.

    <b>Nota:</b> não é necessário responder a nenhum dos prompts, todos assumirão o padrão definido ou serão deixados em branco no arquivo sgenvironment.conf. Isso permite que o processo de instalação
seja executado sem interação com o usuário.

    <b>Nota:</b> o sgenvironment.conf é lido toda vez que você inicia ou reinicia o cliente usando o processo upstart do sistema. É possível editar o arquivo /etc/ibm/sgenvironment.conf a qualquer momento para fazer mudanças em sua configuração e reiniciar o cliente para captar essas mudanças.

    <b>Nota:</b> é possível mudar o idioma dos logs de serviço do cliente {{site.data.keyword.SecureGateway}} mudando o parâmetro `LANGUAGE` no arquivo /etc/ibm/sgenambiment.conf. Os logs de serviço mudarão para o idioma selecionado após a reinicialização do serviço.

 3. Se você reiniciou o cliente, para certificar-se de que o cliente está sendo executado corretamente, emita o comando a seguir:

    ```
    cat /var/log/securegateway/client_console.log
    ```
    {: pre}

 4. Se você desejar editar o arquivo de configuração, para certificar-se de que o arquivo de configuração esteja atualizado corretamente, emita o comando a seguir:

    ```
    sudo vi /etc/ibm/sgenvironment.conf
    ```
    {: pre}

 5. Para verificar o status da instalação do cliente, emita
o comando a seguir:

    ```
    sudo dpkg --status ibm-securegateway-client
    ```
    {: pre}

Um exemplo da saída é retornado:

```
Package: ibm-securegateway-client
Status: install ok installed
Priority: extra
Section: IBM/net
Maintainer: cloudoe-dev@webconf.ibm.com
Architecture: amd64
Source: ibm-securegateway+securegateway-client
Version: 1.7.0
Description: IBM Secure Gateway Client for Bluemix
IBM Secure Gateway client package for Bluemix, supports secure connections to on-premises connections.
Homepage: https://ng.bluemix.net/
License: http://www.ibm.com/software/sla/sladb.nsf/lilookup/986C7686F22D4D3585257E13004EA6CB?OpenDocument
```
{: screen}

### Instalação do RedHat/SuSE
{: #rpm-install}

1. Instale o cliente {{site.data.keyword.SecureGateway}}. Por exemplo, se você estiver usando um pacote RPM, emita o comando a seguir:

   ```
   rpm -ivhf <secure-gateway-rpm-package>
   ```
   {: pre}


   O instalador do cliente inicia e instala o cliente, ele criará um arquivo sgenvironment.conf em /etc/ibm.

2. Opcional: caso deseje usar o processo upstart do sistema, deve-se editar esse arquivo e fornecer o seguinte para que o cliente seja iniciado corretamente. Veja [Usando o upstart](./securegateway_auto-start.html#linux) para obter mais informações sobre a edição desse arquivo de configuração.

3. Se você iniciou o cliente usando o upstart, verifique o arquivo de log para certificar-se de que ele esteja sendo executado corretamente.

   ```
   cat /var/log/securegateway/client_console.log
   ```
   {: pre}

4. Para verificar o status da instalação do cliente, emita
o comando a seguir:

   ```
   rpm -q ibm-securegateway-client
   ```
   {: pre}

### Iniciando uma sessão interativa do cliente
{: #linux-run}

Para iniciar o cliente, execute os comandos a seguir:

```
cd /opt/ibm/securegateway/client
node lib/secgwclient.js <gateway ID> -t <security token>
```
{: codeblock}

Normalmente, ele usa dois parâmetros, um ID do gateway do {{site.data.keyword.SecureGateway}} e o token de segurança do gateway, ambos disponíveis por meio do Painel do {{site.data.keyword.SecureGateway}}.

Retorne para [Introdução - Incluindo um cliente](./securegateway_client.html).

## Windows
{: #windows}

### Instalando o cliente
{: #windows-install}

Você pode requerer privilégios administrativos para executar essa instalação, dependendo da configuração de segurança de seu sistema.

 1. Copie o arquivo do instalador EXE para o sistema.  Também é fornecida uma soma MD5 que pode ser usada para verificar a integridade do pacote de instalação.

 2. Instale-o clicando duas vezes nele e respondendo aos prompts ou executando o executável por meio do prompt de comandos.

 3. O usuário será solicitado a escolher o diretório de instalação sua preferência. O local padrão é o diretório Program Files (x86).

 4. O usuário será solicitado a selecionar o idioma no qual eles desejam ativar a Interface da Linha de Comandos. Se não for selecionado nenhum idioma, ele será padronizado para inglês.

 5. O usuário será solicitado a instalar o cliente como um serviço do Windows. Se o usuário escolher fazer isso, o cliente será executado em segundo plano como um serviço do Windows com as configurações que o usuário fornece no diálogo subsequente. O status do serviço pode ser verificado na página do serviço do Painel de Controle. O nome do serviço é "IBM Bluemix Secure Gateway Service".

 6. O instalador identifica se há uma instalação existente na máquina. Se ela for detectada, aparecerá uma pergunta ao usuário questionando se ele deseja usar as configurações existentes, caso contrário, o usuário terá a opção de inserir novas configurações. Detalhes como os IDs do gateway, os tokens de segurança, os arquivos acl e o nível de log para cada gateway podem ser fornecidos aqui. Se o usuário tiver escolhido executar o cliente como um serviço do Windows, o cliente será ativado com as configurações fornecidas. Se o usuário não tiver escolhido ativar o cliente como um serviço do Windows, as configurações serão armazenadas para uso futuro. Em qualquer caso, as configurações são armazenadas em %Installation_directory%/ibm/securegateway/client/securegw_service.config.

 7. Iniciando com a versão 1.5.0, o cliente é fornecido com uma IU do cliente totalmente funcional. É possível configurá-lo por meio do próprio instalador. O usuário pode fornecer a senha e o número da porta (o padrão é 9003) por meio dos quais a IU do cliente será ativada.

 Se o usuário escolher ativar o cliente com conexão a múltiplos gateways, tome cuidado ao fornecer os valores de configuração. Os IDs do gateway precisam ser separados por espaços. Os tokens de segurança, os arquivos acl e os níveis de log devem ser delimitados por --. Se você não desejar fornecer nenhum desses três valores para um determinado gateway, use 'none'.

 <b>Nota:</b> assegure-se de que não haja espaços em branco residuais.

 <b>Nota:</b> observe que os arquivos acl devem ser colocados no diretório %Installation_directory%/ibm/securegateway/client ou relativo a esse caminho.

### Iniciando uma sessão interativa do cliente
{: #windows-run}

Para iniciar o cliente, abra um prompt de comandos com privilégios administrativos e execute os comandos a seguir:

```
cd "<Installation_directory>\ibm\securegateway\client"
secgw.cmd
```
{: codeblock}

Alternativamente, navegue para `<Installation_directory>\ibm\securegateway\client` e clique duas vezes em `secgw.cmd`.

<b>Nota:</b> é possível escolher usar as configurações armazenadas no arquivo `<Installation_directory>\ibm\securegateway\client\securegw_service.config` ou fornecer os detalhes interativamente.

Retorne para [Introdução - Incluindo um cliente](./securegateway_client.html).

## DataPower
{: #datapower}

O DataPower tem uma versão integrada do cliente {{site.data.keyword.SecureGateway}}. Dependendo da versão do DataPower, você pode ter uma versão diferente do cliente {{site.data.keyword.SecureGateway}}.  Esteja ciente de quaisquer [Limitações do cliente DataPower](./securegateway_interaction.html#limits-datapower) aplicáveis. O uso do cliente Secure Gateway antigo pode apresentar erros inesperados.

| Versão do DataPower | Versão do cliente {{site.data.keyword.SecureGateway}} |
| -- | --  |
| 7.2.0.0, 7.5.0.0 | 1.1.0  |
| 7.5.1.0, 7.7.0 | 1.4.2  |
| 7.5.2.4 | 1.6.1  |
| 7.5.2.6, 7.6.0.0 | 1.7.0  |
| 7.5.2.14, 7.6.0.7, 7.7.1.0 |  1.8.0fp6  |

### Iniciando uma sessão do cliente
{: #datapower-run}

1. Efetue login na GUI da web do DataPower
2. Navegue para Objetos -> Nuvem -> Cliente Secure Gateway ou procure o cliente Secure Gateway
3. Clique em `Add` para configurar uma nova conexão do cliente
4. Forneça um Nome, o ID do gateway e o Token de segurança (se aplicável) e, em seguida, aplique as mudanças.

Retorne para [Introdução - Incluindo um cliente](./securegateway_client.html).
