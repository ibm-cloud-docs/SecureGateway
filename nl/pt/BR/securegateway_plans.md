---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---

# Planos do serviço {{site.data.keyword.SecureGateway}}
{: #secure-gateway-service-plans}

Com a liberação da v1.7.0, um novo modelo de precificação de plano em camadas foi introduzido que permite que o {{site.data.keyword.SecureGateway}} escala de acordo com as necessidades de qualquer empresa.  A tabela a seguir fornece as limitações associadas a cada um dos planos publicamente disponíveis:

![Modelo de plano em camadas](./images/planDetails.png?raw=true "Modelo de plano em camadas")

## Mudando planos
{: #changing-plans}
Quando você mudar os planos, seu novo plano será considerado como um upgrade ou um downgrade.  Isso é determinado pelo número padrão de gateways permitidos por plano (por exemplo, Professional (5) para Enterprise (25) é um upgrade).  Ao fazer upgrade, não haverá interrupção do serviço, no entanto, um downgrade atualizará todos os gateways para que fiquem [inativos](/docs/services/SecureGateway?topic=securegateway-sg-faq#faq-states), o que exigirá a reativação dos gateways e dos destinos (até o seu novo limite de plano) antes da restauração do serviço.

<b>Nota</b>: ao executar a transição do plano Padrão para qualquer um dos novos planos, isso será considerado um downgrade.


## Notificação de limite excedido
{: #limit-exceeded-notifications}
Quando você criar a quantidade máxima permitida de gateway/destino, haverá um log de nível de aviso
mostrado no painel do gateway/destino.

Quando a quantia de clientes for atingida e houver uma nova tentativa do cliente de se conectar
ao gateway, haverá um log de nível de erro mostrado no log do cliente e o novo cliente será encerrado
pelo gateway.

Quando o uso de dados exceder o abono de transferência de dados, haverá um log de nível de erro
mostrado no log do cliente e o cliente será encerrado pelo gateway.
