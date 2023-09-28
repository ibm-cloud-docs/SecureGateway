---

copyright:
  years: 2015, 2021
lastupdated: "2021-03-25"

subcollection: SecureGateway

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:deprecated: .deprecated}
{:external: target="_blank" .external}


# Config File
{: #config-files}

{{site.data.keyword.SecureGateway}} is deprecated. For more information, see the [deprecation details](/docs/SecureGateway?topic=SecureGateway-dep-overview).
{: deprecated}

## Linux
{: #config-files-linux}

This is the default content for the config file `sgenvironment.conf`

```
# Stop and Restart the client during install or upgrade (Yes or No)
# If manually modifying the following, accepted values are: Yes, yes, No, or no
RESTART_CLIENT=yes

# Configuration ID to connect
# If manually modifying the following, accepted values are: "<your gateway ids>" separated by spaces
GATEWAY_ID="<gateway id>"
export SECGW_GATEWAYID="$GATEWAY_ID"

# Security Token for this Configuration ID (if any)
# If manually modifying the following, accepted values are: <your gateway id security tokens> ('none' if no token, separated by '--' otherwise)
SECTOKEN=<security token>

# Access Control List File
# If manually modifying the following, accepted values are the absolute path to your ACL files ('none' if no file, separated by '--' otherwise)
ACL_FILE=none

# Logging Level
# If manually modifying the following, accepted values are: INFO, DEBUG, ERROR, TRACE (default value is INFO. Separate with '--')
LOGLEVEL=INFO

# Client UI port
# If manually modifying the following, set USE_UI to N if you dont want to launch the client UI. Default value for client port is 9003.
USE_UI=Y
UI_PORT=9003

# Proxy Info
# If manually modifying the following, accepted values are: "http://<you proxy server hostname/IP>:<your proxy server port>" or "https://<you proxy server hostname/IP>:<your proxy server port>"
# Skip this field if no proxy is required
PROXY=

# Language
# Default is en; acceptable values are en, de, es, fr, it, ja, ko, pt-BR, zh, zh-TW
LANGUAGE=en

# Startup options
# If manually modifying the following, accepted values are mentioned in this document
# https://cloud.ibm.com/docs/services/SecureGateway?topic=securegateway-client-interacting#startup-args
SECGW_ARGS="--no_license --l $LOGLEVEL --service"

#
# Add arguments only if set
#
if [ 'X_'$SECTOKEN != 'X_' ]; then
    SECGW_ARGS="$SECGW_ARGS --t $SECTOKEN "
fi
if [ 'X_'$ACL_FILE != 'X_' ]; then
    SECGW_ARGS="$SECGW_ARGS --F $ACL_FILE "
fi
if [ 'X_'$PROXY != 'X_' ]; then
    SECGW_ARGS="$SECGW_ARGS --proxy $PROXY "
fi
if [ 'X_'$USE_UI == 'X_N' ] ; then
    SECGW_ARGS="$SECGW_ARGS --noUI "
elif [ 'X_'$UI_PORT != 'X_' ]; then
    SECGW_ARGS="$SECGW_ARGS --port $UI_PORT "
fi

#
# Export the arguments
#
export SECGW_ARGS

export OPERSYS=<os info>
export VERSION=<os version>
```
{: codeblock}

### Multiple client connections
{: #config-files-linux-multiple}

```
# Stop and Restart the client during install or upgrade (Yes or No)
# If manually modifying the following, accepted values are: Yes, yes, No, or no
RESTART_CLIENT=yes

# Configuration ID to connect
# If manually modifying the following, accepted values are: "<your gateway ids>" separated by spaces
GATEWAY_ID="<gateway id 1> <gateway id 2>"
export SECGW_GATEWAYID="$GATEWAY_ID"

# Security Token for this Configuration ID (if any)
# If manually modifying the following, accepted values are: <your gateway id security tokens> ('none' if no token, separated by '--' otherwise)
SECTOKEN=<security token 1>--<security token 2>

# Access Control List File
# If manually modifying the following, accepted values are the absolute path to your ACL files ('none' if no file, separated by '--' otherwise)
ACL_FILE=none--none

# Logging Level
# If manually modifying the following, accepted values are: INFO, DEBUG, ERROR, TRACE (default value is INFO. Separate with '--')
LOGLEVEL=INFO--INFO

# Client UI port
# If manually modifying the following, set USE_UI to N if you dont want to launch the client UI. Default value for client port is 9003.
USE_UI=Y
UI_PORT=9003

# Proxy Info
# If manually modifying the following, accepted values are: "http://<you proxy server hostname/IP>:<your proxy server port>" or "https://<you proxy server hostname/IP>:<your proxy server port>"
# Skip this field if no proxy is required
PROXY=

# Language
# Default is en; acceptable values are en, de, es, fr, it, ja, ko, pt-BR, zh, zh-TW
LANGUAGE=en

# Startup options
# If manually modifying the following, accepted values are mentioned in this document
# https://cloud.ibm.com/docs/services/SecureGateway?topic=securegateway-client-interacting#startup-args
SECGW_ARGS="--no_license --l $LOGLEVEL --service"

#
# Add arguments only if set
#
if [ 'X_'$SECTOKEN != 'X_' ]; then
    SECGW_ARGS="$SECGW_ARGS --t $SECTOKEN "
fi
if [ 'X_'$ACL_FILE != 'X_' ]; then
    SECGW_ARGS="$SECGW_ARGS --F $ACL_FILE "
fi
if [ 'X_'$PROXY != 'X_' ]; then
    SECGW_ARGS="$SECGW_ARGS --proxy $PROXY "
fi
if [ 'X_'$USE_UI == 'X_N' ] ; then
    SECGW_ARGS="$SECGW_ARGS --noUI "
elif [ 'X_'$UI_PORT != 'X_' ]; then
    SECGW_ARGS="$SECGW_ARGS --port $UI_PORT "
fi

#
# Export the arguments
#
export SECGW_ARGS

export OPERSYS=<os info>
export VERSION=<os version>
```
{: codeblock}


## Windows
{: #config-files-windows}

This is the default content for the config file `securegw_service.config`

```
#Config file for Secure Gateway Client, to start as a Windows Service.
#PLEASE AVOID ANY RESIDUAL WHITE SPACES

#Enter the gateway ids separated by single spaces
GATEWAY_ID=<gateway id>

#Enter the security tokens separated by --
SECTOKEN=<security token>

#Enter the ACL files separated by --
ACL_FILE=

# Enter the proxy info if you want to start the client with the proxy
PROXY=

#Enter the log levels separated by --
LOGLEVEL=INFO

#Enter if you want to use client UI (Y/N)
USE_UI=

#Enter the port for client UI
PORT=9003

#Enter the password for client UI
PWD=

#Enter the language for command line interface; acceptable values are en, de, es, fr, it, ja, ko, pt-BR, zh, zh-TW
LANGUAGE=en

#Enter the startup options separated by single spaces, no "" is needed
#Accepted values are mentioned in this document
#https://cloud.ibm.com/docs/services/SecureGateway?topic=securegateway-client-interacting#startup-args
SECGW_ARGS=
```
{: codeblock}

### Multiple client connections
{: #config-files-windows-multiple}

```
#Config file for Secure Gateway Client, to start as a Windows Service.
#PLEASE AVOID ANY RESIDUAL WHITE SPACES

#Enter the gateway ids separated by single spaces
GATEWAY_ID=<gateway id 1> <gateway id 2>

#Enter the security tokens separated by --
SECTOKEN=<security token 1>--<security token 2>

#Enter the ACL files separated by --
ACL_FILE=<path1>--<path2>

# Enter the proxy info if you want to start the client with the proxy
PROXY=

#Enter the log levels separated by --
LOGLEVEL=INFO--INFO

#Enter if you want to use client UI (Y/N)
USE_UI=

#Enter the port for client UI
PORT=9003

#Enter the password for client UI
PWD=

#Enter the language for command line interface; acceptable values are en, de, es, fr, it, ja, ko, pt-BR, zh, zh-TW
LANGUAGE=en

#Enter the startup options separated by single spaces, no "" is needed
#Accepted values are mentioned in this document
#https://cloud.ibm.com/docs/services/SecureGateway?topic=securegateway-client-interacting#startup-args
SECGW_ARGS=
```
{: codeblock}
