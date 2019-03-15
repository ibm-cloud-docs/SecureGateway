---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Autenticação mútua TLS do Node.js
{: #nodejs-tls-ma}

Esta amostra explicará como configurar a Autenticação mútua para ambos os lados de um destino no local: Autenticação do usuário e Autenticação do recurso.

Eu sei que o recurso ao qual desejo me conectar será hospedado na mesma máquina que o cliente Secure Gateway, que ele atenderá na porta 8999 e que requer autenticação mútua para permitir uma conexão.  Com essas informações, eu posso começar a criar meu destino.

![Autenticação mútua TLS inicial](./images/tlsMA.png?raw=true "Autenticação mútua TLS inicial")

## Autenticação do usuário
{: #tls-ma-user-auth}
Estou criando um novo aplicativo, então não tenho nenhum par certificado/chave preexistente para meu aplicativo usar. Assim, os servidores Secure Gateway gerarão automaticamente um par para mim.  Para fazer isso, eu apenas tenho que deixar o campo de upload de Autenticação do usuário vazio.  Se eu já tivesse um par que meu aplicativo estaria usando, eu faria upload do meu certificado para esse campo, em vez disso.

## Autenticação do recurso
{: #tls-ma-resource-auth}

### Autenticação no local
{: #tls-ma-on-prem-auth}
Para que o cliente Secure Gateway autentique o recurso ao qual ele está se conectando, eu tenho que fornecer ao cliente o certificado (ou cadeia de certificados) que o recurso apresentará.  Meu recurso não tem uma cadeia de certificados integral, portanto, apenas preciso fazer upload de seu certificado para o campo Autenticação no local.  Esse campo aceita até 6 arquivos de certificado separados (.pem, .cer, .der, .crt).

### Certificado de cliente e Chave
{: #tls-ma-client-cert-and-key}
Se precisar especificar como o cliente Secure Gateway se identificará para o meu recurso, eu poderei fazer upload de um certificado e uma chave aqui para que o Cliente use.  Como o Cliente e o recurso estão em execução na mesma máquina, eu posso deixá-los vazios e deixar que os servidores Secure Gateway gerem automaticamente um par para mim.  Se meu recurso estivesse em um host separado, eu precisaria [gerar um par certificado/chave para fazer upload](/docs/services/SecureGateway/securegateway_keygen.html).

![Autenticação mútua TLS local](./images/localTLSma.png?raw=true "Autenticação mútua TLS local")

Quando eu criar esse destino, os servidores Secure Gateway gerarão automaticamente um par certificado/chave para que meu aplicativo use, bem como um par para que o cliente Secure Gateway use ao conectar-se ao meu recurso local.  O destino também terá o certificado do meu recurso local para fornecer ao cliente Secure Gateway para uso na CA durante a conexão.  Após a criação, eu posso editar meu destino para ver as informações a seguir:

![Detalhes da autenticação mútua TLS local](./images/editLocalTLSma.png?raw=true "Detalhes da autenticação mútua TLS local")

## Fazendo download dos arquivos de segurança
{: #tls-ma-download-files}
No painel de informações de destino do meu destino, há um link para fazer download de um arquivo .zip contendo todos os certificados e chaves associados ao meu destino:

![Painel de informações de autenticação mútua](./images/infoPanelMA.png?raw=true "Painel de informações de autenticação mútua")

Depois de fazer download do .zip e extrair os arquivos, agora eu tenho o seguinte:

Nome do arquivo | Propósito
-- | --
secureGatewayCert.pem | Certificado primário do servidor Secure Gateway
DigiCertCA2.pem | Certificado intermediário do servidor Secure Gateway
DigiCertTrustedRoot.pem | Certificado raiz do servidor Secure Gateway
Nn5TJ34LyVQ_i2NOT_cert.pem | Certificado gerado automaticamente para ser usado por meu aplicativo ao conectar-se ao host e porta em nuvem
Nn5TJ34LyVQ_i2NOT_key.pem | Chave gerada automaticamente para ser usada pelo meu aplicativo ao conectar-se ao host e porta em nuvem
Nn5TJ34LyVQ_i2NOT_destCert.pem | Certificado gerado automaticamente para o cliente SG para ser usado ao conectar-se ao destino
Nn5TJ34LyVQ_i2NOT_destKey.pem | Chave gerada automaticamente para o cliente SG para ser usada ao conectar-se ao destino
Nn5TJ34LyVQ_clientCert.pem | Certificado gerado automaticamente de seu gateway para o cliente SG para ser usado no caso de não ter um arquivo `_destCert` ou `_destKey`
localServerCert.pem | O certificado de meu recurso local para ser usado na CA do cliente Secure Gateway

## Criando o aplicativo
{: #tls-ma-app-example}
Agora que eu tenho meu host e porta em nuvem para meu destino, bem como os vários certificados e chaves, posso começar a gravar meu aplicativo para conectar-se ao Secure Gateway.  Esse é um aplicativo Node.js simples que se conectará ao meu servidor TLS local, receberá uma pequena quantia de dados e, em seguida, fechará a conexão.

```javascript
let fs = require('fs');
let tls = require('tls');

let client_opts = {
    cert : fs.readFileSync('Nn5TJ34LyVQ_i2NOT_cert.pem'),
    key : fs.readFileSync('Nn5TJ34LyVQ_i2NOT_key.pem'),
    host : 'cap-sg-prd-1.integration.ibmcloud.com',
    port : 17996,
    rejectUnauthorized : true
}

let so = tls.connect(client_opts, function() {
    console.log('Client connected, authorized:', so.authorized);
    if (!so.authorized) console.log('Client authorization error:', so.authorizationError)
});

so.on('data', function(data) {
    console.log('Client data:', data.toString())
});

so.on('error', function(err) {
    console.log('Client error',err);
});

so.on('close', function() {
    console.log('Client connection closing');
});
```
{: codeblock}

## Criando o servidor TLS
{: #tls-ma-server-example}
Tenho meu servidor TLS local, que é um aplicativo Node.js simples que atenderá às conexões, gravará uma mensagem em uma conexão bem-sucedida e, em seguida, fechará essa conexão.

```javascript
let fs = require('fs');
let tls = require('tls');

let server_opts = {
    cert : fs.readFileSync('localServerCert.pem'),
    key : fs.readFileSync('localServerKey.pem'),
    // pfx : fs.readFileSync(''),
    // passphrase : '',
    ca : [fs.readFileSync('Nn5TJ34LyVQ_i2NOT_destCert.pem')],
    rejectUnauthorized : true,
    requestCert : true
}

// Create the local TLS Server with the settings defined above
let server = tls.createServer(server_opts, function(socket) {
    socket.write('Successful connection message from the server');
    socket.end();
});

// Set up the various event listeners for the server and log their output
server.on('error', function(err) {
    console.log('Server error', err);
});

server.on('close', function() {
    console.log('Server closing');
});

server.on('tlsClientError', function(err) {
    console.log('Server tlsClientError', err);
});

server.listen(8999);
```
{: codeblock}

## Atualizar a Lista de Controle de Acesso do cliente
{: #tls-ma-acl}
Antes de testar meu aplicativo, preciso certificar-me de que a ACL em meu Cliente esteja configurada apropriadamente.  Eu incluí `localhost:8999` em minha ACL:

```
--------------------------------------------------------------------
           -- Secure Gateway Client Access Control List --

 Connections to unlisted rules are currently: denied

 hostname:port                path           value
 localhost:8999                            Allow
--------------------------------------------------------------------
```
{: screen}

## Testando a conexão
{: #tls-ma-testing}
Agora que meu aplicativo, meu servidor local e meu Cliente foram configurados, eu obtenho a saída a seguir de meu aplicativo e meu cliente:

### Aplicativo
{: #tls-ma-testing-app}

```
Client connected, authorized: true
Client data: Successful connection message from the server
Client connection closing
```
{: screen}

### Cliente Secure Gateway
{: #tls-ma-testing-client}

```
[2017-04-07 12:22:42.363] [INFO] (Client ID Nn5TJ34LyVQ_qCB) Connection #1 is being established to localhost:8999
[2017-04-07 12:22:42.381] [INFO] (Client ID Nn5TJ34LyVQ_qCB) Connection #1 to localhost:8999 was closed
```
{: screen}

## Sucesso!
{: #tls-ma-testing-result}
Nós configuramos com êxito a Autenticação mútua para Autenticação do usuário e Autenticação do recurso!
