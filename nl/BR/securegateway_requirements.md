---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---

# Requisitos para executar o cliente

## Requisitos do sistema
{: #system}

O cliente Secure Gateway é suportado nos ambientes a seguir:

| Nome | Versões          |
| ------------- | ----------- |
| Windows Desktop | 7, 8.1, 10 e superior |
| Windows Server | 2012 R2 e superior |
| Red Hat Linux | 6.5 e superior |
| CentOS | 7.2 e superior |
| SuSE Linux | 11.0<sup>*</sup> e superior |
| Ubuntu Linux | 14.04 (LTS) e superior |
| Power Machine | Arquitetura ppc64el |
| Ubuntu Z-Linux | - |
| AIX | 7.1 e superior |
| Mac OS X | 10.10 (Yosemite) e superior |
| Docker | 1.7.0 e superior, todos os sistemas operacionais suportados |

<sup> * </sup>- o suporte do SLES 11 está somente com o Service Pack 4

<b>Nota:</b> somente ambientes de 64 bits são suportados atualmente para instalação de cliente nativo.

## Requisitos de rede
{: #network}

O cliente Secure Gateway usa a porta de saída 443 e a porta 9000 para conectar-se ao registro do npm e ao ambiente do {{site.data.keyword.Bluemix}}:
- Porta 443 para instalação do npm
  - Durante a instalação, o instalador se conectará ao registro do npm e executará `npm install` para instalar as dependências requeridas pelo cliente Secure Gateway. Antes da instalação, assegure-se de que a máquina na qual o cliente será instalado possa se conectar a um website do registro do npm. O npm está configurado para usar o registro público do npm, Inc. em https://registry.npmjs.org por padrão. <br><br>
Se houver o servidor npm Enterprise em seu ambiente, inclua na lista de desbloqueio todas as dependências do cliente Secure Gateway no servidor npm Enterprise. Para obter a lista de dependências, consulte o arquivo `<Installation_directory>\ibm\securegateway\client\package.json`.<br><br>

- Porta 443 para autenticação de gateway


  | Região  | Host  |
  | --  | --  |
  | Sul dos EUA  | sgmanager.ng.bluemix.net  |
  | Leste dos EUA  | sgmanager.us-east.bluemix.net  |
  | Reino Unido  | sgmanager.eu-gb.bluemix.net  |
  | Alemanha  | sgmanager.eu-de.bluemix.net  |
  | Sydney  | sgmanager.au-syd.bluemix.net  |


- Porta 9000

  O nó do gateway do SG, que pode ser localizado na configuração do gateway. Cada gateway não estará no mesmo nó, então confirme o nome do host do nó toda vez que você criar o gateway


Assegure-se de verificar ou modificar
regras adicionais de firewall e de Tabela de IP que possam se aplicar. Se os seus administradores de rede requerem IPs específicos, [entre em contato com o suporte para solicitá-los para seu ambiente](./securegateway_troubleshooting.html#support).


## Determinando requisitos de hardware
{: #hardware}

As especificações da máquina que executa o cliente Secure Gateway dependem em grande parte do tráfego que passará pela conexão. Cada instância do cliente é um processo individual que fornece até 250 conexões simultâneas.  Para chegar a uma média máxima que a máquina precisaria suportar, determine o tamanho médio de uma transação em todo o cliente (tanto solicitação quanto resposta) e, em seguida, escale isso para 250 transações simultâneas. Dado que esse número combinou os tamanhos de solicitação e de resposta, o cliente não deve exceder a área de cobertura da memória. Para estimativas mais precisas, uma mistura de tamanhos de solicitação e tamanhos de resposta deve ser usada para simular melhor um cenário de transação no mundo real.

Para executar uma única instância do cliente Secure Gateway, sugerimos um mínimo de 2 núcleos e 4 GB de memória.

Para cenários de HA que executarão múltiplas instâncias do cliente na mesma máquina, sugerimos um mínimo de 4 núcleos e 8 GB de memória.
