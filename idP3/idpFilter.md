# 属性过滤配置

idp3 属性过滤器配置也与 idp2 比较类似


#### attribute-filter 配置

修改 idp.home/conf/attribute-filter.xml
这里可以设定哪些属性能够释放给哪些SP。

可仿造其 example 配置

```
    <AttributeFilterPolicy id="example1">
        <PolicyRequirementRule xsi:type="ANY"/>
        <AttributeRule attributeID="uid">
            <PermitValueRule xsi:type="ANY" />
        </AttributeRule>

        <AttributeRule attributeID="email">
            <PermitValueRule xsi:type="ANY" />
        </AttributeRule>
        <AttributeRule attributeID="cn">
            <PermitValueRule xsi:type="ANY" />
        </AttributeRule>
        <AttributeRule attributeID="domainName">
            <PermitValueRule xsi:type="ANY" />
        </AttributeRule>
    </AttributeFilterPolicy>

```


