---

copyright:
  years: 2015, 2021
lastupdated: "2021-03-17"

subcollection: SecureGateway

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:deprecated: .deprecated}
{:external: target="_blank" .external}


# ACL File
{: #acl-files}

{{_include-segments/list-note.md}}

```
#Lines preceded by a # will be ignored as a comment
#To allow specific port on a specific host:
#acl allow <hostname>:<port>

#To allow all ports on a specific host:
#acl allow <hostname>:

#To allow all hosts from a specific port:
#acl allow :<port>

#To allow all hosts and ports, use below command:
#acl allow :

#To deny specific port on a specific host:
#acl deny <hostname>:<port>

#To deny all ports on a specific host:
#acl deny <hostname>:

#To deny all hosts from a specific port:
#acl deny :<port>

#To deny all hosts and ports, use below command:
#acl deny :
```
{: codeblock}
