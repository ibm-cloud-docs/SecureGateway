---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-04"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 클라이언트와 상호작용

클라이언트와 상호작용하는 몇 가지 방법이 존재합니다.

- 스타트업 전 터미널 명령행을 통해
- 스타트업 후 클라이언트의 대화식 명령행 사용
- 스타트업 후 클라이언트의 로컬 UI 사용

## 스타트업 구성
{: #startup}

### 스타트업 인수 및 옵션
{: #startup-args}

다음 표에서는 클라이언트의 스타트업 명령과 함께 제공할 수 있는 모든 옵션에 대해 설명합니다. 이러한 옵션을 사용하여 클라이언트가 실행된 후 개별 설정을 요구하지 않고 스타트업 전에 클라이언트의 구성을 완료할 수 있습니다.

| 매개변수 및 인수 |설명 |
| ------------- | ----------- |
| &lt;gateway ID&gt; |제공되는 게이트웨이 ID를 사용하여 {{site.data.keyword.Bluemix_notm}}에 연결 |
| -F, -\-aclfile &lt;file&gt; | Access control List file |
| -g, -\-gateway &lt;hostname:port&gt; | Used to manually select a specific gateway destination (advanced use only) |
| -l, -\-loglevel &lt;level&gt; | Change the log level to ERROR, INFO, DEBUG or TRACE |
| -p, -\-logpath &lt;file&gt; | Direct logging to a specific file |
| -t, -\-sectoken &lt;security token&gt; | The security token to use for this gateway connection |
| -P, -\-port &lt;port&gt; | The port for the UI to run on.  Defaults to port 9003 |
| -w, -\-password &lt;password&gt; | The password to protect the UI with.  Defaults to no password |
| -x, -\-proxy &lt;proxy agent&gt; | The proxy for the port 9000 connection |
| -\-noUI | Prevent the UI from starting up automatically |
| -\-allow | Allows all connections to the client. Is overridden by the ACL file, if provided |
| -\-service | After an initial connection, the parent will restart within 60s if all child clients are terminated |

<b>참고:</b> `--service`, `--allow` 및 `--noUI` 플래그는 명령행 인수의 마지막 매개변수여야 합니다.

가장 기본적인 유스 케이스는 기본 설정을 사용하여 단일 클라이언트 연결을 시작하는 것입니다.

```
<gateway_id> -t <security_token>
```
{: pre}

이러한 옵션은 복수의 클라이언트에 대한 자동 연결을 지원하기 위해 확장할 수도 있습니다. 이러한 옵션을 다중 클라이언트 연결에 전달하기 위해서는 특정 형식을 사용하여 전달해야 합니다. 게이트웨이 ID는 단일 클라이언트 연결과 동일한 방식(각각의 게이트웨이 ID를 공백으로 구분함)으로 전달할 수 있지만 해당 ID의 순서에 따라 나머지 인수가 오는 순서가 결정됩니다. 다른 인수를 전달하는 경우 올바르게 선택하기 위해 `--`로 구분해야 합니다. 특정 플래그에 대해 전달되는 항목이 없을 경우 어떤 클라이언트에도 적용되지 않는 것으로 간주합니다.

모든 게이트웨이 ID를 채우기에 충분한 인수가 존재하지 않을 경우 모든 인수가 사용될 때까지 순서대로 적용됩니다. 예를 들어 두 개의 게이트웨이 ID가 전달되고 하나의 보안 토큰이 전달되는 경우 첫 번째 게이트웨이 ID에 토큰이 적용되고 두 번째 게이트웨이 ID에는 적용되지 않습니다.  

다른 인수가 필요한 게이트웨이 ID가 제공되는 경우 게이트웨이에서 적용/제공하지 않는 특정 인수 대신 키워드 `none`을 지정해야 합니다. 예를 들어 세 개의 게이트웨이 ID가 전달되고 사용자가 첫 번째 및 세 번째 게이트웨이 ID에 대한 로그 레벨을 지정하려는 경우 해당 인수는 `--loglevel DEBUG--none--TRACE`와 유사합니다. 이 경우 none은 기본값인 INFO로 설정됩니다.

### 스타트업 인수 예제
{: #startup-examples}

단일 클라이언트 연결, 사용자 정의 로그 레벨, UI 없음:

```
node lib/secgwclient.js <gateway_id> -t <security_token> -l <loglevel> --noUI
```
{: pre}

다중 클라이언트 연결, 기본 옵션:

```
node lib/secgwclient.js <gateway_id_1> <gateway_id_2> -t <security_token_1>--<security_token_2>
```
{: pre}

다중 클라이언트 연결, 모든 사용자 정의 설정:

```
node lib/secgwclient.js <myGatewayID_1> <myGatewayID_2> -t none--<token for gateway 2> -l DEBUG--TRACE -p <full path to log file for gateway 1>--<full path to log file for gateway 2> -F <full path to ACL file for gateway 1>
```
{: pre}

[시작하기 - 클라이언트 추가](/docs/services/SecureGateway/securegateway_client.html)로 돌아가십시오.

## 대화식 구성
{: #interactive}

### 대화식 명령
{: #interactive-commands}

{{site.data.keyword.SecureGateway}} 클라이언트에는 명령행 인터페이스(CLI)와 손쉬운 구성 및 제어를 위한
쉘 프롬프트가 있습니다. 대화식 환경에서는 명령행 인수에 비해 더 풍부한 기능 세트를 지원하여 더 효율적인 클라이언트에 대한 대화식 제어를 가능하게 해줍니다.

| 대화식 명령 |설명       |
| ------------- | ----------- |
| A, acl allow &lt;hostname:port&gt; &lt;worker ID&gt; |액세스 제어 목록 허용 |
| D, acl deny &lt;hostname:port&gt; &lt;worker ID&gt; |액세스 제어 목록 거부 |
| N, no acl &lt;hostname:port&gt; &lt;worker ID&gt; |액세스 제어 목록 항목 제거 |
| S, show acl &lt;worker ID&gt; |액세스 제어 목록 항목 모두 표시 |
| F, acl file &lt;file&gt; &lt;worker ID&gt; | 액세스 제어 목록 파일 |
| C, displayconfig &lt;worker ID&gt; |현재 {{site.data.keyword.SecureGateway}} 구성
표시(사용 가능한 경우) |
| a, authorize &lt;worker ID&gt; | 지정된 작업자의 아웃바운드 TLS 연결을 위한 rejectUnauthorized 매개변수의 대체를 전환 |
| t, sectoken &lt;security token&gt; | 다음 게이트웨이 연결에 사용할 보안 토큰 |
| c, connect &lt;gateway ID&gt; |제공되는 게이트웨이 ID를 사용하여 {{site.data.keyword.Bluemix_notm}}에 연결 |
| l, loglevel &lt;level&gt; &lt;worker ID&gt; | 로그 레벨을 ERROR, INFO, DEBUG 또는 TRACE로 변경 |
| p, logpath &lt;file&gt; &lt;worker ID&gt; |특정 파일로 직접 로깅 |
| r, reverse &lt;worker ID&gt; | 클라이언트에서 현재 역방향 대상에 대해 청취 중인 포트 나열 |
| k, kill &lt;worker ID&gt;  | 지정된 작업자 종료 |
| e, select &lt;worker ID&gt;  | 달리 지정되지 않는 한 명령을 수행할 작업자 지정 |
| d, deselect | 이전에 지정된 작업자를 선택 취소합니다. 다른 연결을 지정하려면 select 명령을
실행하십시오. |
| w, password &lt;old password&gt; &lt;new password&gt; | UI 비밀번호를 설정합니다. &lt;new password&gt;가 비어 있는 경우 비밀번호가 적용되지 않습니다. 비밀번호를 업데이트하는 경우 &lt;old password&gt;가 필요합니다. 비밀번호는 문자만 포함해야 합니다. |
| P, port &lt;new port&gt; |UI가 청취 중인 포트 변경 |
| u, uistart &lt;initial password&gt; &lt;port&gt; | localhost:&lt;port&gt;/dashboard에서 UI를 시작합니다. &lt;initial password&gt;가 공백이고 해당 세션에 다른 비밀번호가 설정되어 있지 않은 경우 UI 비밀번호가 적용되지 않습니다. &lt;port&gt;가 공백이면 UI를 포트 9003에서 연결할 수 있습니다. |
| U, uistop |이 클라이언트 세션과 연관된 UI를 닫습니다. 수동으로 새 UI를 시작할 때까지 CLI를 통해서만 세션에 액세스할 수 있습니다. |
| R, revoke |이 클라이언트 세션과 연관된 모든 UI 권한을 지웁니다. |
| q, quit |연결 끊기 및 종료 |
| s, status &lt;worker ID&gt;  |터널 및 해당 연결의 상태 세부사항을 인쇄 |
| L, list | 현재 연관되어 있는 작업자의 목록을 표시합니다. |

<b>참고:</b> `select` 명령을 사용하여 연결이 지정된 상태에서 작업자 ID를 제공하지 않고 다른 명령을 호출하는 경우 `select`로 지정된 연결에서 해당 명령을 실행하려고 시도합니다.

액세스 제어 목록을 구성하는 방법에 대한 자세한 정보를 확인하려면 [여기를 클릭](/docs/services/SecureGateway/securegateway_acl.html)하십시오.

[시작하기 - 클라이언트 추가](/docs/services/SecureGateway/securegateway_client.html)로 돌아가십시오.

## 클라이언트 UI
{: #ui}

<b>참고:</b> Windows 또는 MacOS에서 Docker를 사용하는 경우 클라이언트 UI가 지원되지 않습니다.

클라이언트 UI는 사용자가 CLI가 아닌 {{site.data.keyword.SecureGateway}} 클라이언트와 상호작용할 수 있는 로컬 웹 인터페이스를 제공합니다. 기본적으로 이 UI는 `localhost:9003/dashboard`에서 사용할 수 있습니다. 이 UI는 다음과 같은 페이지로 분할됩니다.

### 연결
{: #ui-connect}

이 페이지는 사용자가 게이트웨이 ID 및 보안 토큰을 제공하여 첫 번째 클라이언트에 연결할 수 있는 UI의 초기 랜딩 페이지입니다.

### 로그인
{: #ui-login}

UI가 비밀번호로 보호된 경우 이 페이지가 표시됩니다. 비밀번호가 적용되지 않은 상태에서 이 페이지에 도달하는 경우 페이지를 새로 고쳐서 경로 재지정하십시오.

### 대시보드
{: #ui-dashboard}

클라이언트에 연결한 적이 있는 경우 이 페이지가 기본 페이지입니다. 여기에서 로그 보기 페이지, 액세스 제어 목록 페이지 및 연결 정보 페이지에 액세스할 수 있습니다. 또한 맨 아래에서 연결된 클라이언트 중 하나 이상의 연결을 끊도록 선택할 수 있습니다. 페이지의 맨 위에는 현재 선택된 클라이언트 및 추가 클라이언트를 연결하기 위한 옵션이 표시됩니다.

### 로그 보기
{: #ui-logs}

이 페이지를 통해 선택된 클라이언트(페이지의 오른쪽 상단에 표시됨)에서 생성하는 로그를 확인할 수 있습니다. 표시된 로그는 로그 아래 선택란으로 필터링될 수 있습니다.

### 액세스 제어 목록
{: #ui-acl}

이 페이지를 통해 선택된 클라이언트(페이지의 오른쪽 상단에 표시됨)에 대한 액세스 제어 목록을 조작할 수 있습니다. 허용/거부 테이블에 개별적으로 규칙을 추가하거나 페이지의 맨 아래에서 파일을 업로드할 수 있습니다.

### 연결 정보
{: #ui-info}

이 페이지에는 선택된 클라이언트(페이지의 오른쪽 상단에 표시됨)에 대한 현재 연결 정보가 표시됩니다. 게이트웨이 설명, 현재 연결 수, 역방향 대상 리스너와 같은 정보를 여기에서 전송할 수 있습니다.

[시작하기 - 클라이언트 추가](/docs/services/SecureGateway/securegateway_client.html)로 돌아가십시오.

## 원격 클라이언트 종료
{: #remote}

클라이언트에서 ID를 제공한 경우 SG UI 또는 SG API를 통해 해당 클라이언트를 원격으로 종료할 수 있습니다. 서비스로 실행 중인 클라이언트를 종료하는 경우 클라이언트가 다시 시작되면서 새 클라이언트 ID를 받지만 서비스에 복수의 클라이언트가 연결되어 있는 경우 나머지 클라이언트가 모두 종료될 때까지 종료된 클라이언트가 다시 시작되지 않습니다.

## 제한사항
{: #limits}

### 연결 제한사항
{: #limits-conn}

SG 게이트웨이는 250개의 동시 연결만 처리할 수 있습니다. 동시 요청 수가 제한을 초과하는 경우 연결 시도가 거부되고 대기 시간이 발생할 수 있습니다. 이 문제를 수정하는 간단한 방법은 앱을 요청할 때 연결 풀링을 사용하는 것입니다. 250개의 동시 연결 제한은 게이트웨이에 해당되는 것이며 클라이언트 또는 대상에는 해당되지 않습니다. 이 제한은 게이트웨이에 있는 모든 클라이언트 및 대상에서 공유됩니다.

### DataPower 클라이언트 제한사항
{: #limits-datapower}

{{site.data.keyword.SecureGateway}} DataPower 클라이언트의 경우 나머지 IBM 클라이언트의 기능과 일치하도록 업데이트하는 중입니다. 현재는 다음과 같은 제한사항이 존재합니다.

- ACL이 기본값인 ALLOW ALL로 설정됩니다.
- {{site.data.keyword.SecureGateway}} UI에서 게이트웨이 또는 대상을 사용 및 사용 안함으로 설정하는 작업이 지원되지 않습니다. 하지만 DataPower UI의 관리 상태 옵션이 해당 특정 클라이언트에 대한 켜기/끄기 스위치로 작동합니다.
- 연결된 게이트웨이 및 연결이 끊긴 게이트웨이 상태의 실시간 업데이트를 위한 연결 상태 폴링이 지원되지 않습니다.
- DataPower 버전 7.5.1.0 이전 버전에서는 대상 측 TLS가 포함된 전체 인증서 체인이 지원되지 않습니다.
- DataPower 버전 7.5.1.0 이전 버전에서는 클라우드 대상이 지원되지 않습니다.
- 로그 레벨을 TRACE 레벨로 변경할 수 없습니다.
