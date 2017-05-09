# 嵌入式 DS 安装

### 环境
| 版本 | 备注
|------|----
| Centos6 x64  | 因为官方只有 yum 源，因此尽量用 CentOS 或 OpenSUSE
| apache  | 需要 mod_ssl 或 mod_nss 以开启 https
| shibboleth-sp  | 嵌入式 DS 必须与 sp 安装在同一环境中

### 安装
#### 准备 repo
官方 repo 地址是 http://download.opensuse.org/repositories/security://shibboleth 国内访问可能比较慢，可以选择华东师大的镜像

在 http://mirrors.ecnu.edu.cn/repositories/security://shibboleth/ 下载添加 yum repo
注意修改 repo 中的 地址
```
[security_shibboleth]
name=Shibboleth (CentOS_CentOS-6)
type=rpm-md
baseurl=http://mirrors.ecnu.edu.cn/repositories/security:/shibboleth/CentOS_CentOS-6/
gpgcheck=1
gpgkey=http://mirrors.ecnu.edu.cn/repositories/security:/shibboleth/CentOS_CentOS-6//repodata/repomd.xml.key
enabled=1
```
#### yum 安装
```yum install shibboleth-embedded-ds```

#### 安装目录

默认会安装到 ```/etc/shibboleth-ds```
同时会自动配置 apache ，对此目录下的 5 个文件路径别名
```
# Basic Apache configuration

<IfModule mod_alias.c>
  <Location /shibboleth-ds>
    Allow from all
    <IfModule mod_shib.c>
      AuthType shibboleth
      ShibRequestSetting requireSession false
      require shibboleth
    </IfModule>
  </Location>
  Alias /shibboleth-ds/idpselect_config.js /etc/shibboleth-ds/idpselect_config.js
  Alias /shibboleth-ds/idpselect.js /etc/shibboleth-ds/idpselect.js
  Alias /shibboleth-ds/idpselect.css /etc/shibboleth-ds/idpselect.css
  Alias /shibboleth-ds/index.html /etc/shibboleth-ds/index.html
  Alias /shibboleth-ds/blank.gif /etc/shibboleth-ds/blank.gif
</IfModule>
```

#### 使用

嵌入式 ds 非常简单，只需要在您的网站中嵌入他的 js 和 css 文件即可。
```
<link rel="stylesheet" type="text/css" href="idpselect.css">
```
```
<div id="idpSelect"></div>
```
```
<!-- Load languages scripts -->
<script src="idpselect_config.js" type="text/javascript" language="javascript"></script>
<script src="idpselect.js" type="text/javascript" language="javascript"></script>
```

默认提供了一个 index.html ，供测试

#### SP 配置

只需讲 SP 的 SSO 重定向指向嵌入式 DS 所在的 web 页即可，以默认的 index.html 为例
```
<SSO discoveryProtocol="SAMLDS" discoveryURL="https://sp.example.org/shibboleth-ds/index.html">
   SAML2 SAML1
</SSO>
```