# Profile

SAML规范由几个文档构成：

核心文档定义了所有的各种xml元素和属性，并对其内容设置了一些基本的规则（比如属性 X 必须是个正数）。核心文档有点像字典，他定义了一些词但是没有指定如何把他们放在一起并产生些有意义的东西。

Porfile文档描述了如何去通过核心文档定义的元素去执行一些特点的功能（比如，执行一个 SSO 的请求）。他实质上是提供了一些语法的规则。

Profile 是 SAML 内互操作的一个单元（通过 Shibbileth 扩展），这意味如果产品支持给定的 profile，则产品是可以互操作的。Shibboleth 支持的

profile 列表可以在以下链接找到。
（https://wiki.shibboleth.net/confluence/display/DEV/Supported+Protocols）
