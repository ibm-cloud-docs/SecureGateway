---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Lista de controle de acesso
{: #acl}

O cliente {{site.data.keyword.SecureGateway}} fornece suporte à Lista de Controle de Acesso (ACL) integrada. É possível permitir ou restringir (negar) acesso aos recursos no local, fazendo modificações na ACL para o cliente.  Isso pode ser feito interativamente usando os comandos do cliente ou especificando um arquivo que contenha as ACLs que você deseja ter em vigor.

A partir da v1.5.0, as regras da Lista de Controle de Acesso serão sincronizadas em todos os clientes conectados ao mesmo gateway.  Com isso, é necessário estabelecer/atualizar sua ACL somente em um único cliente e ela será compartilhada por todos os clientes em execução conectados a esse gateway.  A ACL também persistirá entre as sessões, de modo que a conexão de um novo cliente também aplicará as mesmas regras de ACL.

## Comandos da Lista de Controle de Acesso
{: #acl-commands}

Os comandos da ACL suportados são:

```
acl allow [<hostname>]:[<port>]
acl deny [<hostname>]:[<port>]
no acl [<hostname>]:[<port>]
no acl
show acl
```
{: screen}

Os formulários nos quais você omitiu um nome do host ou porta pressupõem todos os nomes de host ou portas.  Por exemplo, a seguir está uma regra de ACL para permitir todos os nomes de host para a porta 22.

```
acl allow :22
```
{: pre}

A seguir está outra regra de ACL de exemplo para permitir todos os nomes de host para todas as portas, desativando essencialmente o suporte à ACL. <b>Isso não é recomendado.</b>

```
acl allow :
```
{: pre}

O comando `show acl` mostrará a ACL atualmente configurada ou fornecerá uma mensagem sobre a configuração geral.

Retorne para [Introdução - Incluindo um cliente](/docs/services/SecureGateway?topic=securegateway-add-client).

## Controle de rota de HTTP/S usando a ACL
{: #acl-route-control}

A partir da v1.6.0, os destinos de HTTP/S também podem cumprir rotas específicas nas entradas ACL.  Eles são incluídos da mesma maneira que as entradas ACL típicas, mas com o caminho anexado ao final da regra. Por exemplo, o seguinte permitirá somente solicitações que seguem o caminho /my/api para passar por:

```
acl allow localhost:80/my/api
```
{: pre}

Com essa regra em vigor, será permitida a passagem das solicitações para `<cloud host>:<cloud port>/my/api*`.

As rotas são suportadas somente em comandos `acl allow`.

## Precedência da Lista de Controle de Acesso
{: #acl-precedence}

Depois de fornecer um comando `acl allow :`, se outros comandos `acl allow` forem inseridos, a regra de permissão `ALL:ALL` (de `acl allow :`) será removida da lista com a suposição de que você não deseja mais permitir acesso sem limites.  Depois de fornecer um comando `acl deny:`, se outro comando `acl deny` for inserido, a regra de negação `ALL:ALL` (de `acl deny :`) será removida da lista com a suposição de que você não deseja mais restringir todo o acesso.  Se você listar suas regras de ACL atuais pelo comando `show acl` na CLI, haverá um indicador para exibir se as regras não listadas estão sendo permitidas ou negadas.

## Importando um arquivo ACL
{: #import-acl-file}

É possível fornecer um nome de arquivo para o comando `acl file` que contém os comandos ACL suportados que serão lidos pelo cliente na inicialização. Esse arquivo deve ter comandos do formato a seguir:

```
acl allow [<hostname>]:[<port>]
acl deny [<hostname>]:[<port>]
no acl [<hostname>]:[<port>]
no acl
```
{: screen}

<b>Nota:</b> `no acl` sem nenhum outro parâmetro RECONFIGURA a tabela ACL e configura o acesso para DENY ALL.

É possível localizar um arquivo ACL de amostra [aqui](/docs/services/SecureGateway?topic=securegateway-acl-files).

Retorne para [Introdução - Incluindo um cliente](/docs/services/SecureGateway?topic=securegateway-add-client).

## Copiando seu arquivo ACL para o cliente Docker do {{site.data.keyword.SecureGateway}}
{: #copy-acl-to-docker}

O cliente do docker do {{site.data.keyword.SecureGateway}} é executado essencialmente em seu próprio contêiner de virtualização.  O sistema de arquivos da máquina de hospedagem não é, portanto, acessível diretamente para processos que são executados dentro do contêiner, incluindo o cliente {{site.data.keyword.SecureGateway}}.  A partir da versão 1.8.0 do Docker Engine, é possível
usar o comando 'docker cp' para importar arquivos existentes em seu host para o contêiner enquanto ele
está em execução ou parado. Isso deve ser feito para usar o comando interativo ACL FILE do cliente
{{site.data.keyword.SecureGateway}}.

Para usar o suporte 'cp' interativo no docker de seu host para a instância do docker, deve-se estar no docker 1.8.0. Isso pode ser verificado usando `docker --version`

Depois de ter feito isso, sua versão deve ser exibida como a seguir. É recomendado que você permita que o docker seja executado como usuário não raiz, portanto, execute o comando que é sugerido depois de fazer o upgrade do mecanismo para 1.8.0 ou 1.8.2.

```
Client:
 Version:      1.8.2
 API version:  1.20
 Go version:   go1.4.2
 Git commit:   0a8c2e3
 Built:        Thu Sep 10 19:21:21 UTC 2015
 OS/Arch:      linux/amd64

Server:
 Version:      1.8.2
 API version:  1.20
 Go version:   go1.4.2
 Git commit:   0a8c2e3
 Built:        Thu Sep 10 19:21:21 UTC 2015
 OS/Arch:      linux/amd64
```
{: screen}

Em seguida, para importar a lista de arquivos acl para a imagem do docker, siga estas etapas:

- Execute o comando 'docker ps' para localizar seu ID do contêiner

```
CONTAINER ID IMAGE                        COMMAND                CREATED        STATUS  PORTS NAMES
764aadce386b ibmcom/secure-gateway-client "node lib/secgwclient" 27 seconds ago Up 26 seconds condescending_nobel
```
{: screen}

- Copie seu acl.list com o comando 'docker cp' usando o ID do contêiner ou o nome:

```
docker cp 01_client.list 764aadce386b:/root/01_client.list
```
{: pre}

- Em seguida, no cliente {{site.data.keyword.SecureGateway}} em execução no docker:

```
cli F /root/01_client.list

 [2015-10-01 08:12:30.091] [INFO] The current access control list is being reset and replaced by the user provided file: /root/01_client.list
 [2015-10-01 08:12:30.093] [INFO] The ACL file process accepts acl allow 127.0.0.1:27017
 [2015-10-01 08:12:30.094] [INFO] The ACL file process accepts acl allow 127.0.0.1:22
```
{: screen}

- Exiba a ACL:

```
cli> S
 -------------------------------------------------------------------
           -- Secure Gateway Client Access Control List --          

  hostname                               port                  value
  127.0.0.1                             27017                  Allow
  127.0.0.1                                22                  Allow
 -------------------------------------------------------------------
```
{: screen}

Retorne para [Introdução - Incluindo um cliente](/docs/services/SecureGateway?topic=securegateway-add-client).
