# 日志（Logging）

### 概述
Sails 内建一个名为 [`captains-log`](https://github.com/balderdashy/captains-log) 的简单日志记录器。它的用法与 Node 的 [`console.log`](http://nodejs.org/api/stdio.html) 非常类似，但有一些额外的功能；即支持在终端机输出多种含前缀字和颜色的日志等级。

### 组件设置
Sails 日志记录器的设置在 [`sails.config.log`](http://beta.sailsjs.org/#/documentation/reference/sails.config/sails.config.log.html)，照约定默认对应 Sails 工程的设置文档（[`config/log.js`](http://beta.sailsjs.org/#/documentation/anatomy/myApp/config/log.js.html)）。

当设置了一个日志输出等级，Sails 会在相同或高于目前设置等级时输出日志讯息。这个日志等级已标准化，且适用于从 socket.io、Waterline 及其它相依功能产生输出。日志等级和相应的优先权分级结构总结为以下图表：

| 优先权 | 等级      | 可见的日志          |
|-------|-----------|-------------------|
| 0     | silent    | 无 |
| 1     | error     | `.error()` |
| 2     | warn      | `.warn()`, `.error()` |
| 3     | debug     | `.debug()`, `.warn()`, `.error()` |
| 4     | info      | `.info()`, `.debug()`, `.warn()`, `.error()` |
| 5     | verbose   | `.verbose()`, `.info()`, `.debug()`, `.warn()`, `.error()` |
| 6     | silly     | `.silly()`, `.verbose()`, `.info()`, `.debug()`, `.warn()`, `.error()` |


#### 注意事项
+ 默认的日志等级是「info」。当你设置应用程序的日志等级为「info」，Sails 会记录关于服务器／应用程序状态的有限资讯。
+ 当日志等级设置为「silly」，Sails 会记录已被绑定的路由、其它详细的框架生命周期资讯、诊断和实践细节等内部资讯。
+ 当日志等级设置为「verbose」，Sails 会记录 Grunt 的输出，以及更详细的路由、模型、挂勾（hook）等被载入的资讯。


<docmeta name="uniqueID" value="Logging277763">
<docmeta name="displayName" value="Logging">

