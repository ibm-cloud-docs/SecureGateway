---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---

# Log de mudanças do {{site.data.keyword.SecureGateway}}
{: #secure-gateway-change-log}

## v1.7.0 Fix Pack 1
{: #v171fp1}

### Correções
{: #v171fp1-fixes}

- Corrige o erro `EADDRINUSE` causado por listeners que não executam uma
desmontagem apropriadamente quando um destino é excluído.
- Resolve o problema em que as instâncias de serviço ligadas a um aplicativo não puderam ser excluídas.

## v1.7.0
{: #v170}

### Recursos
{: #v170-features}

- Introduz novos planos de serviço: Essentials, Professional e Enterprise.
- O cliente Secure Gateway agora suporta o proxy SOCKS entre si e o destino no local.

## v1.6.0 Fix Pack 1
{: #v160fp1}

### Correções
{: #v160fp1-fixes}

- Resolve o problema em que a desconexão de múltiplos clientes resulta em processos órfãos na matriz de clientes conectados.
- Agora limpa conexões órfãs que resultaram em listeners pausados incorretamente.

## v1.6.0
{: #v160}

### Recursos
{: #v160-features}

- A Lista de Controle de Acesso agora suporta roteamento de caminho específico para solicitações de HTTP/S.
- Os destinos agora suportam Indicadores do Nome do Servidor para conexões
TLS.

## v1.5.1
{: #v151}

### Recursos
{: #v151-features}

- Assistente de destino incluído para criação de destino.
- Gateways agora suportam o upload de um
certificado/par de chaves customizado.
- Os serviços estão agora limitados a 50 gateways e 50 destinos por gateway.
- Os Destinos/Gateways/Serviços podem agora ser exportados/importados.
- Testes de latência entre o cliente e o servidor capazes de serem executados por meio da IU do Secure Gateway.

## v1.5.0 Fix Pack 1
{: #v150fp1}

### Correções
{: #v150fp1-fixes}

- Limpa os processos de túnel órfãos na desconexão de rede.
- Resolve a alocação de porta duplicada causada pela exclusão do destino com falha.
- Resolve o problema de HTTP/S com a codificação não UTF8.
- Resolve manipuladores IPC ausentes nos processos de substituição.

## V1.5.0
{: #v150}

### Recursos
{: #v150-features}

- O cliente tem agora uma IU interativa local.
- As conexões de Protocolo UDP agora são suportadas.
- A área de cobertura da memória do cliente foi reduzida drasticamente.
- O modo de cliente único não é mais suportado; multicliente será padrão e transparente para um cliente de gateway único.
- A Lista de Controle de Acesso é sincronizada entre clientes conectados ao mesmo gateway.

## v1.4.2
{: #v142}

### Recursos
{: #v142-features}

- Os clientes são agora designados a um ID ao conectar-se ao servidor SG.
- Os clientes agora são finalizados remotamente por meio da API ou da IU do Secure Gateway.
- O status conectado de um cliente pode ser verificado por meio da API.
- O TLS do lado do destino suporta agora a Autenticação mútua.
- Destinos em nuvem suportam agora protocolos de Autenticação mútua.
