---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 인증서/키 관리
{: #cert-key-management}

## 자체 서명 인증서/키 쌍 생성

자체 서명 인증서/키 쌍을 생성하려면 다음 명령을 실행하십시오.

```
openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 -keyout serverKey.pem -out serverCert.pem
```
{: pre}


## 브라우저에 인증서/키 쌍 업로드

상호 인증을 적용하는 대상에 액세스하려면 인증서/키 쌍을 PKCS#12 파일로 변환하여 브라우저에 업로드해야 합니다.

PKCS#12 파일을 작성하려면 다음 명령을 사용하십시오.

```
openssl pkcs12 -export -in <path_to_cert> -inkey <path_to_key> -name "my-sg-pair" -out <path_to_write_to>.p12
```
{: pre}

Firefox의 경우 .p12 파일을 옵션 > 환경 설정 > 고급 > 인증서 > 인증서 보기 > 인증서 > 가져오기에 업로드할 수 있습니다.

Chrome의 경우 .p12 파일을 설정 > HTTPS/SSL > 인증서 관리 > 가져오기에 업로드할 수 있습니다.

Internet Explorer의 경우 .p12 파일을 설정 > 컨텐츠 > 인증서 > 가져오기에 업로드할 수 있습니다.
