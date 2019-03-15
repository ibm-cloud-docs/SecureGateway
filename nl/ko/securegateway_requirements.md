---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-07"

---

# 클라이언트를 실행하기 위한 요구사항
{: #client-requirements}

## 시스템 요구사항
{: #system-requirements}

Secure Gateway 클라이언트는 다음과 같은 환경에서 지원됩니다.

|이름 | 버전          |
| ------------- | ----------- |
| Windows Desktop | 7, 8.1, 10 이상 |
| Windows Server | 2012 R2 이상 |
| Red Hat Linux | 7.3 이상 |
| CentOS | 7.3 이상 |
| SuSE Linux | 12 이상 |
| Ubuntu Linux | 14.04(LTS) 이상 |
| Power Machine | ppc64el 아키텍처 |
| Ubuntu Z-Linux | - |
|AIX | 7.1 TL4 이상 |
|Mac OS X | 10.10(Yosemite) 이상 |
|Docker | 1.7.0 이상, 지원되는 모든 운영 체제 |

<b>참고:</b> 기본 클라이언트 설치의 경우 현재는 64비트 환경만 지원됩니다.

## 네트워크 요구사항
{: #network-requirements}

Secure Gateway 클라이언트는 포트 443 및 포트 9000을 사용하여 npm 레지스트리 및 {{site.data.keyword.Bluemix}} 환경에 연결합니다.
- npm 설치를 위한 `443` 포트
  - 설치 중에 설치 프로그램은 npm 레지스트리에 연결한 후 `npm install`을 실행하여 Secure Gateway 클라이언트에 필요한 종속 항목을 설치합니다. 설치 전에 클라이언트를 설치할 시스템에서 npm 레지스트리 웹 사이트에 연결할 수 있는지 확인하십시오. npm은 기본적으로 npm, Inc.의 공용 레지스트리(https://registry.npmjs.org)를 사용하도록 구성되어 있습니다. <br><br>
사용자 환경에 npm Enterprise 서버가 존재하는 경우 npm Enterprise 서버에서 Secure Gateway 클라이언트의 모든 종속 항목을 화이트리스트에 지정하십시오. 종속 항목의 목록은 `<Installation_directory>\ibm\securegateway\client\package.json` 파일을 참조하십시오.<br><br>

- 게이트웨이 인증을 위한 `443` 포트
  - SG 클라이언트 `v180fp9 and former`의 경우


  |리젼  | 호스트  |
  | --  | --  |
  | 미국 남부  | sgmanager.ng.bluemix.net  |
  | 미국 동부  | sgmanager.us-east.bluemix.net  |
  |영국  | sgmanager.eu-gb.bluemix.net  |
  |Germany  | sgmanager.eu-de.bluemix.net  |
  | 시드니  | sgmanager.au-syd.bluemix.net  |

  - SG 클라이언트 `v181 and later`의 경우
  
  
  |리젼  | 호스트  |
  | --  | --  |
  | 미국 남부  | sgmanager.us-south.securegateway.cloud.ibm.com  |
  | 미국 동부  | sgmanager.us-east.securegateway.cloud.ibm.com  |
  |영국  | sgmanager.eu-gb.securegateway.cloud.ibm.com  |
  |Germany  | sgmanager.eu-de.securegateway.cloud.ibm.com  |
  | 시드니  | sgmanager.au-syd.securegateway.cloud.ibm.com  |

- `9000` 포트

  게이트웨이 구성에 있는 SG 게이트웨이 노드입니다. 각각의 게이트웨이가 동일한 노드에 존재하지 않으므로 게이트웨이를 작성할 때마다 노드의 호스트 이름을 확인하십시오.


적용되는 추가 방화벽 및 IP 테이블 규칙을 검사하거나 수정하는지 확인하십시오. 그러나 IP를 통한 규칙 설정은 권장되지 않습니다. 게이트웨이 인증 및 SG 게이트웨이에 대한 IP가 Bluemix로 제어되고 변경될 수 있으므로 규칙은 호스트 이름 및 포트에 특정하도록 설정되어야 합니다. 네트워크 관리자가 특정 호스트 이름에 대한 현재 IP를 요구하는 경우 [지원 센터에 문의](/docs/services/SecureGateway/securegateway_troubleshooting.html#getting-help-and-support)하여 사용자 환경에 해당하는 IP를 요청하십시오.


## 하드웨어 요구사항 판별
{: #hardware-requirements}

Secure Gateway 클라이언트를 실행하는 시스템의 스펙은 각각의 연결을 통해 전달되는 트래픽에 따라 크게 영향을 받습니다.  각 클라이언트 인스턴스는 최대 250개의 동시 연결을 제공하는 개별 프로세스입니다.  시스템에서 지원해야 하는 평균 최대값을 측정하려면 클라이언트의 평균 트랜잭션 크기(요청 및 응답 모두)를 판별한 후 250개의 동시 트랜잭션으로 스케일링하십시오. 이 숫자에 요청 및 응답 크기가 조합되어 있는 것으로 가정할 때 클라이언트는 이 메모리 풋프린트를 초과할 수 없습니다.  더 정확하게 측정하려면 실제 트랜잭션 시나리오를 더 정확하게 시뮬레이션하기 위해 요청 크기와 응답 크기를 혼합하여 사용해야 합니다.

Secure Gateway 클라이언트의 단일 인스턴스를 실행하는 경우 최소 2개의 코어 및 4GB의 메모리를 권장합니다.

동일한 시스템에서 클라이언트의 다중 인스턴스를 실행하는 HA 시나리오의 경우 최소 4개의 코어 및 8GB의 메모리를 권장합니다.
