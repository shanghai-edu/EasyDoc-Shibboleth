# 认证配置
## 用户认证配置

在 idp 3 的版本中，认证的配置文件都在 conf/authn/* 下
```
[root@idp authn]# pwd
/opt/idp/conf/authn
[root@idp authn]# ls
authn-comparison.xml   duo.properties             ipaddress-authn-config.xml  krb5-authn-config.xml  password-authn-config.xml             spnego-authn-config.xml
authn-events-flow.xml  external-authn-config.xml  jaas-authn-config.xml       ldap-authn-config.xml  remoteuser-authn-config.xml           x509-authn-config.xml
duo-authn-config.xml   general-authn.xml          jaas.config                 mfa-authn-config.xml   remoteuser-internal-authn-config.xml  x509-internal-authn-config.xml
[root@idp authn]# 
```

idp 3 支持多种认证方式，包括
- Password
- RemoteUser
- RemoteUserInternal
- X509
- X509Internal
- SPNEGO / Kerberos 3.2以后版本支持
- IPAddress
- External
- Duo 3.3
- Multi-Factor 3.3 以后版本子支持

这里介绍 Password 方式认证

### Password 认证

Password 认证主要涉及的配置文件是 ```conf/authn/password-authn-config.xml```
它支持三种认证后端对接
- LDAPAuthnConfiguration
- JAASAuthnConfiguration
- KerberosAuthnConfiguration

这里介绍 LDAP 方式对接
#### LDAP 认证

LDAP 认证涉及的配置文件是 ```conf/authn/ldap-authn-config.xml``` 和 ```/conf/ldap.properties```

首先在 ```authn/password-authn-config.xml``` 配置文件中，要引入 ldap 认证（默认）
```
<import resource="ldap-authn-config.xml" />
```
然后修改 ldap.properties 
```



# LDAP authentication configuration, see authn/ldap-authn-config.xml
# Note, this doesn't apply to the use of JAAS

## Authenticator strategy, either anonSearchAuthenticator, bindSearchAuthenticator, directAuthenticator, adAuthenticator
idp.authn.LDAP.authenticator                   = bindSearchAuthenticator

## Connection properties ##
idp.authn.LDAP.ldapURL                          = ldap://localhost:10389
idp.authn.LDAP.useStartTLS                     = false
#idp.authn.LDAP.useSSL                          = false
# Time in milliseconds that connects will block
#idp.authn.LDAP.connectTimeout                  = PT3S
# Time in milliseconds to wait for responses
#idp.authn.LDAP.responseTimeout                 = PT3S

## SSL configuration, either jvmTrust, certificateTrust, or keyStoreTrust
#idp.authn.LDAP.sslConfig                       = certificateTrust
## If using certificateTrust above, set to the trusted certificate's path
idp.authn.LDAP.trustCertificates                = %{idp.home}/credentials/ldap-server.crt
## If using keyStoreTrust above, set to the truststore path
idp.authn.LDAP.trustStore                       = %{idp.home}/credentials/ldap-server.truststore

## Return attributes during authentication
idp.authn.LDAP.returnAttributes                 = uid,cn,mail

## DN resolution properties ##

# Search DN resolution, used by anonSearchAuthenticator, bindSearchAuthenticator
# for AD: CN=Users,DC=example,DC=org
idp.authn.LDAP.baseDN                           = dc=example,dc=org
idp.authn.LDAP.subtreeSearch                   = true
idp.authn.LDAP.userFilter                       = (uid={user})
# bind search configuration
# for AD: idp.authn.LDAP.bindDN=adminuser@domain.com
idp.authn.LDAP.bindDN                           = cn=manager,dc=example,dc=org
idp.authn.LDAP.bindDNCredential                 = ChangeMe

# Format DN resolution, used by directAuthenticator, adAuthenticator
# for AD use idp.authn.LDAP.dnFormat=%s@domain.com
#idp.authn.LDAP.dnFormat                         = uid=%s,ou=people,dc=example,dc=org

# LDAP attribute configuration, see attribute-resolver.xml
# Note, this likely won't apply to the use of legacy V2 resolver configurations
idp.attribute.resolver.LDAP.ldapURL             = %{idp.authn.LDAP.ldapURL}
idp.attribute.resolver.LDAP.connectTimeout      = %{idp.authn.LDAP.connectTimeout:PT3S}
idp.attribute.resolver.LDAP.responseTimeout     = %{idp.authn.LDAP.responseTimeout:PT3S}
idp.attribute.resolver.LDAP.baseDN              = %{idp.authn.LDAP.baseDN:undefined}
idp.attribute.resolver.LDAP.bindDN              = %{idp.authn.LDAP.bindDN:undefined}
idp.attribute.resolver.LDAP.bindDNCredential    = %{idp.authn.LDAP.bindDNCredential:undefined}
idp.attribute.resolver.LDAP.useStartTLS         = %{idp.authn.LDAP.useStartTLS:true}
idp.attribute.resolver.LDAP.trustCertificates   = %{idp.authn.LDAP.trustCertificates:undefined}
idp.attribute.resolver.LDAP.searchFilter        = (uid=$resolutionContext.principal)

# LDAP pool configuration, used for both authn and DN resolution
#idp.pool.LDAP.minSize                          = 3
#idp.pool.LDAP.maxSize                          = 10
#idp.pool.LDAP.validateOnCheckout               = false
#idp.pool.LDAP.validatePeriodically             = true
#idp.pool.LDAP.validatePeriod                   = PT5M
#idp.pool.LDAP.prunePeriod                      = PT5M
#idp.pool.LDAP.idleTime                         = PT10M
#idp.pool.LDAP.blockWaitTime                    = PT3S
```

注意 ```#``` 配置项存在 default值，如果实际的配置与 defualt 不同的话，除了开启 ```#```,还需要在 ```conf/authn/ldap-authn-config.xml``` 中修改，去掉 defualt 值。（也可以直接在 ```conf/authn/ldap-authn-config.xml``` 里写死）

例如这里的 ldap 示例并未开启 tls，因此去掉了```conf/authn/ldap-authn-config.xml```中的 TLS 默认值。同时去掉 ssl 的证书路径配置选项。
```
    <bean id="connectionConfig" class="org.ldaptive.ConnectionConfig" abstract="true" p:ldapUrl="%{idp.authn.LDAP.ldapURL}"
        p:useStartTLS="%{idp.authn.LDAP.useStartTLS}"
        p:useSSL="%{idp.authn.LDAP.useSSL:false}"
        p:connectTimeoutDuration="%{idp.authn.LDAP.connectTimeout:PT3S}"
        p:responseTimeoutDuration="%{idp.authn.LDAP.responseTimeout:PT3S}" />
```
