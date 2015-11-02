# 部署（Deployment）

### 概述

#### 在部署之前

在你启动任何网页应用程序前，你应该问自己几个问题：

+ 你预期的流量为何？
+ 你的合约是否要求满足任何正常执行时间保证，如服务层级协议（SLA）？
+ 哪种前端应用程序会触及你的网页应用程序？
  + Android 应用程序
  + iOS 应用程序
  + 桌面版网页浏览器
  + 行动版网页浏览器（平板电脑、电话、iPad mini？）
  + 电视、手表、烤面包机…？
+ 以及它们会要求什么东西？
  + JSON？
  + HTML？
  + XML？
+ 你会利用 Socket.io 的即时发布订阅功能？
  + 例如聊天、即时分析、应用程序内通知／讯息
+ 你是如何追踪崩溃与错误？
  + 看看 Sails 的日志设置



#### 部署在单一服务器

Node.js 非常快速。对于许多应用程序，在一开始一台服务器就足够处理预期的流量。

##### 设置

+ 所有生产环境设置都储存在 `config/env/production.js`
+ 设置应用程序执行于连接埠 80（如果不是在如 nginx 之类的代理之后）。如果你使用的是 nginx，一定要对其设置中继 WebSocket 到应用程序。你可以在 nginx 文件 [WebSocket proxying](http://nginx.org/en/docs/http/websocket.html) 找到指南。
+ 设置「正式」环境，让所有的 css/js 被打包，且内部服务器被切换到适当的环境（需要[连接器](https://github.com/balderdashy/sails-wiki/blob/0.9/assets.md)）。
+ 务必确认资料库已设置在正式服务器。更重要的一点是，如果你使用的是关联式资料库如 MySQL，当执行于生产环境时， Sails 会设置所有的模型为 `migrate:safe`，这代表启动应用程序时不会进行自动移转。你可以用以下方法设置资料库：
  + 在服务器上建立资料库，使用正式服务器作为资料库，然后在本地使用 `migrate:alter` 设置执行 Sails 应用程序。这样就自动设置好了。
  +  如果你无法远端连线服务器，你可以倒出在本地端的结构，并将其汇入到资料库服务器。
+ 启用 CSRF 来保护 POST、PUT 及 DELETE 请求
+ 启用 SSL
+ 如果你使用 SOCKETS：
  + 设置 `config/sockets.js` 并使用 socket.io 的[生产环境建议设置](https://github.com/LearnBoost/Socket.IO/wiki/Configuring-Socket.IO#recommended-production-settings)
    + 例如启用 `flashsocket` 传输

##### 部署

在生产环境中，你会想要使用 forever 或 PM2 来取代 `sails lift`，以确保即使应用程序崩溃了也会继续运作。

+ 安装 forever：`sudo npm install -g forever`
  + 更多关于 forever 的资讯：https://github.com/nodejitsu/forever
+ 或安装 PM2：`sudo npm install pm2 -g --unsafe-perm`
  + 更多关于 PM2 的资讯：https://github.com/Unitech/pm2 
+ 从你的应用程序目录，使用 `forever start app.js --prod` 或 `pm2 start app.js -x -- --prod` 启动服务器
  + 这和 `sails lift --prod` 所做的事相同，但是当服务器崩溃时，它会自动重新启动。
 


<docmeta name="uniqueID" value="Deployment402941">
<docmeta name="displayName" value="Deployment">

