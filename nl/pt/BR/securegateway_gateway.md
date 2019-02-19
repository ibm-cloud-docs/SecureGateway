---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---

# Incluindo um gateway

Um gateway pode ser pensado como uma maneira de identificar uma rede ou um ambiente específico. É o que um cliente Secure Gateway usará para estabelecer conectividade com os servidores Secure Gateway e pode conter múltiplas definições de recursos ou destinos.

![Painel do Secure Gateway](./images/newDashboard.png?raw=true "Painel do Secure Gateway")

No Painel do Secure Gateway, clique no botão Incluir gateway para abrir o painel Incluir gateway.

![Incluir gateway](./images/addGateway.png?raw=true "Incluir gateway")

A única entrada necessária nesse painel é o Nome do gateway. Por padrão,`Require security token` e `Token Expiration` estão selecionados.

Ao requerer o token de segurança para conectar clientes, cada vez que iniciar um cliente Secure Gateway, você precisará fornecer o ID do gateway e o Token de segurança. Se desmarcar a caixa `Require security token`, você terá somente que fornecer o ID do gateway para o cliente se conectar.

A data de expiração padrão para o Token de segurança é de 90 dias a partir de quando ela é criada. Para mudar a data de expiração, mantenha a caixa `Token Expiration` marcada e edite o campo de texto com o número de dias que você deseja que o token expire (mínimo 1, máximo 365). Para criar um token que não expire, desmarque a caixa `Token Expiration`.  

Para concluir a criação de seu gateway, clique em Incluir gateway. Depois que o gateway for criado, a página navegará automaticamente para o novo gateway.

![Novo gateway](./images/newGateway.png?raw=true "Novo gateway")

Agora que você tem seu gateway recém-criado, é possível [conectar seu primeiro cliente](/docs/services/SecureGateway/securegateway_client.html).
