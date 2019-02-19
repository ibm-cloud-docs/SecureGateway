---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 클라이언트 자동 시작 구성

## Linux
{: #linux}

시스템의 자동 시작 기능을 사용하도록 선택한 경우 다음 방법 중 하나를 사용하여 클라이언트를 시작하십시오. 클라이언트에서 사용할 구성 파일은 다음 위치에서 찾을 수 있습니다.

```
/etc/ibm/sgenvironment.conf
```
{: screen}

<b>참고:</b> Red Hat Linux 6의 경우 자동 시작 기능을 사용할 수 없습니다.

이 파일에는 다음과 같이 설정할 중요한 변수가 포함되어 있습니다.

| 환경 변수 |설명       |
| ------------- | ----------- |
|RESTART_CLIENT | 설치 또는 업그레이드 중에 클라이언트 중지 및 다시 시작(예 또는 아니오) |
|GATEWAY_ID | Secure Gateway for Bluemix UI에서 작성된 것과 동일한 게이트웨이 ID  |
|SECTOKEN | 이 게이트웨이 ID를 위한 보안 토큰(작성 시 보안을 적용하도록 선택한 경우) |
|ACL_FILE | 리소스에 대한 온프레미스 액세스를 제한하기 위해 사용할 액세스 제어 목록 파일 |
|LOGLEVEL | 서비스에 대해 설정할 로그 레벨(기본값은 INFO) |
| USE_UI   | 클라이언트 UI를 실행하지 않으려는 경우 이 변수를 'N'으로 설정 |
| UI_PORT  | 클라이언트 UI를 실행할 포트(기본값은 9003) |
| LANGUAGE | 클라이언트가 로그인하도록 하려는 언어(기본값은 en) |

<b>참고:</b> 이 파일은 시스템의 자동 시작 기능을 사용하는 경우에만 읽어들일 수 있습니다. 클라이언트를 수동으로 실행하는 경우 이 파일이 무시됩니다.

### 시작
{: #upstart}

### 클라이언트 시작
{: #upstart-start}

시스템 스타트업 시 Secure Gateway 클라이언트가 자동으로 시작되도록 시작 기능을 사용하는 경우 먼저 해당 구성을 확인해야 합니다(/etc/ibm/sgenvironment.conf). 시작 서비스를 구성한 후에는 다음 명령을 사용하여 해당 서비스를 시작할 수 있습니다.

```
sudo initctl start securegateway_client
```
{: pre}

서비스를 다시 시작하려면 다음 명령을 실행하십시오.

```
sudo initctl restart securegateway_client
```
{: pre}

### 클라이언트 중지
{: #upstart-stop}

서비스를 중지하려면 다음 명령을 실행하십시오.

```
sudo initctl stop securegateway_client
```
{: pre}

### SystemD
{: #systemd}


### 클라이언트 시작
{: #systemd-start}

시스템 스타트업 시 Secure Gateway 클라이언트가 자동으로 시작되도록 systemD 기능을 사용하는 경우 먼저 해당 구성을 확인해야 합니다(/etc/ibm/sgenvironment.conf). 시작 서비스를 구성한 후에는 다음 명령을 사용하여 해당 서비스를 시작할 수 있습니다.

```
systemctl start securegateway_client
```
{: pre}

서비스를 다시 시작하려면 다음 명령을 실행하십시오.

```
systemctl restart securegateway_client
```
{: pre}

서비스의 상태를 확인하려면 다음 명령을 실행하십시오.

```
systemctl status securegateway_client
```
{: pre}

### 클라이언트 중지
{: #systemd-stop}

서비스를 중지하려면 다음 명령을 실행하십시오.

```
systemctl stop securegateway_client
```
{: pre}

### SystemV
{: #systemv}

다른 자동 시작 기능 등을 설치하는 중에는 SystemV가 설정되지 않습니다. 설치 디렉토리(/opt/ibm/securegateway/client/upstart)에서 다음과 같은 스크립트를 사용할 수 있습니다.

```
secgw.sh
securegateway_clientd
```
{: screen}

<b>참고:</b> 이 절의 내용은 SuSE/SLES 11 사용자에게 적용됩니다.

선택사항: systemV 자동 시작을 사용하는 SuSE 버전 11을 실행하는 경우 이 프로세스를 구성하기 위해 사용할 수 있는 스크립트가 제공됩니다. 다음 프로시저를 수행하십시오.

```
cd /opt/ibm/securegateway/client/upstart/systemV
cp secgw.sh /usr/local/bin
chmod +x /usr/local/bin/secgw.sh
cp securegateway_clientd /usr/local/bin
chmod +x /usr/local/bin/securegateway_clientd
cd /etc/init.d
ln -s /usr/local/bin/securegateway_clientd
insserv securegateway_clientd
vi /etc/ibm/sgenvironment.conf
```
{: codeblock}

이러한 단계가 실행되면 YasT 및 systemV 명령을 사용하여 디먼을 시작/중지할 수 있습니다.

[시작하기 - 클라이언트 추가](/docs/services/SecureGateway/securegateway_client.html)로 돌아가십시오.

## Windows
{: #windows}

Windows 서비스의 상태를 변경하려면 관리자 권한으로 명령 창을 여십시오.

```
cd "<Installation_directory>\ibm\securegateway\client"
```
{: pre}

서비스가 이미 실행 중인 경우 사용자는 다음 명령을 사용하여 해당 서비스를 중지할 수 있습니다.

```
windowsService.cmd uninstall
```
{: pre}

서비스를 시작하려면 다음 명령을 사용하십시오.

```
windowsService.cmd install
```
{: pre}

<b>참고:</b> 서비스는 다음 위치에 저장된 구성을 기반으로 실행됩니다.

```
%Installation_directory%/ibm/securegateway/client/securegw_service.config
```
{: pre}

Windows 서비스에 대한 애플리케이션 로그는 다음 위치에 저장됩니다.

```
%Installation_directory%/ibm/securegateway/client/logs/securegw_win_service.log
```
{:pre}

 로그는 매일 새 파일로 합쳐집니다.

[시작하기 - 클라이언트 추가](/docs/services/SecureGateway/securegateway_client.html)로 돌아가십시오.
