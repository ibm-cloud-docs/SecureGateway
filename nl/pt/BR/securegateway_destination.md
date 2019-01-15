---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---

# Incluindo um destino

Um destino é uma definição de como se conectar a um recurso no local ou em nuvem específico. Depois que o destino tiver sido criado, os servidores {{site.data.keyword.SecureGateway}} fornecerão a ele um terminal público exclusivo no qual ele atenderá as conexões enquanto o gateway estiver conectado.

![Painel sem destinos](./images/emptyDestinations.png?raw=true "Painel sem destinos")

De dentro de seu novo gateway e na guia Destinos, clique no botão Incluir destino para abrir o painel Incluir destino. Há dois métodos para criar seu destino: uma configuração orientada que mostra como cada parte se ajusta à arquitetura geral e em uma configuração avançada que fornece todos os campos e opções em um único painel.  

A configuração guiada <b>não</b> permite a configuração de informações de proxy, indicadores de nome do servidor ou o upload de um par certificado/chave específico a um destino. Após a criação, todos os campos estão disponíveis por meio do painel Editar destino.

## Painel guiado

![Configuração guiada](./images/guidedLanding.png?raw=true "Painel de entrada Configuração guiada")

## Painel avançado

![Configuração avançada](./images/advancedLanding.png?raw=true "Painel de entrada Configuração avançada")

## Destinos no local versus destinos em nuvem
{: #dest-types}

A primeira pergunta a ser respondida ao criar seu destino é onde reside o recurso ao qual você precisa se conectar.

### Destino no local
O destino no local é para o caso de uso em que um aplicativo no espaço público precisa de acesso a um recurso restrito localizado no local.
![Destino no local](./images/onPremDestination.png?raw=true "Destino no local")

### Destino em Nuvem
O destino em nuvem é para o caso de uso em que um aplicativo localizado em uma rede restrita precisa de acesso a um recurso que está disponível em um espaço público.
![Destino reverso](./images/reverseDestination.png?raw=true "Destino em nuvem")

## Definindo seu destino
Para ambos os tipos de destinos, as informações a seguir são necessárias:

- <b>Nome do host do recurso</b>: esse é o IP ou o nome do host do recurso ao qual você precisa se conectar.
- <b>Porta do recurso</b>: essa é a porta na qual seu recurso está atendendo.
- <b>Protocolo</b>: esse é o tipo de conexão que seu aplicativo fará. Veja a tabela a seguir para obter as várias [opções de protocolo](#protocols). Para configurar o tipo de conexão que seu recurso está esperando, verifique a seção [Autenticação do recurso](#resource-auth).

Se você tiver selecionado um destino em nuvem, também será necessário fornecer uma <b>Porta do cliente</b>. Essa é a porta na qual o cliente {{site.data.keyword.SecureGateway}} atenderá para permitir conexões com o nome do host e a porta do recurso associados.

## Opções de protocolo
{: #protocols}

Esta tabela fornece todas as opções disponíveis para como seu aplicativo pode iniciar conexões/solicitações com o {{site.data.keyword.SecureGateway}}.

Protocolo | Descrição
-- | --
TCP | Nenhuma autenticação. Seu aplicativo pode se comunicar diretamente com os servidores {{site.data.keyword.SecureGateway}} sem requerer nenhum certificado.
TLS: Lado do Servidor | A Segurança da Camada de Transporte (TLS) é ativada e o servidor fornece um certificado para provar sua autenticidade.
TLS: Autenticação Mútua | O servidor fornece um conjunto de certificados e seu aplicativo também deve fornecer um certificado como parte de sua conexão.
HTTP | Conexão TCP na qual o cabeçalho do host é regravado para corresponder ao nome do host no local.
HTTPS | TLS: conexão do lado do servidor em que o cabeçalho do host é regravado para corresponder ao nome do host no local.
HTTPS: autenticação mútua | TLS: conexão de autenticação mútua em que o cabeçalho do host é regravado para corresponder ao nome do host no local.


## Configurando a autenticação mútua
{: #mutual-auth}

Para protocolos que cumprem a autenticação mútua, será necessário fazer upload de seu próprio certificado ou o servidor criará automaticamente um par certificado autoassinado/chave para ser usado por seu aplicativo. Esse par pode ser transferido por download juntamente com o certificado do servidor.
![Painéis de autenticação mútua](./images/mutualAuth.png?raw=true "Painéis de autenticação mútua")

### Autenticação do usuário
{: #user-auth}

A seção de autenticação do usuário é para gerenciar a autorização de seu aplicativo de solicitação/conexão com o {{site.data.keyword.SecureGateway}}. Esse campo aceita um único certificado que deve ser o certificado que seu aplicativo apresentará junto a qualquer conexão/solicitação.

### Autenticação do recurso
{: #resource-auth}

A autenticação do recurso determina como o {{site.data.keyword.SecureGateway}} tentará se conectar ao recurso definido. Há três opções disponíveis: Nenhuma, TLS (lado do servidor) e Autenticação mútua. Dependendo de sua escolha, opções de autenticação diferentes são disponibilizadas.

A ativação do TLS na conexão com seu recurso é separada do TLS usado para Autenticação do usuário. O TLS para Autenticação do usuário protege o acesso entre o aplicativo de solicitação inicial e o {{site.data.keyword.SecureGateway}} (por exemplo, entre seu app {{site.data.keyword.Bluemix_notm}} e os servidores {{site.data.keyword.SecureGateway}}) enquanto o TLS para Autenticação do recurso protege a conexão entre o {{site.data.keyword.SecureGateway}} e seu recurso definido (por exemplo, entre o cliente {{site.data.keyword.SecureGateway}} e seu banco de dados no local).

#### Autenticação em nuvem/no local

Essa opção se torna disponível selecionando TLS ou Autenticação mútua para a Autenticação do recurso. O nome do campo corresponderá ao [tipo de destino](#dest-types) que você escolheu. Esse campo permite que até 6 certificados sejam transferidos por upload a fim de validar o certificado do recurso ao qual você está se conectando. Esses arquivos serão incluídos na CA de conexão com o recurso e deverão conter o certificado ou a cadeia de certificados que seu recurso apresentará.

#### Server Name Indicator (SNI)
Essa opção se torna disponível selecionando TLS ou Autenticação mútua para a Autenticação do recurso. Isso é usado para permitir que um nome do host separado seja fornecido para o handshake TLS da conexão de recurso.

### Certificado de cliente e Chave
O local em que os campos Certificado de cliente e Chave aparecem depende do [tipo de destino](#dest-types) escolhido. Em ambas as situações, os arquivos fornecidos aqui serão usados pelo cliente SG para identificar-se para conexões TLS. Se nenhum arquivo for transferido por upload, os servidores {{site.data.keyword.SecureGateway}} gerarão automaticamente um par autoassinado com um CN de `localhost`. Para obter instruções sobre como gerar um par certificado/chave, [clique aqui](./securegateway_keygen.html).

Para um destino no local, ele aparecerá sob Autenticação do recurso se Autenticação do recurso: autenticação mútua tiver sido selecionada. Nesse caso, o cliente usará esse par certificado/chave para sua conexão de saída com o recurso definido. A CA dessa conexão conterá os certificados fornecidos no campo [Autenticação em nuvem/no local](#resource-auth).

Para um destino em nuvem, ele aparecerá sob Autenticação do usuário se um protocolo TLS tiver sido selecionado. Nesse caso, o cliente usará esse par certificado/chave para estabelecer listeners TLS com o arquivo transferido por upload para a [Autenticação do usuário](#user-auth) na CA.  

## Configurando a segurança de rede
Para evitar que todos os endereços IP específicos se conectem ao seu host e porta em nuvem, é possível escolher aplicar regras de iptable em seu destino no local.
![Painel Segurança de rede](./images/networkSecurity.png?raw=true "Painel Segurança de rede")

Para cumprir regras de iptable, marque a caixa <b>Restringir acesso à nuvem para este destino com regras iptable</b> do painel Segurança de rede. Quando a caixa estiver marcada, será possível começar a incluir os IPs que devem ter permissão para se conectar. Se nenhum IP for fornecido, todas as conexões com esse host e porta em nuvem serão rejeitadas, contanto que a caixa <b>Restringir acesso à nuvem</b> esteja marcada.

<b>Nota</b>: os IPs ou as portas fornecidos devem ser o endereço IP externo que os servidores {{site.data.keyword.SecureGateway}} verão, não o endereço IP local da máquina que está fazendo a solicitação.

### Incluindo regras de iptable
Ao incluir regras em iptables, é possível fornecer IPs individuais ou um intervalo de IP junto com uma porta única ou um intervalo de portas. Todos os intervalos fornecidos são inclusivos. A tabela a seguir apresenta alguns exemplos, bem como sua forma de resolução dentro das iptables:

Endereços IP | Portas | Resultados
-- | -- | --
1.2.3.4 | 5000 | Somente o IP 1.2.3.4 da porta 5000 será permitido.
1.2.3.4-2.3.4.5 | 5000 |Todos os IPs entre 1.2.3.4 e 2.3.4.5 da porta 5000 serão permitidos.
1.2.3.4 | 5000:10000 | Somente o IP 1.2.3.4 das portas 5000 e 10000 será permitido.
1.2.3.4-2.3.4.5 | 5000:10000 |Todos os IPs entre 1.2.3.4 e 2.3.4.5 das portas 5000 a 10000 serão permitidos.
1.2.3.4 | | Somente o IP 1.2.3.4 de qualquer porta será permitido.
| 5000 | Qualquer IP da porta 5000 será permitido.

Regras específicas também podem ser associadas a um aplicativo. Para obter mais informações sobre como criar regras associadas, veja [como criar regras de iptable para seu app](./iptables.html).

## Configurando opções de proxy
Se o destino no local estiver localizado atrás de um proxy SOCKS, será possível configurar as configurações de proxy para seu destino no painel Opções de proxy.
![Painel Opções de proxy](./images/proxyOptions.png?raw=true "Painel Opções de proxy")

Para definir as configurações de proxy, é necessário fornecer somente o nome do host e a porta em que o proxy está atendendo, bem como o protocolo SOCKS que está sendo usado (4, 4a, 5).

## Configurações de destino
Depois que seu destino tiver sido criado, clique no ícone de configurações para ver as informações a seguir:

- O ID de destino necessário para usar a API.
- <b>Os destinos no local terão um host e uma porta em nuvem. Essas informações são requeridas por seu aplicativo a fim de se conectar ao seu recurso no local.</b>
- <b>Os destinos em nuvem terão uma porta do cliente. Essa é a porta na qual o cliente {{site.data.keyword.SecureGateway}} atenderá para conectar seu aplicativo no local ao seu recurso em nuvem.</b>
- O host e a porta do recurso para os quais esse destino está apontado.
- Quando o destino foi criado, assim como o usuário que o criou.
- Quando o destino foi modificado pela última vez, assim como o usuário que o modificou.
