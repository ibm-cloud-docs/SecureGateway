---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 문제점 해결
{: #troubleshooting}

## Secure Gateway 클라이언트 실행 우수 사례
{: #best}

- Secure Gateway 클라이언트를 클라이언트 자체로 브릿징된 서비스의 네트워크 가시성이 있는 운영 체제(OS) 파티션에서 실행하십시오. 
예를 들어, 일부 호스팅된 가시화 환경에서는 NAT 및 Bridged를 포함하여 다중 네트워크 연결 모드를 지원합니다. 반드시 인터넷에서 {{site.data.keyword.Bluemix}}
서비스에 대한 액세스를 제공하는 올바른 연결 유형을 선택하십시오.
- Secure Gateway를 회사 보안 정책에서 허용하는 IT 환경에 설치하십시오. 
일반적으로 이는 회사에서 온프레미스 자산을 보호하기 위해 적합한 보안 제어를 시행하는 보안 황색 구역이나 DMZ에 있습니다. Secure Gateway 클라이언트를 설치할 때 항상 회사 보안 정책 및 지시사항을 준수하십시오.
- 클라이언트를 사용자 환경에 설치하기 전에, 인터넷 및 온프레미스 자산이 둘 다 액세스 가능하며
모든 호스트 이름이 DNS에 의해 분석 가능한지 확인하십시오.

## 초기 문제점 해결 단계

- 요청 애플리케이션에서 실패한 요청을 시작하십시오.
- Secure Gateway 클라이언트 로그를 확인하십시오.
- 요청에서 클라이언트 로그가 생성되지 않은 경우 요청 애플리케이션과 Secure Gateway 서버 간의 문제입니다. 이 문제의 범위는 불일치가 발생한 네트워크 프로토콜에 대한 네트워크 신뢰성으로부터 부적절한 TLS 상호 인증 핸드쉐이크까지 해당될 수 있습니다.
- 클라이언트가 요청에서 오류 로그를 생성한 경우 SG 클라이언트와 온프레미스 리소스 간의 문제입니다. 다음은 일반적인 오류, 일반적으로 해당 오류를 유발하는 문제 및 해당 문제점을 해결하기 위한 잠재적인 방법이 포함된 표입니다.

오류 | 일반적인 원인 | 문제점 해결 방법
--- | --- | ---
ETIMEDOUT | 네트워크 제한조건으로 인해 클라이언트에서 연결할 호스트 이름/IP를 찾을 수 없습니다. | 네트워크 연결을 확인하기 위해 클라이언트를 실행 중인 호스트에서 대상의 호스트 이름/IP로 Ping을 실행하십시오. 클라이언트의 Docker 버전을 실행하고 `--net=host`를 사용하여 컨테이너를 호스트 OS와 브릿징하는 경우 문제가 해결될 수도 있습니다.
ECONNREFUSED | 클라이언트에서 연결할 호스트 이름/IP를 분석했지만 연결 핸드쉐이크를 시작할 수 없습니다. | 이 문제는 일반적으로 SG 클라이언트와 온프레미스 리소스 간의 프로토콜 불일치로 인해 발생합니다(예: 클라이언트에서 TLS 연결이 필요한 호스트:포트에 대해 TCP 연결을 시도함). 경우에 따라 방화벽 규칙에서 ETIMEDOUT 대신 이 오류를 유발할 수도 있습니다.
ECONNRESET | 클라이언트에서 대상에 대한 연결을 설정했지만 핸드쉐이크 중에(TLS 핸드쉐이크 오류의 경우 다른 오류를 유발할 수도 있음) 또는 온프레미스 리소스에서 요청을 처리하는 중에 문제가 발생했습니다. | 오류로 인해 연결이 중단되지 않았는지 확인하기 위해 온프레미스 리소스의 로그를 확인해야 합니다. 온프레미스 로그에서 아무 것도 발견되지 않을 경우 연결을 위해 클라이언트에 적절한 프로토콜(및 필요한 경우 인증서)이 제공되는지 확인하기 위해 대상 구성을 검사해야 합니다.
REMOTE_RST | SG 서버 측에서 오류가 발생했습니다. <br><br> 온프레미스 대상의 경우 요청 앱에서 SG 서버에 연결할 때 오류가 발생했거나 온프레미스 리소스로부터 데이터를 수신할 때 제한시간 초과 오류가 발생했습니다. <br><br> 클라우드 대상의 경우 TLS 핸드쉐이크 실패로부터 클라우드 리소스 오류까지 임의의 오류가 될 수 있습니다. | 온프레미스 대상의 경우 요청 앱에서 적절한 프로토콜을 사용하여 SG 서버와의 연결을 설정하는지 확인하십시오. 온프레미스 리소스로부터 데이터를 수신할 때 오류가 발생하는 경우 제한시간을 증가시키거나 사용 안함으로 설정하십시오. <br><br> 클라우드 대상의 경우 오류로 인해 연결이 중단되지 않았는지 확인하기 위해 클라우드 리소스의 로그를 확인해야 합니다. 클라우드 리소스 로그에서 아무 것도 발견되지 않을 경우 연결을 위해 클라이언트에 적절한 프로토콜(및 필요한 경우 인증서)이 제공되는지 확인하기 위해 대상 구성을 검사해야 합니다.

터널의 반대쪽 끝에서 ECONNRESET이 발생한 후 많은 애플리케이션이 "정지"됩니다. 이 상황은 예상된 결과입니다. Secure
Gateway의 경우 터널의 해당 끝에서 이미 TCP 패킷이 수신확인되었기 때문에 터널의 반대편 끝에서 RST 패킷을
재생할 수 없습니다. 애플리케이션 레벨 제한시간은 애플리케이션에서 수신확인 응답을 수신하지 않고 정지를 종료하기
위한 유일한 방법입니다.

## 서버가 다시 시작될 때 다시 시작되도록 Docker 클라이언트 구성
{: #docker}

### 현재 상황
Secure Gateway 클라이언트가 실행되는 서버를 다시 시작하는 경우 수동으로 Secure Gateway Docker 클라이언트를 다시 시작해야 합니다. 시스템이 다시 시작된 후 클라이언트를 자동으로 시작할 수 있는 방법은 다음과 같습니다.

### 수정 방법

- Linux 또는 UNIX 시스템:
- CRON 작업의 결과라고 할 수 있는 스크립트에
Docker 명령을 통합하십시오.
- Linux 워크스테이션을 사용하는 경우 서버가 다시 시작될 때마다 클라이언트가 시작되도록 시작 구성을 작성할 수 있습니다. 자세한 정보는 Docker 웹 사이트에서 컨테이너 자동 시작을 참조하십시오.
- Windows 시스템의 경우 다음 명령을 실행하여 클라이언트를 시작하십시오.

```
for /L %i in (0,0,0) do docker run -it ibmcom/secure-gateway-client <gateway_id>
```
{: pre}

## 연결 오류 메시지: 인증서의 CN에 호스트가 없음
{: #not-in-cn}

### 현재 상황
Secure Gateway 클라이언트를 사용하여 온프레미스 클라이언트 측 TLS를 구현하려고 시도하는 중에 다음과 같은 오류 메시지가 수신됩니다.

```
[ERROR] Connection #<connection ID> had error: Host: <host name>. is not cert's CN: <mycommonname>

Where:
    - connection ID is a client assigned connection number.
    - host name is the host name for the connection.
    - mycommonname is the FDQN or common name that is used in the certificate.
```
{: screen}

### 발생 이유
이 대상에 대해 {{site.data.keyword.Bluemix_notm}}에 업로드한
인증서 및 온프레미스 애플리케이션 간의 공통 이름(예: 서버 FQDN 또는 사용자 이름)이 일치하지 않습니다.

- 다음 항목을 확인하십시오.
- 인증서가 올바르게 생성되었으며 올바른 서버 FQDN 또는 호스트 이름을 사용했습니다.
- 이 클라이언트에 대해 {{site.data.keyword.Bluemix_notm}} 대상으로 올바른 인증서를 업로드했습니다.

### 수정 방법

 1. {{site.data.keyword.Bluemix_notm}} UI에서 Secure Gateway 대시보드로 이동하십시오.
 2. 대상을 선택한 후 편집 아이콘을 클릭하십시오.
 3. 인증서 업로드를 클릭하십시오.
 4. 온프레미스 시스템에 연결하기 위해 사용될 PEM 인증 파일을 업로드하십시오.

## 인증서의 목록에 IP가 없음
{: #san}

### 현재 상황
제공되는 인증서의 CN이 게이트웨이의 IP 주소이지만 인증서에 해당 IP 주소와 일치하는 SAN이 없으며 클라이언트가 연결에 실패합니다.  

호스트 이름 분석 문제 때문에 대상에서 IP 주소를 사용하고 있습니다. 제공되는 인증서의 CN이 게이트웨이의 IP 주소이지만 인증서에 해당 IP 주소와 일치하는 SAN이 없으며 클라이언트가 연결에 실패합니다.

TLS를 사용하여 대상을 작성했지만 대상의 호스트 이름을 사용하지 않고 IP 주소를 사용했습니다. 클라이언트를 연결하면 다음과 같은 오류가 처리됩니다.

```
[2015-10-15 13:00:04.866] [INFO] Connection #0 is being established to 10.3.20.31:443
[2015-10-15 13:00:05.426] [ERROR] Connection #0 to destination 10.3.20.31:443 had error: IP: 10.3.20.31 is not in the cert's list:
[2015-10-15 13:00:05.427] [INFO] Connection #0 to 10.3.20.31:443 was closed
```
{: screen}

### 발생 이유
현재 상황은 이 대상에서 호스트 이름이 아닌 IP 주소를 사용하기 때문에 게이트웨이 클라이언트의 SSL 확인 코드에서 이 대상을 다르게 처리하고 있는 것입니다. 인증서의 CN과 일치시키는 대신 인증서의 SAN에서 IP 주소 일치 항목을 찾고 있습니다. 인증서에 SAN이 없기 때문에 잘못된 연결로 간주하여 SSL 핸드쉐이크에 실패합니다.

### 수정 방법
오류 메시지를 확인하는 경우 CN에 대해 언급하지 않고(예: [ERROR] Connection ## had error: Host: . is not cert&apos;s CN: ) 자체 서명 인증서가 잘못 생성된 것으로 믿도록 인증서 목록이 표시됩니다. 문제점은 IP_Address가 포함된 FQDN 또는 CN을 사용하여 인증서를 생성하는 것입니다. SAN을 사용하는 경우 IP 주소만 지원되기 때문에 올바르게 작동하지 않습니다.

openssl을 사용하는 CN으로서의 IP가 포함된 인증서를 생성하는 방법:

1. openssl 구성 파일을 작성하십시오. /usr/lib/ssl/openssl.cnf에 IBM 구성 파일이 복사되어 있습니다.
2. 다음과 같이 파일에 alternate_names 섹션을 추가하십시오.

   ```
   [ alternate_names ]

   IP.1 = &lt;my application&apos;s ip&gt;
   ```
   {: codeblock}

3. [ v3_ca ] 섹션에 다음 행을 추가하십시오.

    ```
subjectAltName = @alternate_names
    ```
    {: pre}

4. CA_default 섹션에서 copy_extensions를 주석 처리하십시오(확장자 복사 옵션: 주의하여 사용하십시오.).

    ```
copy_extensions = copy
    ```
    {: pre}

5. 개인 키를 생성하십시오.

    ```
openssl genrsa -out private.key 3072
    ```
    {: pre}

6. 조직에 대한 옵션이 포함된 인증서를 생성하십시오.

    ```
    openssl req -new -x509 -key private.key -sha256 -out certificate.pem -days 730 -config
    ```
    {: pre}

7. 파일을 결합하십시오.

    ```
cat private.key certificate.pem > SAN.pem
    ```
    {: pre}

8. SAN.pem 파일을 클라이언트 TLS 인증서로 대상에 로드하십시오.
9. SAN.pem 파일을 온프레미스 애플리케이션에 로드한 후 다시 시작하십시오.

10. 대상에 TCP, HTTP 또는 HTTPS를 구성할 수 있으며, 이제 클라우드 측 애플리케이션에서 온프레미스 애플리케이션에 연결할 수 있습니다.

UNABLE_TO_VERIFY_LEAF_SIGNATURE 문제점이 발생하는 경우 openssl.conf 파일을 확인한 후 다음 코드를 변경하십시오.

    ```
keyUsage = digitalSignature, keyEncipherment
    ```
    {: pre}

기본값으로
변경하십시오.

    ```
    keyUsage = nonRepudiation, digitalSignature, keyEncipherment
    ```
    {: pre}

## 연결 오류 메시지: DEPTH_ZERO_SELF_SIGNED_CERT
{: #depth-zero}

### 현재 상황
Secure Gateway 클라이언트를 사용하여 온프레미스 클라이언트 측 TLS를 구현하려고 시도하는 중에 다음과 같은 오류 메시지가 수신됩니다.

```
[ERROR] Connection #<connection ID> had error: DEPTH_ZERO_SELF_SIGNED_CERT Where:

    connection ID is a client assigned connection number.
```
{: screen}

### 발생 이유
정의한 대상에 클라이언트측 인증서가 누락되어 있습니다.

### 수정 방법
 1. {{site.data.keyword.Bluemix_notm}} UI에서 Secure Gateway 대시보드로 이동하십시오.
 2. 대상을 선택한 후 편집 아이콘을 클릭하십시오.
 3. 인증서 업로드를 클릭하십시오.
 4. 온프레미스 시스템에 연결하기 위해 사용될 PEM 인증 파일을 업로드하십시오.


## Docker 클라이언트에서 대화식으로 ACL 파일을 로드하는 방법은 무엇입니까?
{: #docker-acl}

### 현재 상황
Docker는 컨테이너 또는 가상화된 환경이므로 컨테이너가 실제로 실행될 때까지는 파일 시스템에 직접 액세스할 수 없습니다. 따라서 실제로 시작 및 실행될 때까지는 호스트 시스템의 파일 시스템을 읽어들일 수 없습니다.

### 수정 방법
수행할 수 있는 방법은 다음과 같습니다.

- aclfile.txt를 포함시킬 Dockerfile을 작성하십시오.

```
FROM ibmcom/secure-gateway-client
ADD aclfile.txt /tmp/aclfile.txt
```
{: codeblock}

- 새 Docker 이미지를 빌드하십시오.

```
docker build -t ads-secure-gateway-client .
```
{: pre}

- 새 Docker 이미지를 실행하십시오(-t 및 -i 옵션을 지정해야 하며, 그렇지 않을 경우 파일을 찾을 수 없음 또는 ENOENT 오류 발생).

```
docker run -t -i ads-secure-gateway-client1  --F /tmp/aclfile.txt
```
{: pre}

- 다음과 같은 출력이 표시됩니다.

```
[2015-09-30 16:50:32.084] [INFO] The current access control list is being reset and replaced by the user provided file: /tmp/aclfile.txt
[2015-09-30 16:50:32.086] [INFO] The ACL file process accepts acl allow :8000
[2015-09-30 16:50:32.087] [INFO] The ACL file process accepts acl deny local
```
{: screen}

## 추가적인 도움말 및 지원 받기
{: #support}

Secure Gateway를 사용하여 애플리케이션을 개발하거나 배치하는 방법에 대한 기술적인 질문이 있는 경우 [스택 오버플로우 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://stackoverflow.com/search?q=securegateway+ibm-bluemix)에 질문을 게시하십시오. {{site.data.keyword.Bluemix_notm}} 개발 팀에서 쉽게 발견할 수 있도록 질문에 "ibm-bluemix" 및 "secure-gateway" 태그를 지정하십시오.

서비스 또는 지시사항을 시작하는 방법에 대한 질문이 있는 경우 "bluemix" 및 "Secure Gateway" 태그와 함께 [IBM developerWorks dW Answers ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.ibm.com/answers/topics/securegateway/?smartspace=bluemix) 포럼을 사용하십시오.

이러한 포럼을 사용하는 방법에 대한 자세한 정보는 [여기에서 도움말 페이지 받기](https://console.ng.bluemix.net/docs/support/index.html#getting-help)를 확인하십시오.

IBM 지원 티켓 열기 또는 지원 레벨과 티켓 심각도에 대한 정보는 [지원 문의](https://console.ng.bluemix.net/docs/support/index.html#contacting-support)를 참조하십시오.

티켓을 제출할 때 다음과 같은 정보를 최대한 많이 제공해 주십시오.

- 클라이언트가 실행되고 있는 OS가 무엇입니까?
- 사용 중인 클라이언트의 버전이 무엇입니까(클라이언트에서 'C' 명령을 사용하여 찾을 수 있음)?
- UI 문제의 경우 연관된 브라우저 콘솔 로그 및 스크린샷을 모두 붙여넣거나 첨부하십시오.
- 연관된 요청 애플리케이션 로그를 모두 붙여넣거나 첨부하십시오.
- 연관된 Secure Gateway 클라이언트 로그를 모두 붙여넣거나 첨부하십시오.
- 사용 중인 대상의 세부사항을 제공하십시오(스크린샷 또는 다음 필드 채우기).
   - 대상 ID
   - 프로토콜
   - 대상 측 인증
   - 업로드된 인증서(이름 및 업로드된 상자만)
