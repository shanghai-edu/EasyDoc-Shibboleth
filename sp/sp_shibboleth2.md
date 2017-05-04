# 主配置

shibboleth 主要的配置文件在 ```/etc/shibboleth``` 下，主要的配置文件是```shibboleth2.xml```

修改 ```entityID```
```
<ApplicationDefaults entityID="https://sp.example.org/shibboleth"
         REMOTE_USER="eppn persistent-id targeted-id">
```
修改 ```SSO```
```
<SSO discoveryProtocol="SAMLDS" discoveryURL="https://ds.example.org/ds/WAYF">
  SAML2 SAML1
</SSO>
```
修改 ```MetadataProvider```
```
<MetadataProvider type="XML" uri="https://ds.example.org/metadata.xml"    
 backingFilePath="metadata.xml" legacyOrgNames="true" reloadInterval="7200"/>
```