---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 클라이언트 설치 제거

## Docker
{: #docker}

{{site.data.keyword.SecureGateway}} 클라이언트 Docker 이미지를 제거하려면 다음 명령을 실행하십시오.

```
docker rmi ibmcom/secure-gateway-client
```
{: pre}

## Mac OS X
{: #mac}

Mac OS X에서 {{site.data.keyword.SecureGateway}} 클라이언트를 제거하려면 터미널에서 다음 명령을 실행하십시오.

```
cd /Applications
rm -rf ./ibm
```
{: codeblock}

## Linux
{: #linux}

### Ubuntu/PowerPC
{: #debian-uninstall}

다음 명령을 사용하여 {{site.data.keyword.SecureGateway}} 클라이언트의 설치 상태를 찾아볼 수 있습니다.

```
sudo dpkg -l | grep ibm-securegateway-client
```
{: pre}

설치 디렉토리, 구성 파일, ID, 그룹 ID 및 로그 파일을 제거하지 않고 {{site.data.keyword.SecureGateway}} 클라이언트를
설치 제거하려면 다음 명령을 사용하십시오.

```
sudo dpkg -r ibm-securegateway-client
```
{: pre}

모든 항목을 제거하면서 {{site.data.keyword.SecureGateway}} 클라이언트를 설치 제거하려면 다음 명령을 사용하십시오.

```
sudo dpkg --purge ibm-securegateway-client
```
{: pre}

### RedHat/SuSE
{: #rpm-uninstall}

다음 명령을 사용하여 {{site.data.keyword.SecureGateway}} 클라이언트의 설치 상태를 찾아볼 수 있습니다.

```
rpm -qa | grep ibm-securegateway-client
```
{: pre}

설치 디렉토리, 구성 파일, ID, 그룹 ID 및 로그 파일을 제거하지 않고 {{site.data.keyword.SecureGateway}} 클라이언트를
설치 제거하려면 다음 명령을 사용하십시오.

```
rpm -e ibm-securegateway-client
```
{: pre}

모든 항목을 제거하면서 {{site.data.keyword.SecureGateway}} 클라이언트를 설치 제거하려면 다음 명령을 사용하십시오.

```
yum remove ibm-securegateway-client
```
{: pre}

## Windows
{: #windows}

{{site.data.keyword.SecureGateway}} 클라이언트의 Windows 설치를 제거하는 두 가지 방법이 있습니다.

1. 설치 디렉토리에서 제거: {{site.data.keyword.SecureGateway}} 클라이언트의 설치 디렉토리로 이동하여 uninstall.exe를 두 번 클릭하십시오.

2. 제어판에서 제거: 제어판의 프로그램 섹션에서 {{site.data.keyword.SecureGateway}} 애플리케이션을 제거하십시오.
