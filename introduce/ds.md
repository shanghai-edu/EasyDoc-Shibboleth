# Discovery Service

问题在于，SP 是如何同时与多个 IDP 工作的呢？他怎么知道要把认证的 Response 发给哪个 IDP 呢？

答案是：直接问用户，这就是 IDP Discovery
Shibboleth 提供两种可用于 IDP Discovery 的产品。嵌入式 DS(EDS) 和 SP 共同工作，以便于在你的站点上展示 IDP 的选择界面，而集中式 DS(CDS) 则是一个可以独立部署的中央式应用，SP 可以将请求转发给他，来展示 IDP 的选择界面。

我们鼓励 SP 使用 EDS，因为它能带来更好的用户体验。

