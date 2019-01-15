---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-04"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Interagindo com o cliente

Há algumas maneiras de interagir com o cliente:

- Por meio da linha de comandos do terminal antes da inicialização
- Após a inicialização, usando a linha de comandos interativa do cliente
- Após a inicialização, usando a IU local do cliente

## Configurações de inicialização
{: #startup}

### Argumentos e opções de inicialização
{: #startup-args}

A tabela a seguir descreve todas as opções disponíveis que podem ser fornecidas junto com o comando de inicialização para o cliente. O uso delas permitirá a configuração completa do cliente antes da inicialização em vez de requerer configuração individual depois que o cliente estiver em execução.

| Parâmetro e argumentos | Descrição |
| ------------- | ----------- |
| &lt;gateway ID&gt; | Conecte-se ao {{site.data.keyword.Bluemix_notm}}, usando o ID do gateway que é fornecido |
| -F, -\-aclfile &lt;file&gt; | Access control List file |
| -g, -\-gateway &lt;hostname:port&gt; | Used to manually select a specific gateway destination (advanced use only) |
| -l, -\-loglevel &lt;level&gt; | Change the log level to ERROR, INFO, DEBUG or TRACE |
| -p, -\-logpath &lt;file&gt; | Direct logging to a specific file |
| -t, -\-sectoken &lt;security token&gt; | The security token to use for this gateway connection |
| -P, -\-port &lt;port&gt; | The port for the UI to run on.  Defaults to port 9003 |
| -w, -\-password &lt;password&gt; | The password to protect the UI with.  Defaults to no password |
| -x, -\-proxy &lt;proxy agent&gt; | The proxy for the port 9000 connection |
| -\-noUI | Prevent the UI from starting up automatically |
| -\-allow | Allows all connections to the client. Is overridden by the ACL file, if provided |
| -\-service | After an initial connection, the parent will restart within 60s if all child clients are terminated |

<b>Nota:</b> as sinalizações `--service`, `--allow` e `--noUI` devem ser os últimos parâmetros nos argumentos de linha de comandos.

O caso de uso mais básico é iniciar com uma única conexão do cliente com as configurações padrão:

```
<gateway_id> -t <security_token>
```
{: pre}

Essas opções podem ser estendidas para suportar também a conexão automática de múltiplos clientes. Para passá-las para múltiplas conexões do cliente, elas devem ser passadas usando um formato específico. Os IDs do gateway podem ser passados da mesma maneira que com a conexão única do cliente (com cada ID do gateway separado por um espaço); no entanto, a ordem desses IDs determina a ordem que o restante dos argumentos deve seguir. Ao passar qualquer um dos outros argumentos, eles devem ser separados por `--` para que sejam captados corretamente. Se nada é passado para uma sinalização específica, presume-se que não se aplique a nenhum dos clientes.

Se não houver argumentos suficientes fornecidos para preencher todos os IDs do gateway, eles serão aplicados em ordem até que não haja mais nenhum argumento. Por exemplo, se dois IDs do gateway forem passados e um único token de segurança for passado, o token será aplicado ao primeiro ID do gateway e não ao segundo.  

Se forem fornecidos IDs do gateway que requerem argumentos diferentes, a palavra-chave `none` deverá ser designada no lugar de qualquer argumento específico que um gateway não esteja cumprindo/fornecendo. Por exemplo, se três IDs do gateway forem passados e o usuário desejar especificar um nível de log para o primeiro e o terceiro, o argumento será semelhante a `--loglevel DEBUG--none--TRACE`. Nesse caso, none será, então, padronizado para INFO.

### Exemplos de argumento de inicialização
{: #startup-examples}

Conexão do cliente único, nível de log customizado, nenhuma IU:

```
node lib/secgwclient.js <gateway_id> -t <security_token> -l <loglevel> --noUI
```
{: pre}

Múltiplas conexões do cliente, opções padrão:

```
node lib/secgwclient.js <gateway_id_1> <gateway_id_2> -t <security_token_1>--<security_token_2>
```
{: pre}

Múltiplas conexões do cliente, todas as configurações customizadas:

```
node lib/secgwclient.js <myGatewayID_1> <myGatewayID_2> -t none--<token for gateway 2> -l DEBUG--TRACE -p <full path to log file for gateway 1>--<full path to log file for gateway 2> -F <full path to ACL file for gateway 1>
```
{: pre}

Retorne para [Introdução - Incluindo um cliente](./securegateway_client.html).

## Configuração interativa
{: #interactive}

### Comandos interativos
{: #interactive-commands}

O cliente {{site.data.keyword.SecureGateway}} possui uma
interface de linha de comandos (cli) e um prompt de shell para configuração e controle fáceis. O ambiente interativo suporta um conjunto mais rico de recursos do que os argumentos de linha de comandos para facilitar um melhor controle interativo sobre o cliente.

| Comandos interativos | Descrição       |
| ------------- | ----------- |
| A, acl allow &lt;hostname:port&gt; &lt;worker ID&gt; | Permissão da lista de controle de acesso |
| D, acl deny &lt;hostname:port&gt; &lt;worker ID&gt; | Negação da lista de controle de acesso |
| N, no acl &lt;hostname:port&gt; &lt;worker ID&gt; | Remover uma entrada da lista de controle de acesso |
| S, show acl &lt;worker ID&gt; | Mostrar todas as entradas da lista de controle de acesso |
| F, acl file &lt;file&gt; &lt;worker ID&gt; | Arquivo de Lista de Controle de Acesso |
| C, displayconfig &lt;worker ID&gt; | Exibir a configuração atual do {{site.data.keyword.SecureGateway}},
se disponível |
| a, authorize &lt;worker ID&gt; | Alternar a substituição do parâmetro rejectUnauthorized para conexões TLS de saída do trabalhador especificado |
| t, sectoken &lt;security token&gt; | O token de segurança a ser usado para a próxima conexão de gateway |
| c, connect &lt;gateway ID&gt; | Conecte-se ao {{site.data.keyword.Bluemix_notm}}, usando o ID do gateway que é fornecido |
| l, loglevel &lt;level&gt; &lt;worker ID&gt; | Mudar o nível de log para ERROR, INFO, DEBUG ou TRACE |
| p, logpath &lt;file&gt; &lt;worker ID&gt; | Criação de log direto para um arquivo específico |
| r, reverse &lt;worker ID&gt; | Listar as portas nas quais o cliente está atendendo atualmente para destinos reversos |
| k, kill &lt;worker ID&gt;  | Finalizar o trabalhador especificado |
| e, select &lt;worker ID&gt;  | Especifica um trabalhador para executar comandos, a menos que seja especificado de outra forma |
| d, deselect | Cancela a seleção do trabalhador anteriormente especificado. Emita o comando de seleção para especificar outra |
| w, password &lt;old password&gt; &lt;new password&gt; | Configure a senha da IU. Se &lt;new password&gt; estiver em branco, nenhuma senha será cumprida. &lt;old password&gt; necessário na atualização de senha. As senhas devem conter apenas letras |
| P, port &lt;new port&gt; | Mude a porta na qual a UI está atendendo |
| u, uistart &lt;initial password&gt; &lt;port&gt; | Inicia a IU no localhost:&lt;port&gt;/dashboard. Se &lt;initial password&gt; estiver em branco e nenhuma outra senha tiver sido configurada para a sessão, nenhuma senha da IU será cumprida. Se &lt;port&gt; estiver em branco, a UI será acessível em 9003 |
| U, uistop | Fecha a UI associada a esta sessão do cliente. A sessão estará acessível somente por meio da CLI, até que uma nova IU seja iniciada manualmente |
| R, revoke | Limpa todas as autorizações da UI associadas a esta sessão do cliente |
| q, quit | Desconectar e sair |
| s, status &lt;worker ID&gt;  | Imprimir os detalhes do status do túnel e suas
conexões |
| L, list | Exibe uma lista dos trabalhadores atualmente associados |

<b>Nota:</b> se uma conexão tiver sido especificada com o comando `select` e outro comando for chamado sem fornecer um ID do trabalhador, o comando tentará executar na conexão especificada por `select`.

Para obter mais detalhes sobre como configurar a Lista de Controle de Acesso, [clique aqui](./securegateway_acl.html).

Retorne para [Introdução - Incluindo um cliente](./securegateway_client.html).

## UI do Cliente
{: #ui}

<b>Nota:</b> a IU do cliente não é suportada ao usar o Docker no Windows ou MacOS.

A IU do cliente fornece uma interface da web local para que o usuário interaja com o cliente {{site.data.keyword.SecureGateway}} em vez da CLI. Por padrão, essa IU está disponível em `localhost:9003/dashboard`. A IU é dividida nas páginas a seguir:

### Conexão
{: #ui-connect}

Essa é a página de entrada inicial para a IU na qual um usuário pode fornecer um ID do gateway e um token de segurança para conectar seu primeiro cliente.

### Login:
{: #ui-login}

Essa página será exibida se a IU tiver sido protegida por senha. Se essa página for atingida enquanto nenhuma senha estiver sendo cumprida, atualize a página a ser redirecionada.

### Dashboard
{: #ui-dashboard}

Essa é a página principal após um cliente ter sido conectado. Desse ponto, é possível acessar a página Visualizar logs, a página Lista de Controle de Acesso e a página Informações de conexão. Na parte inferior, também é possível escolher desconectar um/muitos dos clientes conectados. Na parte superior da página, o cliente selecionado atualmente será exibido, bem como uma opção para conectar clientes adicionais.

### Visão de Registros
{: #ui-logs}

Essa página permitirá que você veja os logs sendo gerados pelo cliente selecionado (mostrados no canto superior direito da página). Os logs exibidos podem
ser filtrados pelas caixas de seleção abaixo dos logs.

### Lista de controle de acesso
{: #ui-acl}

Essa página permitirá manipular a Lista de Controle de Acesso do cliente selecionado (mostrada no canto superior direito da página). As regras podem ser incluídas individualmente nas tabelas de permissão/negação ou um arquivo pode ser transferido por upload na parte inferior da página.

### Informações de Conexão
{: #ui-info}

Essa página mostrará as informações de conexão atuais para o cliente selecionado (mostradas no canto superior direito da página). Informações
como descrição de gateway, número de conexões atuais e listeners de destino reverso podem ser vistos aqui.

Retorne para [Introdução - Incluindo um cliente](./securegateway_client.html).

## Finalização do cliente remoto
{: #remote}

Se um cliente tiver recebido um ID, ele poderá ser finalizado remotamente por meio da IU do SG ou da API do SG. Se você finalizar um cliente que esteja sendo executado como um serviço, o cliente reiniciará e obterá um novo ID do cliente; entretanto, se o serviço tiver múltiplos clientes conectados, o cliente finalizado não será reiniciado até que todos os clientes restantes tenham sido finalizados.

## Limitações
{: #limits}

### Limitações de conexão
{: #limits-conn}

O gateway do SG pode manipular somente 250 conexões simultâneas. Se o número de solicitações simultâneas exceder o limite, isso poderá resultar nas tentativas de conexão sendo rejeitadas e levar à latência. Uma maneira fácil de corrigir isso é usar a definição do conjunto de conexões no app de chamada. Observe que o limite de 250 conexões simultâneas está no gateway e não no cliente ou no destino. Esse limite será compartilhado em todos os clientes e destinos no gateway.

### Limitações do cliente DataPower
{: #limits-datapower}

O cliente {{site.data.keyword.SecureGateway}} DataPower está no processo de atualização para corresponder aos recursos do restante de nossos clientes. Ele tem atualmente as limitações a seguir:

- A ACL será padronizada para ALLOW ALL
- Não há suporte para ativar e desativar gateways ou destinos na UI do
{{site.data.keyword.SecureGateway}}. No entanto, a opção Estado administrativo na IU do DataPower funciona como um comutador liga/desliga para esse cliente específico.
- A pesquisa de status da conexão para atualizações de status do gateway conectado ou
desconectado em tempo real não é suportada.
- Cadeias de certificados completas com TLS do lado de destino não são suportadas antes do DataPower versão 7.5.1.0
- Os destinos em nuvem não são suportados antes do DataPower versão 7.5.1.0
- O nível de log não pode ser mudado para o nível TRACE
