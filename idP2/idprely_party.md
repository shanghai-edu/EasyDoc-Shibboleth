# relying_party 配置

可以通过修改 ```IDP_HOME/conf/relying_party.xml``` 的方式来修改 relying_party 的配置。这些配置主要和 SAML 相关，通常不需要修改，我们只要把 idp provider 和 metadata provider 的名字改了就行了。

#### relying_party 

部分配置示例

idp provider 修改。
```
    <rp:AnonymousRelyingParty provider="https://idp.example.org/idp/shibboleth" defaultSigningCredentialRef="IdPCredential"/>

    <rp:DefaultRelyingParty provider="https://idp.example.org/idp/shibboleth" defaultSigningCredentialRef="IdPCredential">
```

metadata provider 修改
```
        <metadata:MetadataProvider id="URLMD" xsi:type="metadata:FileBackedHTTPMetadataProvider"
                          metadataURL="https://ds.example.org/metadata.xml"
                          backingFile="/opt/idp/metadata/allinone.xml">
        </metadata:MetadataProvider>
```



