# 设置（Configuration）

### 概述

虽然 Sails 尽责的坚守[约定优于配置](http://en.wikipedia.org/wiki/Convention_over_configuration)的理念，但了解如何自定义这些方便的默认值是很重要的。对于几乎每个 Sails 的约定，允许你调整或覆盖附带的设置选项，以满足你的需求。本章节的文件完整包含了 Sails 可用的设置选项。

Sails 应用程序可以通过[程序设置](https://github.com/mikermcneil/sails-generate-new-but-like-express/blob/master/templates/app.js#L15)，通过指定[环境变数](http://en.wikipedia.org/wiki/Environment_variable)或命令行参数，通过改变区域或全局 [`.sailsrc` 文档](http://beta.sailsjs.org/#/documentation/anatomy/myApp/sailsrc.html)，或（最常见）使用约定位于工程内 [`config/`](http://beta.sailsjs.org/#/documentation/anatomy/myApp/config) 文件夹的模板设置文档。执行时期可通过 `sails` 全局变数的 `sails.config` 在应用程序使用合并后的设置。


### 标准设置文档 (`config/*`)

在默认情况下，新的 Sails 应用程序包含许多的设置文档。这些模板文档包含了一些行内注解，目的是为了提供一个快速、即时的参考，而不必来回跳转于文件与文字编辑器之间。

在多数情况下，`sails.config` 对象的顶层键（例如 `sails.config.views`）对应在应用程序内特定的设置文档（例如 `config/views.js`）；而设置可以安排在 `config/` 目录内任何你喜欢的文档中。重要的部分是设置的名称（即键），不是它从哪个文档来。

举例来说，假设你新增一个新文档，`config/foo.js`：

```js
// config/foo.js
// 对象会被合并到 `sails.config.blueprints`：
module.exports.blueprints = {
  shortcuts: false
};
```

对于个别设置项目的详细参考资料，默认存在于该设置文档中，请参考本章节内的参考资料页面，或查看[Sails 应用程序剖析](./#!documentation/anatomy)的[「`config/`」](http://beta.sailsjs.org/#/documentation/anatomy/myApp/config)取得更多的说明。





### 在你的应用程序存取 `sails.config`

`config` 对象存在于 Sails 应用程序实例（`sails`）。默认情况下，在启动时会置于[全局范围](http://beta.sailsjs.org/#/documentation/concepts/Globals)，因此存在于应用程序的任何地方。

##### 例子
```javascript
// 这个例子检查在生产环境时 csrf 必需启动。
// 否则，抛出错误并终止应用程序。
if (sails.config.environment === 'production' && !sails.config.csrf) {
  throw new Error('STOP IMMEDIATELY ! CSRF should always be enabled in a production deployment!');
}
```



### 自定义组件设置
Sails 能辨认顶层键下的许多不同设置、命名空间（如 `sails.config.sockets` 和 `sails.config.blueprints`）。但你也可以在你的自定义组件设置使用 `sails.config`（如`sails.config.someProprietaryAPI.secret`）。

##### 例子

```javascript
// config/linkedin.js
module.exports.linkedin = {
  apiKey: '...',
  apiSecret: '...'
};
```

```javascript
// 在你的 controller/service/model/hook/whatever:
// ...
var apiKey = sails.config.linkedin.apiKey;
var apiSecret = sails.config.linkedin.apiSecret;
// ...
```




### 设置 `sails` 命令行界面

当谈到设置，大部分时间你会专注于管理特定应用程序的执行时期设置：连接埠、资料库连线，等等。然而，为了简化你的工作流程，减少重复性任务，执行自定义的自动化建置等，自定义 Sails 命令行界面也是很有用的。值得庆幸的是，Sails v0.11 增加了强大的新工具来做到这一点。

[`.sailsrc` 文档](http://beta.sailsjs.org/#/documentation/anatomy/myApp/sailsrc.html)与其他在 Sails 中的设置文档不同，它也可以被用于设置 Sails 命令行界面－无论是全系统、目录群组或仅当你 `cd` 到特定文件夹。这样做的主要理由是自定义用于执行 `sails generate` 和 `sails new` 的[产生器](http://beta.sailsjs.org/#/documentation/concepts/extending-sai