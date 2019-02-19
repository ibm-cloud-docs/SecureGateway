---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-08"

---

# Perguntas mais frequentes
{: #sg-faq}

## Camada de modelo OSI
{: #osi}

### Pergunta
Qual camada do modelo OSI o serviço Secure Gateway representa?

### Resposta
O serviço Secure Gateway representa a camada 4 do modelo OSI.

## Suporte à versão do TLS
{: #tls}

### Pergunta
Qual versão do TLS faz o serviço Secure Gateway suporta?

### Resposta
O serviço Secure Gateway suporta o TLS versão 1.2.

## Por que eu desativaria meu gateway ou destino?
{: #disabled}

### Pergunta

Por que você desejaria desativar um gateway ou um destino?

### Resposta
Você
pode querer desativar um destino ou gateway por um dos motivos a
seguir:

- Você criou um destino, mas não configurou a segurança.  Nesse
caso, você poderá desativar o destino até sua segurança ser
configurada.
- Você não deseja que o serviço fique disponível para os usuários
porque está fazendo algumas atualizações no serviço.  Nesse caso, você pode desativar temporariamente os gateways necessários e aguardar que o serviço seja atualizado.
- Você configurou todos os seus gateways e destinos no front-end, mas seu back-end ainda está sendo construído. Nesse caso, você poderia
desativar seus gateways ou destinos até que a construção de backend
esteja completa.

Para obter mais informações sobre a desativação de um gateway ou um destino, veja [como gerenciar sua instância de serviço do Secure Gateway](/docs/services/SecureGateway/securegateway_managing.html).

## Qual é a abordagem recomendada para a automação de criação entre múltiplos espaços?
{: #automation-spaces}

### Pergunta
Um ambiente do cliente tem uma organização e três espaços. Um espaço é para desenvolvimento, outro para preparação e o último para produção. O cliente crie uma única ou múltiplas instâncias (por exemplo, uma para cada espaço) do Secure Gateway. Se o cliente pode criar múltiplos gateways, há quaisquer considerações para reutilizar um aplicativo Node.js para criar um gateway e um destino em cada espaço?

### Resposta

- É possível criar uma única instância do Secure Gateway para todos os três espaços. No entanto, deve-se lembrar das [limitações de gateway e de destino para seu plano específico](/docs/services/SecureGateway/securegateway_plans.html).
- Não há considerações adicionais para reutilizar um aplicativo Node.js, pois nenhuma ligação de serviço é requerida pelo Secure Gateway.


## Qual é a abordagem recomendada para a automação de criação em múltiplas organizações?
{: #automation-orgs}

### Pergunta
Um ambiente do cliente tem três organizações: uma para desenvolvimento, uma para preparação e uma para produção. Uma instância de serviço do Secure Gateway é necessária para cada organização e a configuração está disponível para todos os espaços dentro dessa organização?

### Resposta

- Você não precisa ter uma instância de serviço do Secure Gateway em cada organização. É possível ter uma instância em uma organização e usar os gateways nessa instância por meio de todos os outros ambientes. Com essa configuração, deve-se lembrar as [limitações de gateway e de destino para seu plano específico](/docs/services/SecureGateway/securegateway_plans.html).
- É possível ter uma instância de serviço do Secure Gateway em cada organização e a configuração estará disponível para todos os seus espaços.

## Meu app precisa estar no mesmo espaço?
{: #app-space}

### Pergunta
Você precisa executar o app Node.js no mesmo espaço do {{site.data.keyword.Bluemix_notm}} que o serviço Secure Gateway?

### Resposta
Não, você não precisa executar seu app no mesmo espaço do {{site.data.keyword.Bluemix_notm}} que o serviço Secure Gateway.

## É possível obter os logs do servidor Secure Gateway?
{: #server-logs}

### Pergunta
É possível recuperar os logs de erro para o servidor Secure Gateway?

### Resposta
Os logs de erro no servidor não podem ser recuperados. Somente erros que são feitos no momento da solicitação podem ser vistos.

## Quais são os estados funcionais do Secure Gateway?
{: #states}

### Pergunta
Quais são os diferentes estados do ciclo de vida de gateways e destinos?

### Resposta

#### Estado não funcional
A liberação 1.7.0 introduziu um novo modelo de precificação de plano em camadas. Com esse modelo, veio a capacidade de marcar os Gateways e os Destinos como 'Ativo' ou 'Inativo'. Parte da nova estrutura de faturamento do plano cobra o usuário pelo número de Gateways e Destinos que ele tem.

- Itens inativos não são faturados
- Os Destinos inativos não têm uma porta em nuvem provisionada.
- Todos os Destinos dentro de um gateway inativo também estarão inativos e não poderão ser reativados até que seu Gateway também seja configurado como Ativo.
- Itens inativos são considerados Não funcionais. Os itens inativos não podem estar em nenhum dos estados Funcionais.

#### Estados funcionais
Quando um gateway ou um destino for marcado como ativo, ele será faturado. Os estados ativos para Gateways e Destinos estão abaixo:

- Ativado - o gateway ou o destino está pronto para uso em circunstâncias normais.
- Desativado - o gateway ou o destino foi desativado.
    - Gateways - os clientes não poderão fluir dados para o gateway desativado.
    - Destinos - os clientes não conseguirão criar conexões com esses destinos desativados.

## Como saber quais são as atividades de dados no cliente Secure Gateway?
{: #data-size}

### Pergunta
Como eu conheço as atividades de dados por meio do cliente Secure Gateway?

### Resposta
No cliente SecureGateway, mude o nível de log para TRACE. As informações a seguir serão exibidas após as solicitações serem enviadas.

Tamanho dos dados enviados do aplicativo de solicitação:
```
[TRACE] Connection #<connection ID> transmitted data: <size> bytes
```

Tamanho dos dados enviados do destino:
```
[TRACE] Connection #<connection ID> received data: <size> bytes
```

## Como obter o número de conexão em tempo real no Secure Gateway?
{: #connect-num}

### Pergunta
Como obter as informações de conexão em tempo real, o tamanho dos dados enviados e recebidos do cliente Secure Gateway?

### Resposta

- Na linha de comandos interativa do cliente Secure Gateway:
digite `s` para imprimir os detalhes do status da conexão. 
- Na IU do cliente Secure Gateway, clique no menu Informações de conexão

As estatísticas de conexão a seguir seriam exibidas: 
- Tamanho de dados gerais transmitido.
- Tamanho de dados gerais recebido.
- O número de conexões totais.
- Conexões ativas em tempo real.

Nota: o número de Conexões atuais na IU do Secure Gateway não é renderizado em tempo real. Use as maneiras acima no cliente Secure Gateway para recuperar as informações de conexão em tempo real.

## Quais são as configurações recomendadas para tornar minhas conexões mais seguras?
{: #secure-app}

### Pergunta
Quais são as configurações recomendadas para tornar minhas conexões mais seguras?

### Resposta

#### Usar autenticação mútua
Ativar a Autenticação mútua para ambos os lados dos destinos no local torna o Secure Gateway mais seguro. No lado Autenticação do usuário, ative a autenticação mútua para restringir o acesso do nó em nuvem do Secure Gateway por meio da autenticação usando um certificado de cliente quando a solicitação é por meio de TLS/HTTPS. No lado Autenticação do recurso, ative a autenticação mútua para fornecer a credencial apropriada ao conectar-se ao terminal de destino, assegure acesso seguro/criptografado para o recurso no local. Veja [Configurando a autenticação mútua](/docs/services/SecureGateway/securegateway_destination.html#mutual-auth) e [Autenticação mútua TLS do Node.js](/docs/services/SecureGateway/securegateway_tls-ma.html#node-js-tls-mutual-authentication) para obter mais informações.

#### Configurar regras de tabela de IPs (para o destino no local)
O host e a porta em nuvem do Secure Gateway de um destino no local estão no espaço público; portanto, é permitido que todos acessem por padrão.
Para controlar o tráfego acessando no Secure Gateway, configure as regras de iptable para permitir acesso somente por um intervalo específico de IPs e portas para proteger recursos no local. Veja [Regras de tabela de IPs](/docs/services/SecureGateway/securegateway_destination.html#configuring-network-security) para obter mais informações sobre como configurar as regras de iptable no Secure Gateway.

#### Configurar Lista de Controle de Acesso (para o destino no local)
Configure o suporte à Lista de Controle de Acesso para permitir ou restringir o acesso aos recursos no local para tornar os destinos no local mais seguros, especificando o direito de acesso no host e na porta de destino específicos. É recomendável definir as rotas HTTP/S permitidas ou restritas nas entradas ACL, bem como aprimorar a segurança do destino no local. Veja [Lista de Controle de Acesso](/docs/services/SecureGateway/securegateway_acl.html#access-control-list) e [Controle de HTTPS/Rota usando a ACL](/docs/services/SecureGateway/securegateway_acl.html#routes) para obter mais informações.

#### Configurar a senha na IU do cliente Secure Gateway
É recomendável configurar a senha da IU para restringir o acesso da IU do cliente Secure Gateway. Veja [Interagindo com o cliente](/docs/services/SecureGateway/securegateway_interaction.html#interacting-with-the-client) para obter mais detalhes sobre como configurar a senha usando a configuração de inicialização ou os comandos interativos na linha de comandos do terminal do cliente Secure Gateway.
