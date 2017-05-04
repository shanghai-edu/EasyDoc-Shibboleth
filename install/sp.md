# SP 安装

### 环境
| 版本 | 备注
|------|----
| Centos6.7 x64  | 因为官方只有 yum 源，因此尽量用 CentOS 或 OpenSUSE
| apache  | 需要 mod_ssl 或 mod_nss 以开启 https

#### 关闭 selinux
#### 临时关闭
setenforce 0 
#### 永久生效
修改 ```/etc/selinux/config``` 将其 disable
```
# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=disabled
# SELINUXTYPE= can take one of these two values:
#     targeted - Targeted processes are protected,
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted
```
然后重启服务器

### 安装
#### 准备 repo
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
```yum install shibboleth httpd mod_ssl php -y```

#### apache 配置

修改 ```/etc/httpd/conf.d/shib.conf``` 文件
这表示被 Shibboleth 保护的路径，访问此路径将会进行跨校认证。
```
<Location /secure>
  AuthType shibboleth
  ShibCompatWith24 On
  ShibRequestSetting requireSession 1
  require shib-session
</Location>
<Location /test>
  AuthType shibboleth
  ShibCompatWith24 On
  ShibRequestSetting requireSession 1
  require shib-session
</Location>
```

### 服务
启动服务
```service shibd start```
```service httpd start```

由于 Shibboleth 服务对时间敏感，因此需要配置 ntp，不再赘述
```service ntpd start```

