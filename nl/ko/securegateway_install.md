---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-11"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 클라이언트 설치

## Docker
{: #docker}

Docker는 써드파티 플랫폼이며 몇 가지 구성만으로 또는 구성 작업을 수행하지 않고 애플리케이션을 빠르고 쉽게 설치하는 컨테이너 접근 방식을 제공합니다. {{site.data.keyword.SecureGateway}} 서비스는 Docker 유틸리티가 사용자 워크스테이션에 설치된 후에 사용할 Docker 이미지를 제공합니다.  Docker를 설치하려면 Docker 설치 웹 사이트를 참조하여 사용자 시스템에 해당하는 지시사항을 수행하십시오.

### 클라이언트 설치 및 업데이트
{: #docker-install}

{{site.data.keyword.SecureGateway}} 클라이언트 Docker 이미지를 설치 및 업데이트하는 작업은 모두 동일한 명령을 사용합니다.

```
docker pull ibmcom/secure-gateway-client
```
{: pre}

### 대화식 클라이언트 세션 시작
{: #docker-run}

IBM {{site.data.keyword.SecureGateway}}용 Docker 컨테이너를 실행하려면 다음 명령을 실행하십시오.

```
docker run -it ibmcom/secure-gateway-client <gateway ID> -t <security token>
```
{: pre}

이 명령에서는 일반적으로 두 개의 매개변수({{site.data.keyword.SecureGateway}} 게이트웨이 ID 및 게이트웨이의 보안 토큰)를 사용하며, 둘 다 {{site.data.keyword.SecureGateway}} 대시보드를 통해 사용할 수 있습니다.

### 지원되는 Docker 명령
{: #docker-commands}

{{site.data.keyword.SecureGateway}} 클라이언트의 경우 컨테이너를 조작하기 위해 `pull` 및 `run` 명령만 지원합니다.

[시작하기 - 클라이언트 추가](/docs/services/SecureGateway/securegateway_client.html)로 돌아가십시오.

## Mac OS X
{: #mac}

### Mac OS X에서 실행하기 위한 요구사항
{: #mac-requirements}

Mac OS X에서 클라이언트를 실행하려면 먼저 다음과 같은 전제조건이 필요합니다.
 1. Xcode 개발 도구를 설치하십시오. 이 플랫폼의 일부 노드 모듈에서 필요합니다.

### Mac OS X 설치 프로시저
{: #mac-install}

시스템의 보안 설정에 따라 이 설치를 수행하기 위해 관리 권한이 필요할 수도 있습니다.

 1. {{site.data.keyword.SecureGateway}} UI에서 다운로드한 DMG 이미지를 일반적으로 '두 번 클릭'하여 마운트하십시오.
 2. 새 '파인더' 창이 표시됩니다. 이 창에는 애플리케이션 "바로 가기" 아이콘이 포함되어 있어야 합니다. 애플리케이션을 끌어서 바로 가기에 놓으십시오. 그렇지 않을 경우 마운트된 볼륨을 '두 번 클릭'한 후 애플리케이션 아이콘을 끌어서 파인더 사이드바에 있는 애플리케이션 아이콘에 놓으십시오.

### 대화식 클라이언트 세션 시작
{: #mac-run}

클라이언트를 시작하려면 기본 설치 위치(`/Applications/ibm/`)에 있는 `secgw.command` 파일을 실행하십시오.

[시작하기 - 클라이언트 추가](/docs/services/SecureGateway/securegateway_client.html)로 돌아가십시오.

## Linux
{: #linux}

이 설치에는 {{site.data.keyword.SecureGatewayfull}} 클라이언트 및 보안 버전의 IBM nodejs 패키지가 포함되어 있습니다. 둘 다 시스템의 /opt/ibm 디렉토리에 설치됩니다. 설치 프로그램에서는 다음과 같은 항목을 작성하거나 업데이트합니다.

|디렉토리 | 파일 이름 |설명          |
| ------------- | ------------- | ----------- |
| /etc/ibm | sgenvironment.conf | 시작용 구성 파일 |
| /etc/ibm | passwd | 사용자 작성: secgwadmin:x:501:501::/home/secgwadmin:/bin/bash |
| /etc/ibm | group | 그룹 작성: secgwadmin:x:501: |
| /etc/init | securegateway_client.conf | 시작 파일 |
| /opt/ibm | node-v&lt;version&gt;-linux-x64 | IBM Node JS 설치 |
| /opt/ibm | securegateway | {{site.data.keyword.SecureGateway}} 클라이언트 설치 |
| /usr/local/bin | node | IBM 노드 실행 파일용으로 작성된 symlink |
| /usr/local/bin | npm | IBM npm 실행 파일용으로 작성된 symlink |
| /var/log/securegateway | client_console.log | 자동 시작 프로세스로 클라이언트를 실행할 때 작성된 로그 파일 |

이 설치를 수행하려면 루트 또는 관리 권한이 필요합니다.

### Ubuntu/PowerPC 설치
{: #debian-install}

 1. {{site.data.keyword.SecureGateway}} 클라이언트를 설치하십시오. 예를 들어 Debian 패키지를 사용하는 경우 다음 명령을 실행하십시오.

    ```
    sudo dpkg -i <secure-gateway-debian-package>
    ```
    {: pre}

 2. 클라이언트 설치 프로그램이 시작되면 다음과 같은 정보를 요청하는 프롬프트가 표시됩니다.

    <b>자동 시작 프로세스 예/아니오</b>
        업그레이드 또는 재설치의 경우 설치 프로세스가 실행되는 동안 기존 클라이언트를 중지할 것인지 여부를
        선택해야 합니다. 기존 클라이언트를 중지하려면 Y/y를 선택하십시오. 그렇지 않을 경우 클라이언트를
        다시 시작하지 않고 클라이언트 패키지가 업그레이드됩니다. 즉, 적절한 소프트웨어 업데이트 창에서
        다시 시작을 수행할 때까지 대기할 수 있습니다. N/n을 선택하는 경우 설치가 계속되고 수동으로 클라이언트를 다시 시작해야
        합니다.

    <b>게이트웨이 ID</b>
        클라이언트의 게이트웨이 ID를 설정하십시오. 게이트웨이 ID는 {{site.data.keyword.SecureGateway}} 서비스를 작성할 때 정의됩니다. 클라이언트가
        연결에 실패하는 경우 .conf 파일을 편집하여 선택사항을 변경할 수 있습니다.

    <b>보안 토큰</b>
        게이트웨이 ID에서 보안 토큰을 검사하도록 설정한 경우 지금 해당 보안 토큰을 제공하십시오. 게이트웨이 ID에
        보안 토큰이 필요하지 않으면 이를 비워 두십시오.

    <b>로깅 레벨</b>
        기본 설정은 INFO입니다. 다른 유효한 값은 TRACE, DEBUG 및 ERROR입니다.

    <b>액세스 제어 목록</b>
        온프레미스 리소스에 대한 액세스 권한이 있는 액세스 제어 목록이 포함된 파일 이름의 절대 경로를
        입력하십시오. 자세한 정보는 /opt/ibm/securegateway의 README.md 파일 또는 액세스 제어 목록을 참조하십시오.

    <b>클라이언트 UI</b>
        클라이언트 UI를 사용하려면 선택하십시오. 예를 선택하는 경우 사용자가 UI의 포트를 변경할 수 있습니다. 기본값은 9003입니다.

    <b>참고:</b> 프롬프트에 응답할 필요는 없으며, 응답하지 않을 경우 모두 sgenvironment.conf 파일에서 정의된 기본값을 사용하거나 비워 두게 됩니다. 이렇게 하면 사용자 상호작용 없이 설치 프로세스를
실행할 수 있습니다.

    <b>참고:</b> 시스템의 시작 프로세스를 사용하여 클라이언트를 시작하거나 다시 시작할 때마다 sgenvironment.conf를 읽어들입니다. 언제든지 /etc/ibm/sgenvironment.conf 파일을 편집하여 구성을 변경한 후 해당 변경사항이 적용되도록 클라이언트를 다시 시작할 수 있습니다.

    <b>참고:</b> /etc/ibm/sgenvironment.conf 파일의 `LANGUAGE` 매개변수를 변경하여 {{site.data.keyword.SecureGateway}} 클라이언트 서비스 로그의 언어를 변경할 수 있습니다. 서비스를 다시 시작하면 서비스 로그가 선택한 언어로 변경됩니다.

 3. 클라이언트를 다시 시작한 경우 클라이언트가 올바르게 실행 중인지 확인하기 위해 다음 명령을 실행하십시오.

    ```
cat /var/log/securegateway/client_console.log
    ```
    {: pre}

 4. 구성 파일을 편집하려는 경우 구성 파일이 올바르게 업데이트되었는지 확인하기 위해 다음 명령을 실행하십시오.

    ```
sudo vi /etc/ibm/sgenvironment.conf
    ```
    {: pre}

 5. 클라이언트 설치 상태를 확인하려면 다음 명령을 실행하십시오.

    ```
sudo dpkg --status ibm-securegateway-client
    ```
    {: pre}

다음은 리턴되는 출력의 예제입니다.

```
Package: ibm-securegateway-client
Status: install ok installed
Priority: extra
Section: IBM/net
Maintainer: cloudoe-dev@webconf.ibm.com
Architecture: amd64
Source: ibm-securegateway+securegateway-client
Version: 1.7.0
Description: IBM Secure Gateway Client for Bluemix
IBM Secure Gateway client package for Bluemix, supports secure connections to on-premises connections.
Homepage: https://ng.bluemix.net/
License: http://www.ibm.com/software/sla/sladb.nsf/lilookup/986C7686F22D4D3585257E13004EA6CB?OpenDocument
```
{: screen}

### RedHat/SuSE 설치
{: #rpm-install}

1. {{site.data.keyword.SecureGateway}} 클라이언트를 설치하십시오. 예를 들어 RPM 패키지를 사용하는 경우 다음 명령을 실행하십시오.

   ```
   rpm -ivhf <secure-gateway-rpm-package>
   ```
   {: pre}


   클라이언트 설치 프로그램이 시작되어 클라이언트를 설치하고 /etc/ibm에 sgenvironment.conf 파일을 작성합니다.

2. 선택사항: 시스템의 시작 프로세스를 사용하려는 경우 클라이언트가 올바르게 시작되도록 이 파일을 편집하여 다음 항목을 제공해야 합니다. 이 구성 파일을 편집하는 방법에 대한 자세한 정보는 [시작 사용](/docs/services/SecureGateway/securegateway_auto-start.html#linux)을 참조하십시오.

3. 시작을 사용하여 클라이언트를 시작한 경우 올바르게 실행 중인지 확인하기 위해 로그 파일을 확인하십시오.

   ```
cat /var/log/securegateway/client_console.log
   ```
   {: pre}

4. 클라이언트 설치 상태를 확인하려면 다음 명령을 실행하십시오.

   ```
   rpm -q ibm-securegateway-client
   ```
   {: pre}

### 대화식 클라이언트 세션 시작
{: #linux-run}

클라이언트를 시작하려면 다음 명령을 실행하십시오.

```
cd /opt/ibm/securegateway/client
node lib/secgwclient.js <gateway ID> -t <security token>
```
{: codeblock}

이 명령에서는 일반적으로 두 개의 매개변수({{site.data.keyword.SecureGateway}} 게이트웨이 ID 및 게이트웨이의 보안 토큰)를 사용하며, 둘 다 {{site.data.keyword.SecureGateway}} 대시보드를 통해 사용할 수 있습니다.

[시작하기 - 클라이언트 추가](/docs/services/SecureGateway/securegateway_client.html)로 돌아가십시오.

## Windows
{: #windows}

### 클라이언트 설치
{: #windows-install}

시스템의 보안 설정에 따라 이 설치를 수행하기 위해 관리 권한이 필요할 수도 있습니다.

 1. EXE 설치 프로그램 파일을 시스템에 복사하십시오.  설치 패키지의 무결성을 확인하는 데 사용하는
MD5 합계도 제공됩니다.

 2. 두 번 클릭한 후 프롬프트에 응답하거나 명령 프롬프트에서 실행 파일을 실행하여 설치하십시오.

 3. 사용자에게 원하는 설치 디렉토리를 선택하도록 요청하는 프롬프트가 표시됩니다. 기본 위치는 Program Files (x86) 디렉토리입니다.

 4. 사용자에게 명령행 인터페이스를 실행하는 언어를 선택하도록 요청하는 프롬프트가 표시됩니다. 언어를 선택하지 않을 경우 기본값인 영어로 설정됩니다.

 5. 사용자에게 클라이언트를 Windows 서비스로 설치할 것인지 확인하는 프롬프트가 표시됩니다. 사용자가 그렇게 하도록 선택하는 경우 사용자가 후속 대화 상자에서 제공하는 구성을 사용하여 클라이언트를 백그라운드에서 Windows 서비스로 실행합니다. 서비스의 상태는 제어판의 서비스 페이지에서 확인할 수 있습니다. 서비스 이름은 "IBM Bluemix Secure Gateway Service"입니다.

 6. 설치 프로그램에서 시스템에 기존 설치가 존재하는지 여부를 식별합니다. 발견되는 경우 사용자에게 기존 구성을 사용할 것인지 여부를 확인하며, 그렇지 않을 경우 사용자가 새 구성을 입력할 수 있습니다. 여기에서 각각의 게이트웨이에 대한 게이트웨이 ID, 보안 토큰, ACL 파일 및 로그 레벨 등의 세부사항을 제공할 수 있습니다. 사용자가 클라이언트를 Windows 서비스로 실행하도록 선택한 경우 제공된 구성을 사용하여 클라이언트를 실행합니다. 사용자가 클라이언트를 Windows 서비스로 실행하도록 선택하지 않은 경우 나중에 사용하기 위해 구성이 저장됩니다. 두 경우 모두 %Installation_directory%/ibm/securegateway/client/securegw_service.config에 구성이 저장됩니다.

 7. 버전 1.5.0부터는 클라이언트가 전체 기능 클라이언트 UI와 함께 제공됩니다. 설치 프로그램 자체에서 이 UI를 구성할 수 있습니다. 사용자는 클라이언트 UI를 실행할 비밀번호 및 포트 번호(기본값은 9003)를 제공할 수 있습니다.

 사용자가 복수의 게이트웨이에 대한 연결이 포함된 클라이언트를 실행하도록 선택하는 경우 구성 값을 제공할 때 주의하십시오. 게이트웨이 ID는 공백으로 구분해야 합니다. 보안 토큰, ACL 파일 및 로그 레벨은 --로 구분해야 합니다. 특정 게이트웨이에 대해 이러한 세 가지 값 중 하나를 제공하지 않으려면 'none'을 사용하십시오.

 <b>참고:</b> 잔여 공백이 존재하지 않는지 확인하십시오.

 <b>참고:</b> ACL 파일은 %Installation_directory%/ibm/securegateway/client 디렉토리 또는 해당 상대 경로에 배치되어야 합니다.

### 대화식 클라이언트 세션 시작
{: #windows-run}

클라이언트를 시작하려면 관리 권한으로 명령 프롬프트를 열고 다음 명령을 실행하십시오.

```
cd "<Installation_directory>\ibm\securegateway\client"
secgw.cmd
```
{: codeblock}

또는 `<Installation_directory>\ibm\securegateway\client`로 이동하여 `secgw.cmd`를 두 번 클릭하십시오.

<b>참고:</b> `<Installation_directory>\ibm\securegateway\client\securegw_service.config` 파일에 저장된 구성을 사용하도록 선택하거나 대화식으로 세부사항을 제공할 수 있습니다.

[시작하기 - 클라이언트 추가](/docs/services/SecureGateway/securegateway_client.html)로 돌아가십시오.

## DataPower
{: #datapower}

DataPower에는 임베디드 버전의 {{site.data.keyword.SecureGateway}} 클라이언트가 포함되어 있습니다. DataPower 버전에 따라 서로 다른 버전의 {{site.data.keyword.SecureGateway}} 클라이언트가 포함되어 있을 수 있습니다. 적용 가능한 [DataPower 클라이언트 제한사항](/docs/services/SecureGateway/securegateway_interaction.html#limits-datapower)을 파악하십시오. 기존 Secure Gateway 클라이언트를 사용하는 경우 예기치 않은 오류가 발생할 수 있습니다.

| DataPower 버전 | {{site.data.keyword.SecureGateway}} 클라이언트 버전  |
| -- | --  |
| 7.2.0.0, 7.5.0.0 | 1.1.0  |
| 7.5.1.0, 7.7.0 | 1.4.2  |
| 7.5.2.4 | 1.6.1  |
| 7.5.2.6, 7.6.0.0 | 1.7.0  |
| 7.5.2.14, 7.6.0.7, 7.7.1.0 |  1.8.0fp6  |

### 클라이언트 세션 시작
{: #datapower-run}

1. DataPower 웹 GUI에 로그인하십시오.
2. 오브젝트 -> 클라우드 -> Secure Gateway 클라이언트로 이동하거나 Secure Gateway 클라이언트를 검색하십시오.
3. `추가`를 클릭하여 새 클라이언트 연결을 구성하십시오.
4. 이름, 게이트웨이 ID 및 보안 토큰(적용 가능한 경우)을 제공한 후 변경사항을 적용하십시오.

[시작하기 - 클라이언트 추가](/docs/services/SecureGateway/securegateway_client.html)로 돌아가십시오.
