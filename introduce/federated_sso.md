# 联邦式的单点登录

如果你听说过 shibboleth，你可能也听说过“联盟”，或者“联邦认证”。上面那些步骤对于所有的单点登系统都是一样的，但是一些系统在设计上要求 IDP 和 SP 在同一个组织内才能工作，同时其他一些在设计上则并不关心这两者在不在同一个组织内。后一种方式被成为联邦认证。

这其实并不算罕见，有时候 SP 会希望同时为多个 IDP 提供服务（比如一些应用希望扩大用户范围，一些资源希望能够为不同组织的研究者共享）。同时一个 IDP 也会希望为多个 SP 提供认证。当一批的 IDP 和 SP 互相达成一致，共同工作，这样的组合就被成为“联盟”
