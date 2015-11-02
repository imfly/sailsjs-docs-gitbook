# 扩充（Scaling）

如果你预料到会有大流量到应用程序（或者更好的是，你已经拥有大流量！），你要建立一个可扩充的架构，让应用程序可以随著越来越多人使用而进行扩充。

### 效能基准（Benchmarks）

在大多数情况下，Sails 效能与任何 Connect、Express 或 Socket.io 应用程序相同。这已在几个不同的场合下被证实，最近一次是在[这里](http://serdardogruyol.com/?p=111)。如果你有自己的效能基准想和大家分享，请在 Github 发送 pull request 到本页面。


### 例子架构

```
　　　　　          Sails.js 服务器
　　　　　                ....                 
　　　　　       /  Sails.js 服务器  \      /  资料库（如 Mongo、Postgres 等）
负载平衡器  <-->    Sails.js 服务器    <-->    Socket 储存区（Redis）
　　　　　       \  Sails.js 服务器  /      \  会话（Session）储存区（Redis）
　　　　　                ....                 
　　　　　          Sails.js 服务器
```


### 设置应用程序的丛集部署

+ 确保模型所使用的资料库（如 MySQL、Postgres、Mongo）具有可扩充性（如分片丛集）
+ 设置应用程序使用共享的会话（Session）储存区
  + 内建支持 redis（查看 `config/session.js` 内的 `adapter` 选项）
+ 如果你使用 SOCKETS：
  + 设置应用程序使用共享的 socket 储存区
  + 内建支持 redis（查看 `config/sockets.js` 内的 `adapter` 选项）
  + 注意：如果你不想设置 socket 储存区，这种状况下可行的解决方案是在负载平衡器使用黏性会话（sticky sessions）。
+ 确保应用程序可能会使用的其他相依功能没有依赖于共享记忆体。

### 部署 Sails 丛集到多台服务器

+ 在负载平衡器之后部署多个实例（又称服务器执行应用程序的副本）
  + 在每个实例使用 `forever` 启动 Sails
  + 更多关于负载平衡器的资讯：http://en.wikipedia.org/wiki/Load_balancing_(computing)
+ 设置负载平衡器终止 SSL 请求
  + 因为传输已经被解密，你不需要在 Sails 使用 SSL 设置


<docmeta name="uniqueID" value="Scaling291270">
<docmeta name="displayName" value="Scaling">

