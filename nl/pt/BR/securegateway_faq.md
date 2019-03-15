---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-16"

---

# Perguntas mais frequentes
{: #sg-faq}

## Camada de modelo OSI
{: #faq-osi}

### Pergunta
{: #osi-question}
Qual camada do modelo OSI o serviço Secure Gateway representa?

### Resposta
{: #osi-answer}
O serviço Secure Gateway representa a camada 4 do modelo OSI.

## Suporte à versão do TLS
{: #faq-tls}

### Pergunta
{: #tls-question}
Qual versão do TLS faz o serviço Secure Gateway suporta?

### Resposta
{: #tls-answer}
O serviço Secure Gateway suporta o TLS versão 1.2.

## Por que eu desativaria meu gateway ou destino?
{: #faq-disabled}

### Pergunta
{: #disabled-question}

Por que você desejaria desativar um gateway ou um destino?

### Resposta
{: #disabled-answer}
Você
pode querer desativar um destino ou gateway por um dos motivos a
seguir:

- Você criou um destino, mas não configurou a segurança.  Nesse
caso, você poderá desativar o destino até sua segurança ser
configurada.
- Você não deseja que o serviço fique disponível para os usuários
porque está fazendo algumas atualizações no serviço.  Nesse caso, você pode desativar temporariamente os gateways necessários e aguardar que o serviço seja atualizado.
- Você configurou todos os seus gateways e destinos no front-end, mas seu back-end ainda está sendo construído.  Nesse caso, você poderia
desativar seus gateways ou destinos até que a construção de backend
esteja completa.

Para obter mais informações sobre a desativação de um gateway ou um destino, veja [como gerenciar sua instância de serviço do Secure Gateway](/docs/services/SecureGateway/securegateway_managing.html).

## Qual é a abordagem recomendada para a automação de criação entre múltiplos espaços?
{: #faq-automation-spaces}

### Pergunta
{: #automation-spaces-question}
Um ambiente do cliente tem uma organização e três espaços.  Um espaço é para desenvolvimento, outro para preparação e o último para produção.  O cliente deve criar uma única instância do Secure Gateway ou
várias instâncias (por exemplo, uma para cada espaço)? Se o cliente pode criar múltiplos gateways, há quaisquer considerações para reutilizar um aplicativo Node.js para criar um gateway e um destino em cada espaço?

### Resposta
{: #automation-spaces-answer}

- É possível criar uma única instância do Secure Gateway para todos os três espaços.  No entanto, deve-se lembrar das [limitações de gateway e de destino para seu plano específico](/docs/services/SecureGateway/securegateway_plans.html).
- Não há considerações adicionais para reutilizar um aplicativo Node.js, pois nenhuma ligação de serviço é requerida pelo Secure Gateway.


## Qual é a abordagem recomendada para a automação de criação em múltiplas organizações?
{: #faq-automation-orgs}

### Pergunta
{: #automation-orgs-question}
Um ambiente do cliente tem três organizações: uma para desenvolvimento, uma para preparação e uma para produção.  Uma instância de serviço do Secure Gateway é necessária para cada organização e a configuração está disponível para todos os espaços dentro dessa organização?

### Resposta
{: #automation-orgs-answer}

- Você não precisa ter uma instância de serviço do Secure Gateway em cada organização. É possível ter uma instância em uma organização e usar os gateways nessa instância por meio de todos os outros ambientes.  Com essa configuração, deve-se lembrar as [limitações de gateway e de destino para seu plano específico](/docs/services/SecureGateway/securegateway_plans.html).
- É possível ter uma instância de serviço do Secure Gateway em cada organização e a configuração estará disponível para todos os seus espaços.

## Meu app precisa estar no mesmo espaço?
{: #faq-app-space}

### Pergunta
{: #app-space-question}
É necessário executar o app Node.js no mesmo espaço do {{site.data.keyword.Bluemix_notm}} que
o do serviço Secure Gateway?

### Resposta
{: #app-space-answer}
Não, você não precisa executar seu app no mesmo espaço do {{site.data.keyword.Bluemix_notm}} que o serviço Secure Gateway.

## É possível obter os logs do servidor Secure Gateway?
{: #faq-server-logs}

### Pergunta
{: #server-logs-question}
É possível recuperar os logs de nível de erro para o servidor do Secure Gateway?

### Resposta
{: #server-logs-answer}
Os logs de nível de erro no servidor não podem ser recuperados. Somente erros que são feitos no momento da solicitação podem ser vistos.

## Quais são os estados funcionais do Secure Gateway?
{: #faq-states}

### Pergunta
{: #states-question}
Quais são os diferentes estados do ciclo de vida de gateways e destinos?

### Resposta
{: #states-answer}

#### Estado não funcional
{: #states-answer-non-functional}
A liberação 1.7.0 introduziu um novo modelo de precificação de plano em camadas. Com esse modelo, veio a capacidade de marcar os Gateways e os Destinos como 'Ativo' ou 'Inativo'. Parte da nova estrutura de faturamento do plano cobra o usuário pelo número de Gateways e Destinos que ele tem.

- Itens inativos não são faturados
- Os Destinos inativos não têm uma porta em nuvem provisionada.
- Todos os Destinos dentro de um gateway inativo também estarão inativos e não poderão ser reativados até que seu Gateway também seja configurado como Ativo.
- Itens inativos são considerados Não funcionais. Os itens inativos não podem estar em nenhum dos
estados Funcionais.

#### Estados funcionais
{: #states-answer-functional}
Quando um gateway ou um destino for marcado como ativo, ele será faturado. Os estados ativos para Gateways e Destinos estão abaixo:

- Ativado - o gateway ou o destino está pronto para uso em circunstâncias normais.
- Desativado - o gateway ou o destino foi desativado.
    - Gateways - os clientes não poderão fluir dados para o gateway desativado.
    - Destinos - os clientes não conseguirão criar conexões com esses destinos desativados.

## Como saber quais são as atividades de dados no cliente Secure Gateway?
{: #faq-data-size}

### Pergunta
{: #data-size-question}
Como eu conheço as atividades de dados por meio do cliente Secure Gateway?

### Resposta
{: #data-size-answer}
No cliente SecureGateway, mude o nível de log para TRACE. As informações a seguir serão exibidas após as solicitações serem enviadas.

Tamanho dos dados enviados do aplicativo de solicitação:
```
[TRACE] Connection #<connection ID> transmitted data: <size> bytes
```

Tamanho dos dados enviados do destino:
```
[TRACE] Connection #<connection ID> received data: <size> bytes
```

## Como obtenho a quantia de conexões em tempo real no Secure Gateway?
{: #faq-connect-num}

### Pergunta
{: #connect-num-question}
Como obtenho informações de conexões, tais como a quantia de conexões em tempo real e o tamanho dos
dados enviados e recebidos do cliente Secure Gateway?

### Resposta
{: #connect-num-answer}

- Na linha de comandos interativa do cliente Secure Gateway:
digite `s` para imprimir os detalhes do status da conexão. 
- Na IU do cliente Secure Gateway, clique no menu Informações de conexão

As estatísticas de conexão a seguir seriam exibidas: 
- Tamanho de dados gerais transmitido.
- Tamanho de dados gerais recebido.
- A quantia de conexões totais.
- Conexões ativas em tempo real.

Nota: essas informações de conexões estão no nível do Cliente, não no nível do Gateway. Se você
precisar de informações de conexões no nível do Gateway, verifique cada Cliente que esteja conectado
a esse Gateway.

## Quais são as configurações recomendadas para tornar minhas conexões mais seguras?
{: #faq-secure-app}

### Pergunta
{: #secure-app-question}
Quais são as configurações recomendadas para tornar minhas conexões mais seguras?

### Resposta
{: #secure-app-answer}

#### Usar autenticação mútua
{: #secure-app-answer-ma}
Ativar a Autenticação mútua para ambos os lados dos destinos no local torna o Secure Gateway mais seguro. No lado Autenticação do usuário, ative a autenticação mútua para restringir o acesso do nó em nuvem do Secure Gateway por meio da autenticação usando um certificado de cliente quando a solicitação é por meio de TLS/HTTPS. No
lado de Autenticação de recurso, ative a autenticação mútua para que ela forneça a credencial apropriada ao
conectar-se ao terminal de destino e assegure acesso seguro/criptografado ao recurso no local. Veja [Configurando a autenticação mútua](/docs/services/SecureGateway/securegateway_destination.html#dest-mutual-auth) e [Autenticação mútua TLS do Node.js](/docs/services/SecureGateway/securegateway_tls-ma.html#nodejs-tls-ma) para obter mais informações.

#### Configurar regras de tabela de IPs (para o destino no local)
{: #secure-app-answer-iptables}
O host e a porta em nuvem do Secure Gateway de um destino no local estão no espaço público; portanto, é permitido que todos acessem por padrão.
Para controlar o acesso ao tráfego no Secure Gateway, configure as regras iptables para permitir acesso
somente por um intervalo específico de IPs e portas para proteger os recursos no local. Veja [Regras
de tabela de IPs](/docs/services/SecureGateway/securegateway_destination.html#dest-network-security) para obter mais informações sobre como configurar as regras iptables no
Secure Gateway.

#### Configurar Lista de Controle de Acesso (para o destino no local)
{: #secure-app-answer-acl}
Configure o suporte à Lista de Controle de Acesso para permitir ou restringir o acesso aos recursos no local para tornar os destinos no local mais seguros, especificando o direito de acesso no host e na porta de destino específicos. Recomenda-se definir as rotas HTTP/S permitidas ou restritas nas entradas ACL, bem como
aprimorar a segurança do destino no local. Veja [Lista de controle de acesso](/docs/services/SecureGateway/securegateway_acl.html#acl) e [Controle de rota HTTP/S usando a ACL](/docs/services/SecureGateway/securegateway_acl.html#acl-route-control) para obter informações.

#### Configurar a senha na IU do cliente Secure Gateway
{: #secure-app-answer-ui-pw}
Recomenda-se configurar a senha da IU para restringir o acesso da IU do cliente Secure Gateway. Veja [Interagindo com o cliente](/docs/services/SecureGateway/securegateway_interaction.html#client-interacting) para obter mais detalhes sobre como configurar a senha usando a configuração de inicialização ou os comandos interativos na linha de comandos do terminal do cliente Secure Gateway.

## O que é migração de gateway? Por que o domínio foi mudado após dezembro de 2018?
{: #faq-gateway-migration}

### Pergunta
{: #gateway-migration-question}
Após a manutenção de dezembro de 2018, há um botão de migração no painel do gateway. Para que serve esse botão? Por que o domínio foi mudado?

### Resposta
{: #gateway-migration-answer}

Após a manutenção de dezembro de 2018, o host em nuvem do Secure Gateway foi renomeado para
usar `securegateway.appdomain.cloud` em vez de `integration.ibmcloud.com`
e usar `securegateway.cloud.ibm.com` para autenticação de gateway em vez de
`bluemix.net`. Para compatibilidade com versões anteriores, o gateway existente
continuará a usar o domínio antigo até que ele seja migrado, e o cliente SG v180fp9 e anterior continuará
usando `bluemix.net` para autenticação de gateway.

Após a migração, o host em nuvem dos destinos no local mudará para usar o novo domínio, e os
usuários/aplicativos precisarão ser atualizados para enviar a solicitação para o novo host em nuvem.

Atualmente, a migração não é obrigatória e não há uma data exata para que o domínio antigo deixe de receber suporte, mas, quando isso for definido, o cliente que ainda estiver usando o nome de domínio antigo será notificado.

## Onde posso receber notificações?
{: #faq-notification}

### Pergunta
{: #notification-question}
Onde posso receber notificações do Secure Gateway, especialmente para manutenção disruptiva?

### Resposta
{: #notification-answer}

É possível receber notificações por meio de nossa [página
de status](https://console.bluemix.net/status). Para isso, procure Secure Gateway nessa página.

Quando o cliente Secure Gateway for desconectado inesperadamente, acesse a página de status
para verificar se há manutenção disruptiva no momento.
