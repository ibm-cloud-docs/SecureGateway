---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Resolver problemas

## Melhores práticas para executar o cliente Secure Gateway
{: #best}

- Execute o cliente Secure Gateway em uma partição do sistema operacional (S.O.) que tenha visibilidade de rede dos serviços que são vinculados pelo próprio cliente. Por exemplo,
alguns ambientes de virtualização hospedados suportam vários modos de conectividade de
rede, incluindo NAT e Conectado por ponte. Certifique-se de escolher o tipo de conexão
correto que forneça acesso aos serviços do {{site.data.keyword.Bluemix}}
a partir da Internet.
- Instale o Secure Gateway em seu ambiente de TI no qual a política de segurança corporativa permite. Isso
seria, geralmente, em uma zona amarela protegida ou DMZ, em que sua empresa
possa instituir os controles de segurança apropriados para proteger ativos
no local. Sempre siga suas políticas e instruções de segurança corporativa ao instalar o cliente Secure Gateway.
- Antes de instalar um cliente em seu ambiente, assegure-se de que a Internet e seus
ativos no local estejam acessíveis e de que todos os nomes de host possam ser
resolvidos por um DNS.

## Etapas iniciais de resolução de problemas

- Inicie a solicitação com falha por meio do aplicativo solicitante
- Verifique os logs do cliente Secure Gateway
- Se nenhum log de cliente foi gerado por meio da solicitação, o problema é entre o aplicativo solicitante e os servidores Secure Gateway. Isso pode variar desde a confiabilidade de rede a protocolos de solicitação incompatíveis para um handshake de autenticação mútua TLS incorreto.
- Se o cliente gerou logs de erro por meio da solicitação, o problema é entre o cliente SG e o recurso no local. Abaixo está uma tabela contendo erros comuns, os problemas que normalmente os causam e os potenciais métodos para solucioná-los.

Erro | Causa típica | Métodos de resolução de problemas
--- | --- | ---
ETIMEDOUT | O cliente é incapaz de localizar o nome do host/ip ao qual se conectar devido a restrições de rede. | Tente executar ping do nome do host/ip do destino por meio do host que está executando o cliente para assegurar a conectividade de rede. Se estiver executando a versão Docker do cliente, vincular o contêiner ao S.O. do host com `--net=host` poderá resolver o problema.
ECONNREFUSED | O cliente resolveu o nome do host/ip ao qual se conectar, mas é incapaz de iniciar o handshake da conexão | Isso é geralmente causado por um protocolo incompatível entre o cliente SG e o recurso no local (por exemplo, o cliente está tentando uma conexão TCP com um host:porta que está esperando uma conexão TLS). Em alguns casos, uma regra de firewall pode causar esse erro, em vez de ETIMEDOUT.
ECONNRESET | O cliente estabeleceu uma conexão com o destino, mas algo deu errado durante o handshake (um erro de handshake TLS também pode resultar em erros diferentes) ou enquanto a solicitação estava sendo manipulada pelo recurso no local. | Os logs do recurso no local devem ser verificados para confirmar que nenhum erro causou a interrupção da conexão. Se nada for localizado nos logs no local, a configuração de destino deverá ser examinada para assegurar que os protocolos (e certificados, se necessário) apropriados estejam sendo fornecidos ao cliente para a conexão.
REMOTE_RST | Ocorreu um erro no lado do servidor SG. <br><br> Para o destino no local, ocorre erro quando o app solicitante se conecta ao servidor SG ou o erro de tempo limite ao receber dados do recurso no local. <br><br> Para o destino em nuvem, isso pode ser qualquer coisa, desde falha de handshake TLS até erros no recurso em nuvem | Para o destino no local, assegure que o app solicitante que usa os protocolos apropriados estabeleça a conexão com o servidor SG. Se o erro ocorrer ao receber dados do recurso no local, tente estender/desativar o tempo limite. <br><br> Para o destino em nuvem, os logs do recurso em nuvem devem ser verificados para confirmar que nenhum erro causou a interrupção da conexão. Se nada for localizado nos logs de recurso em nuvem, a configuração de destino deverá ser examinada para assegurar que os protocolos (e certificados, se necessário) apropriados estejam sendo fornecidos ao cliente para a conexão.

Muitos aplicativos experimentam "interrupções" depois que um ECONNRESET ocorre na outra extremidade do túnel. Isso é esperado. O Secure
Gateway não pode reproduzir o pacote RST na outra extremidade do túnel, uma vez que os pacotes TCP já foram confirmados para esse
lado do túnel. Os tempos limite de nível do aplicativo, o aplicativo nunca recebe uma resposta de reconhecimento, são o único
método para terminar a interrupção.

## Configurar o cliente Docker para ser reiniciado durante e reinicialização de servidor
{: #docker}

### O que está acontecendo?
Ao reiniciar o servidor no qual o cliente Secure Gateway é executado, deve-se reiniciar manualmente o cliente Docker do Secure Gateway. Como o cliente pode ser iniciado automaticamente depois de uma
reinicialização do sistema?

### Como corrigir isso

- Em sistemas Linux ou UNIX:
- Integre o comando do Docker a um script que possa ser chamado
como resultado de uma tarefa CRON.
- Se estiver usando uma estação de trabalho Linux, será possível criar uma configuração upstart para assegurar que o cliente seja iniciado sempre que o servidor for reiniciado. Para obter mais informações, veja Iniciar contêineres automaticamente no website do Docker.
- Em sistemas Windows, emita o comando a seguir para iniciar o cliente:

```
for /L %i in (0,0,0) do docker run -it ibmcom/secure-gateway-client <gateway_id>
```
{: pre}

## Mensagem de erro de conexão: host não está no CN do certificado
{: #not-in-cn}

### O que está acontecendo?
Você está tentando implementar o TLS do lado do cliente no local usando o cliente Secure Gateway e recebe a mensagem de erro a seguir.

```
[ERROR] Connection #<connection ID> had error: Host: <host name>. is not cert's CN: <mycommonname>

Where:
    - connection ID is a client assigned connection number.
    - host name is the host name for the connection.
    - mycommonname is the FDQN or common name that is used in the certificate.
```
{: screen}

### Por que está acontecendo?
O Nome comum, por exemplo, o nome FQDN do servidor ou SEU nome,
entre o aplicativo no local e o certificado transferido por
upload para o {{site.data.keyword.Bluemix_notm}} para
esse destino não corresponde.

- Verifique os itens a seguir:
- Você gerou o certificado corretamente e se usou o
FQDN ou o nome do host correto do servidor.
- Você transferiu por upload o certificado correto para o destino do {{site.data.keyword.Bluemix_notm}}
para esse cliente.

### Como corrigir isso

 1. Na IU do {{site.data.keyword.Bluemix_notm}}, acesse o Painel do Secure Gateway.
 2. Selecione seu destino e clique no ícone Editar.
 3. Clique em Fazer upload de certificado.
 4. Faça upload do arquivo de certificado PEM que deve ser usado para se conectar
ao sistema no local.

## O IP não está na lista do certificado
{: #san}

### O que está acontecendo?
O CN no certificado apresentado é o endereço IP do gateway, mas o certificado não tem uma SAN correspondente ao endereço IP e o cliente falha ao se conectar.  

Devido a problemas de resolução do nome do host, estamos usando o endereço IP em nosso destino. O CN no certificado apresentado é o endereço IP do gateway, mas o certificado não tem uma SAN correspondente ao endereço IP e o cliente falha ao se conectar

Um destino usando TLS foi criado, mas em vez de usar o nome do host do destino, você usou seu endereço IP. Ao conectar o cliente, o erro a seguir está sendo lançado.

```
[2015-10-15 13:00:04.866] [INFO] Connection #0 is being established to 10.3.20.31:443
[2015-10-15 13:00:05.426] [ERROR] Connection #0 to destination 10.3.20.31:443 had error: IP: 10.3.20.31 is not in the cert's list:
[2015-10-15 13:00:05.427] [INFO] Connection #0 to 10.3.20.31:443 was closed
```
{: screen}

### Por que está acontecendo?
O que está acontecendo é que o código de verificação SSL no cliente de gateway está tratando esse destino de forma diferente porque ele usa um endereço IP em vez de um nome do host. Em vez de corresponder com o CN do certificado, ele está examinando a SAN do certificado para uma correspondência do endereço IP. Como não há SAN no certificado, ele vê isso como uma conexão inválida e falha o handshake SSL.

### Como corrigir isso
Se você observa a mensagem de erro, ela não diz CN, (por exemplo, [ERROR] Connection ## had error: Host: . is not cert&apos;s CN:), mas cert&apos;s list, o que leva a crer que você gerou seu certificado autoassinado incorretamente. O problema está gerando o certificado usando um FQDN ou CN com um IP_Address. Isso não funcionará, pois os endereços IP são suportados somente ao usar a SAN.

Método para gerar um certificado com um IP como o CN com openssl:

1. Crie um arquivo de configuração openssl, eu copiei o meu de /usr/lib/ssl/openssl.cnf
2. Inclua uma seção alternate_names no arquivo como a seguir:

   ```
   [ alternate_names ]

   IP.1 = &lt;my application&apos;s ip&gt;
   ```
   {: codeblock}

3. Na seção [ v3_ca ], inclua esta linha:

    ```
    subjectAltName = @alternate_names
    ```
    {: pre}

4. Sob a seção CA_default, remova o comentário de copy_extensions (opção de cópia de extensão: use com cuidado):

    ```
    copy_extensions = copy
    ```
    {: pre}

5. Gere a chave privada

    ```
    openssl genrsa -out private.key 3072
    ```
    {: pre}

6. Gere o certificado com opções sobre organização

    ```
    openssl req -new -x509 -key private.key -sha256 -out certificate.pem -days 730 -config
    ```
    {: pre}

7. Combine os arquivos

    ```
    cat private.key certificate.pem > SAN.pem
    ```
    {: pre}

8. Carregue o arquivo SAN.pem em seu destino como o certificado TLS do cliente.
9. Carregue o arquivo SAN.pem em seu aplicativo no local e reinicie.

10. Seu destino pode ser configurado para TCP, HTTP ou HTTPS e seu aplicativo do lado da nuvem deve agora ser capaz de se conectar ao seu aplicativo no local.

Se você encontrar um problema UNABLE_TO_VERIFY_LEAF_SIGNATURE, verifique seu arquivo openssl.conf, mude o seguinte de:

    ```
    keyUsage = digitalSignature, keyEncipherment
    ```
    {: pre}

para o padrão:

    ```
    keyUsage = nonRepudiation, digitalSignature, keyEncipherment
    ```
    {: pre}

## Mensagem de erro de conexão: DEPTH_ZERO_SELF_SIGNED_CERT
{: #depth-zero}

### O que está acontecendo?
Você está tentando implementar o TLS do lado do cliente no local usando o cliente Secure Gateway e recebe a mensagem de erro a seguir.

```
[ERROR] Connection #<connection ID> had error: DEPTH_ZERO_SELF_SIGNED_CERT Where:

    connection ID is a client assigned connection number.
```
{: screen}

### Por que está acontecendo?
Há um certificado de lado do cliente ausente no destino definido.

### Como corrigir isso
 1. Na IU do {{site.data.keyword.Bluemix_notm}}, acesse o Painel do Secure Gateway.
 2. Selecione seu destino e clique no ícone Editar.
 3. Clique em Fazer upload de certificado.
 4. Faça upload do arquivo de certificado PEM que deve ser usado para se conectar
ao sistema no local.


## Como posso carregar de forma interativa um arquivo ACL no cliente Docker?
{: #docker-acl}

### O que está acontecendo?
Como o Docker é um contêiner ou ambiente virtualizado, ele não tem acesso direto ao seu sistema de arquivos até que o contêiner seja realmente ativado. Isso evita que ele leia o sistema de arquivos de máquinas host até que seja realmente ativado e esteja em execução.

### Como corrigir isso
Isso é o que pode ser feito:

- Crie um Dockerfile para incluir o aclfile.txt

```
FROM ibmcom/secure-gateway-client
ADD aclfile.txt /tmp/aclfile.txt
```
{: codeblock}

- Construa uma nova imagem do docker

```
docker build -t ads-secure-gateway-client .
```
{: pre}

- Execute a nova imagem do docker (é necessário especificar as opções -t e -i, caso contrário, você obterá erro, arquivo não localizado ou ENOENT):

```
docker run -t -i ads-secure-gateway-client1 --F /tmp/aclfile.txt
```
{: pre}

- Obtenha a saída a seguir:

```
[2015-09-30 16:50:32.084] [INFO] The current access control list is being reset and replaced by the user provided file: /tmp/aclfile.txt
[2015-09-30 16:50:32.086] [INFO] The ACL file process accepts acl allow :8000
[2015-09-30 16:50:32.087] [INFO] The ACL file process accepts acl deny local
```
{: screen}

## Obtendo ajuda e suporte adicionais
{: #support}

Se você tiver perguntas técnicas sobre como desenvolver ou implementar um aplicativo com o Secure Gateway, poste sua pergunta no [Stack Overflow ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://stackoverflow.com/search?q=securegateway+ibm-bluemix). Identifique sua pergunta com "ibm-bluemix" e "secure-gateway" para que ela possa ser localizada mais prontamente pelas equipes de desenvolvimento do {{site.data.keyword.Bluemix_notm}}.

Se você tiver alguma pergunta sobre o serviço ou instruções sobre como começar, use o fórum do [IBM developerWorks dW Answers ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/answers/topics/securegateway/?smartspace=bluemix) com as tags "bluemix" e "Secure Gateway".

Para obter mais detalhes sobre como usar esses fóruns, verifique a [página Obtendo ajuda aqui](https://console.ng.bluemix.net/docs/support/index.html#getting-help).

Para obter informações sobre como abrir um chamado de suporte IBM
ou sobre os níveis de suporte e severidades de chamado, consulte
[Entrando
em contato com o suporte](https://console.ng.bluemix.net/docs/support/index.html#contacting-support).

Forneça o máximo possível das informações a seguir ao enviar um chamado:

- Em qual S.O. o cliente está em execução?
- Qual versão do cliente está sendo usada (isso pode ser localizado usando o comando 'C' no cliente)?
- Se esse for um problema de IU, cole ou anexe quaisquer logs e capturas de tela do console do navegador associados
- Cole ou anexe quaisquer logs do aplicativo solicitante associados
- Cole ou anexe quaisquer logs do cliente Secure Gateway associados
- Forneça os detalhes do destino que está sendo usado (uma captura de tela ou preencha os campos a seguir):
   - ID de destino
   - Protocol
   - Autenticação do lado do destino
   - Certificados transferidos por upload (apenas nomes e a caixa em que foram transferidos por upload)
