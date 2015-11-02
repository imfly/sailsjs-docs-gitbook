# 全局变数（Globals）
### 概述

为了方便起见，Sails 公开了一些全局变数。默认情况下，应用程序的[模型](http://beta.sailsjs.org/#/documentation/reference/Models)、[服务](http://beta.sailsjs.org/#/documentation/reference/Services)，和全局 `sails` 对象都存在于全局范围；这代表你可以在后端程序码的任何地方通过名称参考使用它们（只要 Sails [已经载入](https://github.com/balderdashy/sails/tree/master/lib/app)）。

在 Sails 核心没有什么东西是依赖于这些全局变数，每个公开的全局变数也可以在 `sails.config.globals` 内禁用（通常设置在 `config/globals.js`）。


### 应用程序对象（`sails`）
在大多数情况下，你会想保留 `sails` 对象的全局存取，它使你的程序码更加干净。但是，如果你_确实_需要禁用_所有_全局变数，包含 `sails`，你可以从请求对象（`req`）存取 `sails`。

### 模型和服务
应用程序的[模型](http://beta.sailsjs.org/#/documentation/reference/Models)和[服务](http://beta.sailsjs.org/#/documentation/reference/Services)被通过它们的 `globalId` 公开为全局变数。例如，定义在 `api/models/Foo.js` 文档的模型可以通过 `Foo` 在全局存取，而定义在 `api/services/Baz.js` 的服务则可通过 `Baz` 存取。

### Async（`async`）和 Lodash（`_`）
Sails 也公开 `_` 为 [lodash](http://lodash.com) 的实例，以及 `async` 为 [async](https://github.com/caolan/async) 的实例。默认已提供这些常用的套件，这样你就不必在每个新工程 `npm install` 它们。如同 sails 的其他全局变数，它们可以被禁用。

### 禁用全局变数

Sails 通过检查 [`sails.config.globals`](http://beta.sailsjs.org/#/documentation/reference/sails.config/sails.config.globals.html) 来决定要公开哪个全局变数，通常设置在 [`config/globals.js`](http://beta.sailsjs.org/#/documentation/anatomy/myApp/config/globals.js.html)。

要禁用所有全局变数，只需将组件设置为 `false`：

```js
// config/globals.js
module.exports.globals = false;
```

要禁用_一些_全局变数，指定一个对象来代替，例如：

```js
// config/globals.js
module.exports.globals = {
  _: false,
  async: false,
  models: false,
  services: false
};
```

### 注意事项

> + 请记住，在 sails 被载_入前_，没有一个全局变数，包含 `sails`，可以被存取。换句话说，你不能使用 `sails.models.user` 或 `User` 功能（因为 `sails` 还没载入完成。）

<!-- not true anymore:
Most of this section of the docs focuses on the methods and properties of `sails`, the singleton object representing your app.  
-->

<docmeta name="uniqueID" value="Globals668238">
<docmeta name="displayName" value="Globals">

