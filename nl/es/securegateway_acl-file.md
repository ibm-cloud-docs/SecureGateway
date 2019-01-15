---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# ACL File

```
#Las líneas precedidas por un signo # se pasan por alto como comentario
#Para permitir un puerto específico en un host específico:
#acl allow <hostname>:<port>

#Para permitir todos los puertos en un host específico:
#acl allow <hostname>:

#Para permitir todos los hosts desde un puerto específico:
#acl allow :<port>

#Para permitir todos los hosts y puertos, utilice este mandato:
#acl allow :

#Para denegar un puerto específico en un host específico:
#acl deny <hostname>:<port>

#Para denegar todos los puertos en un host específico:
#acl deny <hostname>:

#Para denegar todos los hosts de un puerto específico:
#acl deny :<port>

#Para denegar todos los hosts y puertos, utilice este mandato:
#acl deny :
```
{: codeblock}
