# idP 的 metadata
idp3 的 metadata 配置也与 idp2 比较类似
需要修改 ```idp.home/conf/metadata-providers.xml``` 配置文件

#### Metadata 的获取
修改 ```idp.home/conf/metadata-providers.xml```
增加 metadata 的获取

    <MetadataProvider id="HTTPMetadata"
                      xsi:type="FileBackedHTTPMetadataProvider"
                      backingFile="%{idp.home}/metadata/localCopyFromXYZHTTP.xml"
                      metadataURL="https://ds.exampel.org/metadata.xml">

    </MetadataProvider>


#### Metadata

在 idp 安装完以后，可以在 ```idp.home/metadata/idp-metadata.xml``` 处找到 idp 的 metadata
需要将此文件提交给联盟完成 idp 的注册



