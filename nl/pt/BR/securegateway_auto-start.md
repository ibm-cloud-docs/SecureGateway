---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Configurando a autoinicialização para o cliente
{: #auto-start-conf}

## Linux
{: #auto-start-linux}

Se você escolheu usar o recurso de autoinicialização do sistema, use um dos métodos abaixo para iniciar o cliente.  O arquivo de configuração que será usado pelo cliente pode ser localizado em:

```
/etc/ibm/sgenvironment.conf
```
{: screen}

<b>Nota:</b> a funcionalidade de autoinicialização não está disponível para o Red Hat Linux 6.

Esse arquivo inclui as variáveis importantes a seguir a serem configuradas:

| Variável de ambiente | Descrição       |
| ------------- | ----------- |
| RESTART_CLIENT | Parar e reiniciar o cliente durante a instalação ou o upgrade (Sim ou Não) |
| GATEWAY_ID | Seu ID do gateway conforme criado na IU do Secure Gateway for Bluemix |
| SECTOKEN | Token de segurança para esse ID de gateway, se você escolher impor segurança ao criar o gateway |
| ACL_FILE | Arquivo de Lista de Controle de Acesso que você deseja usar para restringir o acesso no local aos recursos |
| LOGLEVEL | O nível de log que você deseja configurar para seu serviço (o padrão é INFO) |
| USE_UI   | Configure isso como 'N' se não desejar ativar a IU do cliente |
| UI_PORT  | A porta na qual você deseja ativar a IU do cliente (o padrão é 9003) |
| LANGUAGE | O idioma no qual você deseja que o cliente efetue login (o padrão é en) |

<b>Nota:</b> esse arquivo será lido somente se você estiver usando o recurso de autoinicialização do sistema.  Se você estiver executando o cliente manualmente, esse arquivo será ignorado.

### Upstart
{: #auto-start-upstart}

### Iniciando o cliente
{: #upstart-start}

Se estiver usando o recurso upstart para que o cliente Secure Gateway seja executado automaticamente na inicialização do sistema, você precisará verificar sua configuração primeiro (/etc/ibm/sgenambiment.conf).  Depois de ter configurado o serviço upstart, será possível usar o comando a seguir para iniciá-lo:

```
sudo initctl start securegateway_client
```
{: pre}

Para reiniciar o serviço:

```
sudo initctl restart securegateway_client
```
{: pre}

### Parando o cliente
{: #upstart-stop}

Para parar o serviço, execute o comando a seguir:

```
sudo initctl stop securegateway_client
```
{: pre}

### SystemD
{: #auto-start-systemd}


### Iniciando o cliente
{: #systemd-start}

Se estiver usando o recurso systemD para que o cliente Secure Gateway seja executado automaticamente na inicialização do sistema, você talvez precise verificar sua configuração primeiro (/etc/ibm/sgenvironment.conf).  Depois de ter configurado o serviço upstart, será possível usar o comando a seguir para iniciá-lo:

```
systemctl start securegateway_client
```
{: pre}

Para reiniciar o serviço:

```
systemctl restart securegateway_client
```
{: pre}

Para obter o status do serviço:

```
systemctl status securegateway_client
```
{: pre}

### Parando o cliente
{: #systemd-stop}

Para parar o serviço, execute o comando a seguir:

```
systemctl stop securegateway_client
```
{: pre}

### System V
{: #auto-start-system-v}

O System V não é configurado durante a instalação, como as outras instalações de autoinicialização. Os Scripts estão disponíveis no diretório de instalação /opt/ibm/securegateway/client/upstart, eles são:

```
secgw.sh
securegateway_clientd
```
{: screen}

<b>Nota:</b> esta seção afetará os usuários do SuSE/SLES 11.

Opcional: se você estiver executando o SuSE versão 11 que usa a autoinicialização do
System V, serão fornecidos scripts que podem ser usados para configurar esse processo. Siga o procedimento abaixo:

```
cd /opt/ibm/securegateway/client/upstart/systemV
cp secgw.sh /usr/local/bin
chmod +x /usr/local/bin/secgw.sh
cp securegateway_clientd /usr/local/bin
chmod +x /usr/local/bin/securegateway_clientd
cd /etc/init.d
ln -s /usr/local/bin/securegateway_clientd
insserv securegateway_clientd
vi /etc/ibm/sgenvironment.conf
```
{: codeblock}

Depois que essas etapas tiverem sido executadas, os comandos do YasT e do System V poderão ser usados
para iniciar/parar o daemon.

Retorne para [Introdução - Incluindo um cliente](/docs/services/SecureGateway/securegateway_client.html).

## Windows
{: #auto-start-windows}

Para alterar o estado do serviço do Windows, abra uma janela de comando com privilégios de administrador.

```
cd "<Installation_directory>\ibm\securegateway\client"
```
{: pre}

Se o serviço já estiver em execução, o usuário poderá usar o comando a seguir para pará-lo

```
windowsService.cmd uninstall
```
{: pre}

Para iniciar o serviço, use o comando a seguir

```
windowsService.cmd install
```
{: pre}

<b>Nota:</b> o serviço será executado com base nas configurações armazenadas em:

```
%Installation_directory%/ibm/securegateway/client/securegw_service.config
```
{: pre}

Os logs do aplicativo para o serviço do Windows serão armazenados em:

```
%Installation_directory%/ibm/securegateway/client/logs/securegw_win_service.log
```
{:pre}

 Os logs são movimentados para um novo arquivo diariamente.

Retorne para [Introdução - Incluindo um cliente](/docs/services/SecureGateway/securegateway_client.html).
