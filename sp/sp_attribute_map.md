# 属性过滤器

修改属性过滤器配置文件，```/etc/attribute-map.xml```

```
  <Attribute name="urn:mace:dir:attribute-def:uid" id="uid" />
  <Attribute name="urn:mace:dir:attribute-def:email" id="email" />
  <Attribute name="urn:mace:dir:attribute-def:cn" id="cn" />
  <Attribute name="urn:mace:dir:attribute-def:domainName" id="domainName" />
  <Attribute name="urn:oid:2.5.4.2" id="uid" />
  <Attribute name="urn:oid:2.5.4.3" id="cn" />
  <Attribute name="urn:oid:2.5.4.5" id="domainName" />
  <Attribute name="urn:oid:0.9.2342.19200300.100.1.3" id="email" />
```
这里定义了 shibboleth 接受的属性，需要符合区域认证联盟内的属性标准规范。