---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---

# Sobre o {{site.data.keyword.SecureGateway}}
{: #about-sg}

## Como ele é seguro?
O {{site.data.keyword.SecureGatewayfull}} mantém uma conexão única criptografada persistente (TLS v1.2) entre o cliente {{site.data.keyword.SecureGateway}} (na rede no local) e os servidores {{site.data.keyword.SecureGateway}}. Com essa conexão bidirecional, somos capazes de transmitir com segurança dados entre seus recursos em nuvem e seus recursos no local. Para as conexões em cada extremidade (entre o recurso em nuvem e os servidores SG e entre os recursos no local e o cliente SG), o usuário define o protocolo (TCP, TLS, HTTP, HTTPS, UDP) e as medidas de segurança (autenticação mútua, iptables) colocadas nelas.  

## Suporte bidirecional
Combinando o destino no local tradicional com o novo destino em nuvem, obtemos o suporte bidirecional completo. O novo destino em nuvem permite que um serviço/aplicativo no local envie informações/solicitações para um aplicativo em nuvem por meio do cliente {{site.data.keyword.SecureGateway}}. Ao criar um novo destino em nuvem, o destino conterá `resource host` (o nome do host ou o IP do aplicativo em nuvem), `resource port` (a porta na qual o aplicativo em nuvem atenderá) e `client port` (a porta na qual o cliente {{site.data.keyword.SecureGateway}} atenderá). Depois que esse destino for estabelecido, o serviço no local poderá, então, conectar-se à porta do cliente e começar a enviar dados que serão enviados por meio do cliente para o servidor {{site.data.keyword.SecureGateway}} e, em seguida, para o aplicativo em nuvem especificado.

A porta do cliente que é fornecida para um destino deve ser exclusiva dentro do gateway associado.  Por exemplo, um único gateway não pode ter dois destinos reversos atendendo na porta 12000, mas cada um dos dois gateways pode ter seu próprio destino atendendo na porta 12000. No entanto, no último caso, se ambos os gateways estiverem conectados ao mesmo cliente, somente um dos destinos será capaz de atender na porta 12000.

### Destino em Nuvem
![Destino em nuvem](./images/reverseDestination.png?raw=true "Destino em nuvem")

### Destino no local
![Destino no local](./images/onPremDestination.png?raw=true "Destino no local")

## Alta disponibilidade
Para alcançar a alta disponibilidade, crie várias conexões do cliente com o mesmo ID do
gateway.  Isso pode ser feito conectando múltiplos clientes de gateway único ao mesmo ID do gateway e/ou emitindo múltiplas conexões em um único cliente de diversos gateways para o mesmo ID do gateway. As conexões serão distribuídas entre todos os clientes conectados em um modo round-robin.
