# 属性获取配置
idp 3 的属性获取配置与 idp 2 比较类似

在 ```conf/attribute-resolver.xml``` 处修改

#### 定义 idP 所需要获取的属性

```
    <AttributeDefinition id="uid" xsi:type="Simple" sourceAttributeID="uid">
        <Dependency ref="myLDAP" />
        <AttributeEncoder xsi:type="SAML1String" name="urn:mace:dir:attribute-def:uid" encodeType="false" />
        <AttributeEncoder xsi:type="SAML2String" name="urn:oid:2.5.4.2" friendlyName="uid" encodeType="false" />
    </AttributeDefinition>

    <!--
    In the rest of the world, the email address is the standard identifier,
    despite the problems with that practice. Consider making the EPPN value
    the same as your official email addresses whenever possible.
    -->
    <AttributeDefinition id="email" xsi:type="Simple" sourceAttributeID="mail">
        <Dependency ref="myLDAP" />
        <AttributeEncoder xsi:type="SAML1String" name="urn:mace:dir:attribute-def:mail" encodeType="false" />
        <AttributeEncoder xsi:type="SAML2String" name="urn:oid:0.9.2342.19200300.100.1.3" friendlyName="mail" encodeType="false" />
    </AttributeDefinition>
    <AttributeDefinition id="cn" xsi:type="Simple" sourceAttributeID="cn">
        <Dependency ref="myLDAP" />
        <AttributeEncoder xsi:type="SAML1String" name="urn:mace:dir:attribute-def:cn" encodeType="false" />
        <AttributeEncoder xsi:type="SAML2String" name="urn:oid:2.5.4.3" friendlyName="cn" encodeType="false" />
    </AttributeDefinition>
    <AttributeDefinition id="domainName" xsi:type="Simple" sourceAttributeID="domainName">
        <Dependency ref="staticAttributes" />
        <AttributeEncoder xsi:type="SAML1String" name="urn:mace:dir:attribute-def:domainName" encodeType="false" />
        <AttributeEncoder xsi:type="SAML2String" name="urn:oid:2.5.4.5" friendlyName="domainName" encodeType="false" />
    </AttributeDefinition>
```

这里的xsi:type="enc:SAML2String" name="urn:oid:2.5.4.2"，需要遵循联盟内的属性标准，可与联盟的管理单位联系确认。

#### 静态连接器获取属性
部分数据我们通过静态方式获取，因此需要配置静态的数据连接器。下面我们创建一个静态连接器，赋予 ```domainName = example.org ``` 这个值

```
    <DataConnector id="staticAttributes" xsi:type="Static">
        <Attribute id="domainName">
            <Value>example.org</Value>
        </Attribute>
    </DataConnector>
```

##### ldap 连接器
此处与 idp2 不同。其配置信息可以从 ```ldap.properties``` 中共享，由于刚才已经配置了 ldap 认证，因此实际上不需要重复配置。
示例
```
    <DataConnector id="myLDAP" xsi:type="LDAPDirectory"
        ldapURL="%{idp.attribute.resolver.LDAP.ldapURL}"
        baseDN="%{idp.attribute.resolver.LDAP.baseDN}" 
        principal="%{idp.attribute.resolver.LDAP.bindDN}"
        principalCredential="%{idp.attribute.resolver.LDAP.bindDNCredential}"
        useStartTLS="%{idp.attribute.resolver.LDAP.useStartTLS:false}"
        connectTimeout="%{idp.attribute.resolver.LDAP.connectTimeout}"
        responseTimeout="%{idp.attribute.resolver.LDAP.responseTimeout}">
        <FilterTemplate>
            <![CDATA[
                %{idp.attribute.resolver.LDAP.searchFilter}
            ]]>
        </FilterTemplate>
        <ConnectionPool
            minPoolSize="%{idp.pool.LDAP.minSize:3}"
            maxPoolSize="%{idp.pool.LDAP.maxSize:10}"
            blockWaitTime="%{idp.pool.LDAP.blockWaitTime:PT3S}"
            validatePeriodically="%{idp.pool.LDAP.validatePeriodically:true}"
            validateTimerPeriod="%{idp.pool.LDAP.validatePeriod:PT5M}"
            expirationTime="%{idp.pool.LDAP.idleTime:PT10M}"
            failFastInitialize="%{idp.pool.LDAP.failFastInitialize:false}" />
    </DataConnector>

```


