# 单点登录

1：用户访问资源
用户尝试访问一个被保护的资源。资源监视器会判断用户是否拥有一个有效的 session，如果没有则引导他转向 SP 来开启以 SSO 的流程。

2：SP 发起认证请求
用户访问 SP，SP 发起认证请求，并将用户重定向到用户所在的 IDP 进行认证。SP 组件一般会和资源安装在一台服务器上。

3：IDP 上的用户认证
当用户被重定向IDP服务器上时，IDP 会检查用户是否已经有一个存在的session（即认证过）。如果有的话，直接跳下一步。如果没有，则在 IDP 上发起用户认证（比如通过用户名和密码），然后进行下一步

4：IDP 发起认证 response
用户认证通过后，IDP 产生一个认证 response，并将其发送给 SP，同时用户返回到 SP。

5：SP 检查认证 response
当用户带着 IDP 的认证 response 回到 SP，SP 将验证这个 response，并为用户创建一个 Session ，并从 response 中提取受限资源所需要的一些信息（比如用户的标识）。然后用户被跳转至资源。

6：资源返回内容
如同第一步，用户现在再一次尝试访问这些受保护的资源，但这一次用户有 session 了，并且资源也知道他们的信息了。因此，这一次资源将回应用户的请求，并返回用户请求的数据。

