---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-29"

---

# 자주 묻는 질문(FAQ)
{: #sg-faq}

## OSI 모델 계층
{: #faq-osi}

### 질문
{: #osi-question}
Secure Gateway 서비스가 표시되는 OSI 모델의 계층은 무엇입니까?

### 답변
{: #osi-answer}
Secure Gateway 서비스는 OSI 모델의 계층 4에 표시됩니다.

## TLS 버전 지원
{: #faq-tls}

### 질문
{: #tls-question}
Secure Gateway 서비스에서 지원하는 TLS 버전은 무엇입니까?

### 답변
{: #tls-answer}
Secure Gateway 서비스에서는 TLS 버전 1.2를 지원합니다.

## 내 게이트웨이 또는 대상을 사용 안함으로 설정하는 이유가 무엇입니까?
{: #faq-disabled}

### 질문
{: #disabled-question}

게이트웨이 또는 대상을 사용 안함으로 설정하는 이유가 무엇입니까?

### 답변
{: #disabled-answer}
다음 이유 중 하나 때문에 대상 또는 게이트웨이를 사용 안함으로 설정할 수 있습니다.

- 대상을 작성하지만 보안을 설정하지 않았습니다.  이 경우 보안을 설정할 때까지 대상을 사용 안함으로 설정할 수 있습니다.
- 서비스에 업데이트를 작성 중이어서 사용자가 서비스를 사용하는 것을 원하지 않습니다.  이 경우 임시로 필요한 게이트웨이를 사용 안함으로 설정하고 서비스가 업데이트될 때까지 대기할 수 있습니다.
- 모든 게이트웨이 및 대상이 프론트 엔드에 설정되도록 했지만 여전히 백엔드에서 빌드 중입니다.  이 경우 백엔드 빌드가 완료될 때까지 게이트웨이나 대상을 사용 안함으로 설정할 수 있습니다.

게이트웨이 또는 대상을 사용 안함으로 설정하는 방법에 대한 자세한 정보는 [Secure Gateway 서비스 인스턴스를 관리하는 방법](/docs/services/SecureGateway?topic=securegateway-manage-sg-service)을 참조하십시오.

## 복수의 영역에서 작성을 자동화하기 위해 권장하는 접근법은 무엇입니까?
{: #faq-automation-spaces}

### 질문
{: #automation-spaces-question}
고객 환경에 하나의 조직 및 세 개의 영역이 있습니다.  영역 중 하나는 개발용이고, 다른 하나는 스테이징용이며, 마지막 하나는 프로덕션용입니다.  고객이 하나의 Secure Gateway 인스턴스를 작성해야 합니까 아니면 복수의 인스턴스(예: 각각의 영역당 하나씩)를 작성해야 합니까?  고객이 복수의 게이트웨이를 작성할 수 있는 경우 각각의 영역에서 게이트웨이 및 대상을 작성하는 Node.js 애플리케이션을 재사용하기 위한 고려사항이 존재합니까?

### 답변
{: #automation-spaces-answer}

- 세 개의 영역 모두에 대해 하나의 Secure Gateway 인스턴스를 작성할 수 있습니다.  하지만 [특정 플랜에 대한 게이트웨이 및 대상 제한사항](/docs/services/SecureGateway?topic=securegateway-secure-gateway-service-plans)을 기억해야 합니다.
- Secure Gateway의 경우 서비스 바인딩이 필요하지 않기 때문에 Node.js 애플리케이션을 재사용하기 위한 추가적인 고려사항은 존재하지 않습니다.


## 복수의 조직에서 작성을 자동화하기 위해 권장하는 접근법은 무엇입니까?
{: #faq-automation-orgs}

### 질문
{: #automation-orgs-question}
고객 환경에 세 개의 영역이 있습니다. 하나는 개발용이고, 하나는 스테이징용이고, 하나는 프로덕션용입니다.  각각의 조직에 대해 Secure Gateway 서비스 인스턴스가 필요합니까? 그리고 해당 조직 내의 모든 영역에서 해당 구성을 사용할 수 있습니까?

### 답변
{: #automation-orgs-answer}

- 각각의 조직에 Secure Gateway 서비스 인스턴스를 포함시킬 필요는 없습니다. 하나의 조직에 인스턴스가 포함되어 있는 경우 다른 모든 환경에서 해당 인스턴스 내에 있는 게이트웨이를 사용할 수 있습니다.  이 설정을 사용하는 경우 [특정 플랜에 대한 게이트웨이 및 대상 제한사항](/docs/services/SecureGateway?topic=securegateway-secure-gateway-service-plans)을 기억해야 합니다.
- 각각의 조직에 Secure Gateway 서비스 인스턴스를 포함시킬 수 있으며 모든 영역에서 해당 구성을 사용할 수 있습니다.

## 내 앱이 동일한 영역에 있어야 합니까?
{: #faq-app-space}

### 질문
{: #app-space-question}
Node.js 앱을 Secure Gateway 서비스와 동일한 {{site.data.keyword.Bluemix_notm}} 영역에서 실행해야 합니까?

### 답변
{: #app-space-answer}
아니오, Secure Gateway 서비스와 동일한 {{site.data.keyword.Bluemix_notm}} 영역에서 앱을 실행할 필요는 없습니다.

## Secure Gateway 서버 로그를 가져올 수 있습니까?
{: #faq-server-logs}

### 질문
{: #server-logs-question}
Secure Gateway 서버의 오류 레벨 로그를 검색할 수 있습니까?

### 답변
{: #server-logs-answer}
서버의 오류 레벨 로그는 검색할 수 없습니다.  요청 시 발생한 오류만 확인할 수 있습니다.

## Secure Gateway의 작동 상태에는 무엇이 있습니까?
{: #faq-states}

### 질문
{: #states-question}
게이트웨이 및 대상의 라이프사이클 상태에는 무엇이 있습니까?

### 답변
{: #states-answer}

#### 비작동 상태
{: #states-answer-non-functional}
1.7.0 릴리스에서는 새로운 계층 지정 플랜 가격 모델이 도입되었습니다. 이 모델을 사용함으로써 게이트웨이 및 대상을 모두 '활성' 또는 '비활성'으로 표시하는 기능이 도입되었습니다. 새로운 플랜 가격 구조 중 일부는 사용자에게 해당 사용자가 보유한 게이트웨이 및 대상 수에 대한 비용을 청구합니다.

- 비활성 항목은 청구되지 않음
- 비활성 대상에는 프로비저닝된 클라우드 포트가 없습니다.
- 비활성 게이트웨이 내의 대상도 모두 비활성되어 해당 게이트웨이가 활성으로 설정되기 전에는 다시 활성화할 수 없습니다.
- 비활성 항목은 비작동 상태로 간주됩니다. 비활성 항목은 어떠한 작동 상태에도 포함될 수 없습니다.

#### 작동 상태
{: #states-answer-functional}
게이트웨이 또는 대상이 활성으로 표시되면 비용이 청구됩니다. 게이트웨이 및 대상의 활성 상태는 다음과 같습니다.

- 사용 - 정상적인 환경에서 게이트웨이 또는 대상을 사용할 준비가 되었습니다.
- 사용 안함 - 게이트웨이 또는 대상이 사용 안함으로 설정되었습니다.
    - 게이트웨이 - 클라이언트에서 사용 안함으로 설정된 게이트웨이로 데이터를 플로우할 수 없습니다.
    - 대상 - 클라이언트에서 이러한 사용 안함으로 설정된 대상으로의 연결을 작성할 수 없습니다.

## Secure Gateway 클라이언트의 데이터 활동을 파악하는 방법은 무엇입니까?
{: #faq-data-size}

### 질문
{: #data-size-question}
Secure Gateway 클라이언트를 통한 데이터 활동을 파악하는 방법은 무엇입니까?

### 답변
{: #data-size-answer}
SecureGateway 클라이언트에서 로그 레벨을 TRACE로 변경하십시오. 요청이 전송되면 다음과 같은 정보가 표시됩니다.

요청 애플리케이션에서 전송된 데이터 크기:
```
[TRACE] Connection #<connection ID> transmitted data: <size> bytes
```

대상에서 전송된 데이터 크기:
```
[TRACE] Connection #<connection ID> received data: <size> bytes
```

## Secure Gateway의 실시간 연결 수를 가져오는 방법은 무엇입니까?
{: #faq-connect-num}

### 질문
{: #connect-num-question}
Secure Gateway 클라이언트에서 연결 정보(실시간 연결 수, 전송된 데이터 크기 및 수신된 데이터 크기)를 가져오는 방법은 무엇입니까?

### 답변
{: #connect-num-answer}

- Secure Gateway 클라이언트 대화식 명령행에서
`s`를 입력하여 연결 상태 세부사항을 인쇄하십시오. 
- Secure Gateway 클라이언트 UI에서 연결 정보 메뉴를 클릭하십시오.

다음과 같은 연결 통계가 표시됩니다. 
- 전송된 전체 데이터 크기
- 수신된 전체 데이터 크기
- 총 연결 수
- 실시간 활성 연결 수

참고: 이 연결 정보의 레벨은 게이트웨이 레벨이 아닌 클라이언트 레벨입니다. 게이트웨이 레벨의 연결 정보가 필요한 경우 해당 게이트웨이에 연결된 각 클라이언트를 확인하십시오.

## 더욱 안전한 연결을 위해 권장하는 구성은 무엇입니까?
{: #faq-secure-app}

### 질문
{: #secure-app-question}
더욱 안전한 연결을 위해 권장하는 구성은 무엇입니까?

### 답변
{: #secure-app-answer}

#### 상호 인증 사용
{: #secure-app-answer-ma}
온프레미스 대상의 양쪽 측면에 대한 상호 인증을 사용으로 설정하여 Secure Gateway를 더욱 안전하게 만들 수 있습니다. 사용자 인증 측에서는 상호 인증을 사용으로 설정하여 TLS/HTTPS를 통해 요청이 수행될 때 클라이언트 인증서를 사용하여 인증함으로써 Secure Gateway 클라우드 노드에 대한 액세스를 제한하십시오. 리소스 인증 측에서는 상호 인증을 사용으로 설정하여 대상 엔드포인트에 연결할 때 적절한 인증 정보를 제공하도록 함으로써 온프레미스 리소스에 대한 보안/암호화 액세스를 보장하십시오. 자세한 정보는 [상호 인증 구성](/docs/services/SecureGateway?topic=securegateway-add-dest#dest-mutual-auth) 및 [Node.js TLS 상호 인증](/docs/services/SecureGateway?topic=securegateway-nodejs-tls-ma#nodejs-tls-ma)을 참조하십시오.

#### IP 테이블 규칙 설정(온프레미스 대상의 경우)
{: #secure-app-answer-iptables}
온프레미스 대상의 Secure Gateway 클라우드 호스트 및 포트는 공용 영역에 존재하기 때문에 기본적으로 모든 사용자가 액세스할 수 있도록 허용됩니다.
온프레미스 리소스에 보안을 적용하기 위해 Secure Gateway에 대한 트래픽 액세스를 제어하려면 특정 범위의 IP 및 포트에서만 액세스할 수 있도록 허용하는 iptable 규칙을 설정하십시오. Secure Gateway에서 iptable 규칙을 구성하는 방법에 대한 자세한 정보는 [IP 테이블 규칙](/docs/services/SecureGateway?topic=securegateway-add-dest#dest-network-security)을 참조하십시오.

#### 액세스 제어 목록 구성(온프레미스 대상의 경우)
{: #secure-app-answer-acl}
온프레미스 리소스에 대한 액세스를 허용하거나 제한하는 액세스 제어 목록 지원을 구성하여 특정 대상 호스트 및 포트에 대한 액세스 권한을 지정함으로써 온프레미스 대상을 더욱 안전하게 만들 수 있습니다. 온프레미스 대상의 보안을 강화하기 위해 ACL 항목에 대해 허용되거나 제한된 HTTP/S 라우트도 정의하는 것이 좋습니다. 자세한 정보는 [액세스 제어 목록](/docs/services/SecureGateway?topic=securegateway-acl#acl) 및 [ACL을 사용한 HTTP/S 라우트 제어](/docs/services/SecureGateway?topic=securegateway-acl#acl-route-control)를 참조하십시오.

#### Secure Gateway 클라이언트 UI에 비밀번호 설정
{: #secure-app-answer-ui-pw}
Secure Gateway 클라이언트 UI에 대한 액세스를 제한하기 위해 UI 비밀번호를 설정하는 것이 좋습니다. Secure Gateway 터미널 명령행에서 스타트업 구성 또는 대화식 명령을 사용하여 비밀번호를 설정하는 방법에 대한 자세한 정보는 [클라이언트와 상호작용](/docs/services/SecureGateway?topic=securegateway-client-interacting#client-interacting)을 참조하십시오.

## 게이트웨이 마이그레이션은 무엇입니까? 2018년 12월 후 도메인이 변경된 이유는 무엇입니까?
{: #faq-gateway-migration}

### 질문
{: #gateway-migration-question}
2018년 12월의 유지보수 후, 게이트웨이 패널에 마이그레이션 단추가 생성되었습니다. 이 단추의 용도는 무엇입니까? 도메인이 변경된 이유는 무엇입니까?

### 답변
{: #gateway-migration-answer}

2018년 12월의 유지보수 후 `integration.ibmcloud.com` 대신 `securegateway.appdomain.cloud`를 사용하고, `bluemix.net` 대신 게이트웨이 인증을 위해 `securegateway.cloud.ibm.com`을 사용하도록 Secure Gateway의 클라우드 호스트의 이름이 변경되었습니다. 역호환성을 위해 기존 게이트웨이는 게이트웨이가 마이그레이션될 때까지 이전 도메인을 계속해서 사용하고 SG 클라이언트 v180fp9 이하에서는 게이트웨이 인증을 위해 `bluemix.net`을 계속해서 사용합니다.

마이그레이션 후 온프레미스 대상의 클라우드 호스트는 새 도메인을 사용하도록 변경하고 사용자/애플리케이션은 요청을 새 클라우드 호스트로 전송하도록 업데이트해야 합니다.

현재 마이그레이션은 필수가 아니며 이전 도메인의 지원이 중단될 정확한 날짜는 지정되지 않았습니다. 날짜가 지정되면 이전 도메인을 계속해서 사용하는 고객에게 해당 날짜를 알려드릴 것입니다.

## 알림을 어디서 받을 수 있습니까?
{: #faq-notification}

### 질문
{: #notification-question}
Secure Gateway 알림을 어디서 받을 수 있습니까? 특히, 유지보수 중단에 대한 알림을 어디서 받을 수 있습니까?

### 답변
{: #notification-answer}

[상태 페이지](https://cloud.ibm.com/status?selected=status)를 통해 알림을 받을 수 있습니다.
- 완료된/진행 중인 중단 유지보수에 대한 알림을 받으려면 `상태` 탭에서 `Secure Gateway`를 검색하십시오.
- 계획된 중단 유지보수에 대한 알림을 받으려면 `계획된 유지보수` 탭에서 `Secure Gateway`를 검색하십시오.

예기치 않게 Secure Gateway 클라이언트의 연결이 끊어진 경우 상태 페이지로 이동하여 해당 시간에 유지보수가 중단되었는지 확인하십시오.

유지보수로 인해 10분 이상 중단되어야 하는 경우 유지보수 이후 Secure Gateway 서버에 다시 연결하기 위해 Secure Gateway 클라이언트를 수동으로 다시 시작해야 할 수 있습니다. 일반적으로 서비스 중단 시간은 10분 이하이므로 Secure Gateway 클라이언트(버전 v180 이후)가 Secure Gateway 서버에 자동으로 다시 연결할 수 있어야 합니다.

## DataPower에서 Secure Gateway 클라이언트 로그를 캡처할 수 있는 방법은 무엇입니까?
{: #faq-dp-log}

### 질문
{: #dp-log-question}
Secure Gateway 클라이언트 로그를 캡처하여 DataPower의 파일에 쓸 수 있는 방법은 무엇입니까?

### 답변
{: #dp-log-answer}

Secure Gateway 클라이언트 로그의 이벤트 카테고리는 `sgclient`입니다. 특정 이벤트 카테고리가 포함된 로그를 파일에 쓰기 위한 [로그 대상](https://www.ibm.com/support/knowledgecenter/en/SS9H2Y_7.7.0/com.ibm.dp.doc/logtarget_logs.html)을 작성할 수 있습니다. 예제는 다음과 같습니다.

- 기본 도메인에서:
    - GUI 사이드 패널에서 `오브젝트` → `로깅 구성` → `로그 대상`을 선택하십시오. 또는 `검색` 필드에서 `로그 대상`을 검색하십시오.
    - `추가` 단추를 선택하여 로그 대상을 추가하십시오.
- `기본` 탭에서:
    - `이름`을 입력하십시오.
    - `대상 유형`을 `파일`로
    - `로그 형식`을 `텍스트`로
    - 출력 위치를 정의하기 위한 `파일 이름`을 입력하십시오(예: `logtemp:///sgclient.log`).
    - `아카이브 모드`를 `순환`으로 선택하십시오.
- `이벤트 구독` 탭에서:
    - `이름`을 입력하십시오.
    - `추가` 단추를 선택하여 대상 이벤트 구독을 추가하십시오.
    - `sgclient`를 선택하여 `이벤트 카테고리`를 채우십시오.
    - `최소 이벤트 우선순위`를 `debug`로 채우십시오.
