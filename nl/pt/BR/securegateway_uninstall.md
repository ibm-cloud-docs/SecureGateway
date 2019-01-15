---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Desinstalando o cliente

## Docker
{: #docker}

Para remover a imagem do Docker do cliente {{site.data.keyword.SecureGateway}}, execute o comando a seguir:

```
docker rmi ibmcom/secure-gateway-client
```
{: pre}

## Mac OS X
{: #mac}

Para remover o cliente {{site.data.keyword.SecureGateway}} do Mac OS X, execute os comandos a seguir no terminal:

```
cd /Applications
rm -rf ./ibm
```
{: codeblock}

## Linux
{: #linux}

### Ubuntu/PowerPC
{: #debian-uninstall}

Para localizar o status de instalação do cliente {{site.data.keyword.SecureGateway}},
é possível usar o comando a seguir:

```
sudo dpkg -l | grep ibm-securegateway-client
```
{: pre}

Para desinstalar o cliente {{site.data.keyword.SecureGateway}} sem remover o diretório de instalação, os arquivos de configuração, o ID, o ID do grupo
e os arquivos de log, use o seguinte:

```
sudo dpkg -r ibm-securegateway-client
```
{: pre}

Para desinstalar o cliente {{site.data.keyword.SecureGateway}} removendo tudo, use o seguinte:

```
sudo dpkg --purge ibm-securegateway-client
```
{: pre}

### RedHat/SuSE
{: #rpm-uninstall}

Para localizar o status de instalação do cliente {{site.data.keyword.SecureGateway}},
é possível usar o comando a seguir:

```
rpm -qa | grep ibm-securegateway-client
```
{: pre}

Para desinstalar o cliente {{site.data.keyword.SecureGateway}} sem remover o diretório de instalação, os arquivos de configuração, o ID, o ID do grupo
e os arquivos de log, use o seguinte:

```
rpm -e ibm-securegateway-client
```
{: pre}

Para desinstalar o cliente {{site.data.keyword.SecureGateway}} removendo tudo, use o seguinte:

```
yum remove ibm-securegateway-client
```
{: pre}

## Windows
{: #windows}

Há duas maneiras de remover a instalação do Windows para o cliente {{site.data.keyword.SecureGateway}}:

1. Removendo do diretório de instalação: acesse o diretório de instalação para o cliente {{site.data.keyword.SecureGateway}} e clique duas vezes no uninstall.exe.

2. Removendo do Painel de Controle: remova o aplicativo {{site.data.keyword.SecureGateway}} da seção Programas do Painel de Controle.
