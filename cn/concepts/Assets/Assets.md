# 资源（Assets）

### 概述

资源指的是在你的服务器上想让外界存取的[静态文档](http://en.wikipedia.org/wiki/Static_web_page)（js、css、图档等等）。在 Sails，这些文档都放在 [`assets/`](http://beta.sailsjs.org/#/documentation/anatomy/myApp/assets) 目录，当你启动应用程序，他们会被处理并同步到一个隐藏的暂存目录(`.tmp/public/`)。这个 `.tmp/public` 文件夹就是 Sails 实际提供的内容，大致等同于 [express](https://github.com/expressjs) 的「public」文件夹，或是其他你或许熟悉的网站服务器如 Apache 的「www」文件夹。这中间的过程允许 Sails 准备或预先编译在用户端上使用的资源，像是 LESS、CoffeeScript、SASS、spritesheets、Jade 模板等等。

### 静态中间件（Static middleware）

在幕后，Sails 使用 Express 的[静态中间件](http://www.senchalabs.org/connect/static.html)来提供你的资源。你可以在 [`/config/http.js`](/#/documentation/reference/sails.config/sails.config.http.html) 设置这个中间件（例如 cache 设置）。

##### `index.html`
如同大多数网页服务器，Sails 实践了 `index.html` 约定。举例来说，如果你在新的 Sails 工程建立 `assets/foo.html`，便可通过 `http://localhost:1337/foo.html` 存取。但是，如果你建立 `assets/foo/index.html`，则可通过 `http://localhost:1337/foo/index.html` 及 `http://localhost:1337/foo` 存取。

##### 优先权
重要的是需注意静态[中间件](http://stephensugden.com/middleware_guide/)是安装在 Sails 路由**之后**。所以，如果你定义了一个[自定义路由](/#/documentation/concepts/Routes?q=custom-routes)，但在你的资源目录也有文档与该路径冲突，自定义路由会在到达静态中间件前拦截请求。举例来说，如果你建立 `assets/index.html` 且没有定义路由在 [`config/routes.js`](/#/documentation/reference/sails.config/sails.config.routes.html) 文档，它会被当成你的首页。但是如果你定义一个自定义路由 `'/': 'FooController.bar'`，将优先采用此路由。


<docmeta name="uniqueID" value="Assets220313">
<docmeta name="displayName" value="Assets">

