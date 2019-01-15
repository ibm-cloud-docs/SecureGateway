---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---

# Gerenciando seu serviço {{site.data.keyword.SecureGateway}}

## {{site.data.keyword.SecureGateway}} Dashboard
Em seu painel do {{site.data.keyword.SecureGateway}}, é possível ver rapidamente os detalhes a seguir:

- O número atual de conexões com seus destinos em todos os gateways.
- O fluxo de dados de entrada entre todos os gateways nas últimas 12 horas.
- O fluxo de dados de saída entre todos os gateways nas últimas 12 horas.
- Um gráfico exibindo as últimas 12 horas de uso em uma base por gateway.
- Seus gateways existentes, seu status atual e o número de destinos que eles contêm.

![Painel do {{site.data.keyword.SecureGateway}} com uso](./images/dashboardUsage.png?raw=true "Painel do {{site.data.keyword.SecureGateway}} com uso")

### Perfil de serviço
Clicando no botão Perfil, é possível ver:

Campo | Descrição
-- | --
Nível de plano | Seu plano de serviço atualmente selecionado.
Limite de gateway | O número de gateways que você pode ter antes de ser cobrado por gateways extras.
Limite de clientes | O número de clientes que podem ser conectados a qualquer gateway único.
Limite de destino | O número de destinos que qualquer gateway único pode conter.
Limite de dados | A quantia de transferência de dados (em GB) que seu plano suporta antes de ser cobrado por um excedente de dados.
Excedentes de gateway | O número atual de excedente de gateway pelos qual você está sendo cobrado.
Excedentes de dados | O número de excedentes de dados incorridos para o ciclo de faturamento do mês atual.

### Painel de informações do gateway
Clicando no botão Configurações em qualquer um dos tiles de gateway, é possível ver:

- O token de segurança necessário para usar a API ou para conectar um cliente, dependendo de se o token está sendo cumprido nas conexões do cliente.
- O ID do gateway necessário para usar a API ou para conectar um cliente.
- O nó no qual seu gateway foi criado. Esse é o servidor ao qual seu cliente se conectará.
- Quando o gateway foi criado e o usuário que o criou.
- Quando o gateway foi modificado pela última vez e o usuário que o modificou.
- Um link para gerar novamente o certificado e a chave associados ao gateway. Esses arquivos são usados somente com destinos anteriores que não contêm seu próprio par certificado/chave para autenticação mútua no cliente.

As tarefas a seguir também podem ser concluídas por meio do painel Informações do gateway:

- Edite ou visualize mais informações sobre seu gateway e token de segurança. Um novo painel será exibido quando você selecionar essa opção.
- Desative ou ative o gateway.
- Exclua o gateway.

## Painel para um determinado gateway
Clicando em um dos tiles de gateway, o painel para esse gateway específico é exibido. De forma semelhante ao painel principal do {{site.data.keyword.SecureGateway}}, é possível ver rapidamente os detalhes a seguir:

- O número atual de conexões com os destinos nesse gateway.
- O fluxo de dados de entrada nesse gateway nas últimas 12 horas.
- O fluxo de dados de saída nesse gateway nas últimas 12 horas.
- Um gráfico exibindo as últimas 12 horas de uso em uma base por destino.
- Seus destinos existentes, seu status atual e seu número atual de conexões ativas.

![Painel para um gateway específico](./images/viewGateway.png?raw=true "Painel para um gateway específico")

### Painel de informações de destino
Clicando no botão Configurações em qualquer um dos tiles de destino, é possível ver:

- O ID de destino necessário para usar a API.
- Os destinos no local terão um host e uma porta em nuvem. Essas informações são requeridas por seu aplicativo a fim de se conectar ao seu recurso no local.
- Os destinos em nuvem terão uma porta do cliente. Essa é a porta na qual o cliente {{site.data.keyword.SecureGateway}} atenderá para conectar seu aplicativo no local ao seu recurso em nuvem.
- O host e a porta do recurso para os quais esse destino está apontado.
- Quando o destino foi criado, assim como o usuário que o criou.
- Quando o destino foi modificado pela última vez, assim como o usuário que o modificou.

As tarefas a seguir também podem ser concluídas por meio do painel Informações de destino:

- Edite ou visualize mais informações sobre o destino. Um novo painel será exibido quando você selecionar essa opção.
- Desative ou ative o destino.
- Exclua o destino.
