---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 액세스 제어 목록

{{site.data.keyword.SecureGateway}} 클라이언트는 임베디드 액세스 제어 목록(ACL) 지원을 제공합니다. 클라이언트의 ACL을 수정하여 온프레미스 리소스에 대한 액세스를 허용하거나 제한(거부)할 수 있습니다. 이 작업은 클라이언트 명령을 사용하거나 적용하려는 ACL이 포함된 파일을 지정하여 대화식으로 수행할 수 있습니다.

v1.5.0부터는 동일한 게이트웨이에 연결된 모든 클라이언트에서 액세스 제어 목록 규칙이 동기화됩니다. 이 경우 단일 클라이언트에서만 ACL을 설정/업데이트하면 해당 게이트웨이에 연결된 실행 중인 모든 클라이언트에서 해당 설정/업데이트가 공유됩니다. ACL은 또한 세션 간에 지속되므로 새 클라이언트 연결 역시 동일한 ACL 규칙이 적용됩니다.

## 액세스 제어 목록 명령
{: #commands}

지원되는 ACL 명령은 다음과 같습니다.

```
acl allow [<hostname>]:[<port>]
acl deny [<hostname>]:[<port>]
no acl [<hostname>]:[<port>]
no acl
show acl
```
{: screen}

호스트 이름 또는 포트를 비워둔 양식의 경우 모든 호스트 이름 또는 포트를 의미합니다. 예를 들어 다음은 포트 22에 대한 모든 호스트 이름을 허용하는 ACL 규칙입니다.

```
acl allow :22
```
{: pre}

다음은 모든 포트에 대한 모든 호스트 이름을 허용하는 다른 ACL 규칙 예제이며 기본적으로 ACL 지원이 사용 안함으로 설정됩니다. <b>이 방법은 권장하지 않습니다.</b>

```
acl allow :
```
{: pre}

`show acl` 명령은 현재 설정되어 있는 ACL을 보여주거나 전체 설정에 대한 메시지를 제공합니다.

[시작하기 - 클라이언트 추가](./securegateway_client.html)로 돌아가십시오.

## ACL을 사용한 HTTP/S 라우트 제어
{: #routes}

v1.6.0부터는 HTTP/S 대상에서도 ACL 항목에 있는 특정 라우트를 적용할 수 있습니다. 이러한 라우트는 일반 ACL 항목과 동일한 방식으로 추가되지만 규칙의 끝 부분에 경로가 추가됩니다. 예를 들어 다음 명령은 /my/api 경로 다음에 오는 요청만 통과하도록 허용합니다.

```
acl allow localhost:80/my/api
```
{: pre}

이 규칙이 적용되면 `<cloud host>:<cloud port>/my/api*`에 대한 요청이 통과하도록 허용됩니다.

라우트는 `acl allow` 명령에서만 지원됩니다.

## 액세스 제어 목록 우선순위
{: #precedence}

`acl allow :` 명령을 제공한 후 추가적인 `acl allow` 명령을 입력하는 경우 사용자가 더 이상 바인딩 해제된 액세스를 허용하지 않으려는 것으로 간주하여 `ALL:ALL` 허용 규칙(`acl allow :`)이 목록에서 제거됩니다. `acl deny :` 명령을 제공한 후 다른 `acl deny` 명령을 입력하는 경우 사용자가 더 이상 모든 액세스를 제한하지 않으려는 것으로 간주하여 `ALL:ALL` 거부 규칙(`acl deny :`)이 목록에서 제거됩니다. 현재 ACL 규칙을 CLI에 `show acl` 명령을 통해 나열하면 표시기가 나열되지 않은 규칙이 허용되는지 또는 거부되는지를 표시합니다.

## ACL 파일 가져오기
{: #import}

스타트업 시 클라이언트에서 읽어들일 지원되는 ACL 명령이 포함된 `acl file` 명령에 파일 이름을 제공할 수 있습니다. 이 파일에는 다음과 같은 형식의 명령이 포함되어야 합니다.

```
acl allow [<hostname>]:[<port>]
acl deny [<hostname>]:[<port>]
no acl [<hostname>]:[<port>]
no acl
```
{: screen}

<b>참고:</b> 다른 매개변수 없이 `no acl`을 사용하는 경우 ACL 테이블이 재설정되고 액세스 권한이 DENY ALL로 설정됩니다.

[여기](./securegateway_acl-file.html)에서 샘플 ACL 파일을 찾을 수 있습니다.

[시작하기 - 클라이언트 추가](./securegateway_client.html)로 돌아가십시오.

## ACL 파일을 {{site.data.keyword.SecureGateway}} Docker 클라이언트에 복사
{: #docker}

{{site.data.keyword.SecureGateway}} Docker 클라이언트는 기본적으로 자체 가상화 컨테이너 내에서 실행됩니다. 따라서 호스팅 시스템의 파일 시스템은 {{site.data.keyword.SecureGateway}} 클라이언트를 포함하여 컨테이너 내에서 실행되는 프로세스에 직접 액세스할 수 없습니다. Docker 엔진 버전 1.8.0부터는 실행 중이거나 중지된 동안 'docker cp' 명령을 사용하여 호스트에 있는 파일을 컨테이너에 푸시할 수 있습니다. {{site.data.keyword.SecureGateway}} 클라이언트의 ACL FILE 대화식 명령을 사용하려면 이 작업을 수행해야 합니다.

호스트에서 Docker 인스턴스에 대한 Docker의 대화식 'cp' 지원을 사용하려면 Docker 1.8.0을 사용해야 합니다. `docker --version`을 사용하여 이 내용을 확인할 수 있습니다.

이 작업을 완료하면 다음과 같이 버전이 표시됩니다. Docker가 루트가 아닌 사용자로 실행되도록 허용하는 것이 좋습니다. 따라서 엔진을 1.8.0 또는 1.8.2로 업그레이드한 후에 제안된 명령을 실행하십시오.

```
Client:
 Version:      1.8.2
 API version:  1.20
 Go version:   go1.4.2
 Git commit:   0a8c2e3
 Built:        Thu Sep 10 19:21:21 UTC 2015
 OS/Arch:      linux/amd64

Server:
 Version:      1.8.2
 API version:  1.20
 Go version:   go1.4.2
 Git commit:   0a8c2e3
 Built:        Thu Sep 10 19:21:21 UTC 2015
 OS/Arch:      linux/amd64
```
{: screen}

그런 다음 acl 파일 목록을 Docker 이미지로 푸시하기 위해 다음 단계를 수행하십시오.

- 'docker ps' 명령을 실행하여 컨테이너 ID를 찾으십시오.

```
CONTAINER ID IMAGE                        COMMAND                CREATED        STATUS  PORTS NAMES
764aadce386b ibmcom/secure-gateway-client "node lib/secgwclient" 27 seconds ago Up 26 seconds condescending_nobel
```
{: screen}

- 컨테이너 ID 또는 이름과 함께 'docker cp' 명령을 사용하여 acl.list 목록을 복사하십시오.

```
docker cp 01_client.list 764aadce386b:/root/01_client.list
```
{: pre}

- 그런 다음 Docker에서 실행 중인 {{site.data.keyword.SecureGateway}} 클라이언트에서 다음 작업을 수행하십시오.

```
cli F /root/01_client.list

 [2015-10-01 08:12:30.091] [INFO] The current access control list is being reset and replaced by the user provided file: /root/01_client.list
 [2015-10-01 08:12:30.093] [INFO] The ACL file process accepts acl allow 127.0.0.1:27017
 [2015-10-01 08:12:30.094] [INFO] The ACL file process accepts acl allow 127.0.0.1:22
```
{: screen}

- ACL을 표시하십시오.

```
cli> S
 -------------------------------------------------------------------
           -- Secure Gateway Client Access Control List --

  hostname                               port                  value
  127.0.0.1                             27017                  Allow
  127.0.0.1                                22                  Allow
 -------------------------------------------------------------------
```
{: screen}

[시작하기 - 클라이언트 추가](./securegateway_client.html)로 돌아가십시오.
