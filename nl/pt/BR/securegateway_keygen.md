---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Gerenciamento de certificado/chave
{: #cert-key-management}

## Gerando um par certificado autoassinado/chave
{: #self-signed-cert-gen}

Para gerar um par certificado autoassinado/chave, execute o comando a seguir:

```
openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 -keyout serverKey.pem -out serverCert.pem
```
{: pre}


## Fazendo upload de um par certificado/chave no navegador
{: #upload-cert-to-browser}

Para acessar um destino que imponha a autenticação mútua, deve-se converter seu par de
certificado/chave em um arquivo PKCS#12 e carregá-lo em seu navegador.

Para criar o arquivo PKCS#12, use o comando a seguir:

```
openssl pkcs12 -export -in <path_to_cert> -inkey <path_to_key> -name "my-sg-pair" -out <path_to_write_to>.p12
```
{: pre}

No Firefox, o arquivo .p12 pode ser transferido por upload para Opções > Preferências > Avançado > Certificados > Visualizar certificados > Seus certificados > Importar

No Chrome, o arquivo .p12 pode ser transferido por upload para Configurações > HTTPS/SSL > Gerenciar certificados > Importar

No Internet Explorer, o arquivo .p12 pode ser transferido por upload para Configurações > Conteúdo > Certificados > Importar
