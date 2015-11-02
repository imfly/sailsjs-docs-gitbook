# 自定义回应（Custom Responses）

### 概述

Sails v.10 允许自定义服务器回应。Sails 默认附带一些常见的回应类型。可以在工程的 `/api/responses` 目录找到它们。只需编辑对应的 .js 文档，就可以自定义。

作为一个简单的例子，思考以下的控制器动作：

```
foo: function(req, res) {
   if (!req.param('id')) {
     res.status(400);
     res.view('400', {message: 'Sorry, you need to tell us the ID of the FOO you want!'});
   }
   ...
}
```

这个程序码通过发送一个 400 错误状态及简短问题描述来处理错误请求。然而，这个程序码有几个缺点，主要是：

* 它不是*标准化*的：该代码是特定于此情况，我们必须在任何地方努力保持相同的格式
* 它不是*被分离*的：当我们想要在其他地方使用类似的方法，就需要复制／贴上程序码
* 它不是*内容协商*的：如果用户端期待一个 JSON 回应，那别指望了

现在，思考一下这个修改：

```
foo: function(req, res) {
   if (!req.param('id')) {
     res.badRequest('Sorry, you need to tell us the ID of the FOO you want!');
   }
   ...
}
```


这种方法具有许多优点：

 - 错误被标准化
 - 有考虑到生产环境与开发环境的日志记录
 - 错误代码是一致的
 - 有考虑到内容协商（JSON 与 HTML）
 - 可在适当的共用回应文档快速的调整 API

### 回应方法和文档（Responses methods and files）

任何储存在 `/api/responses` 文件夹的 `.js` 脚本可通过在控制器内呼叫 `res.[responseName]` 来执行。例如，可以通过呼叫 `res.serverError(errors)` 来执行 `/api/responses/serverError.js`。在回应脚本内可以通过 `this.req` 和 `this.res` 取得请求及回应对象；这让实际的回应方法可以取得任意参数。（如 `serverError` 的 `errors` 参数）。

### 默认回应

以下的回应已绑定在所有新的 Sails 应用程序的 `/api/responses` 文件夹内。当用户端期望收到 JSON，会回应一个包含了 HTTP 状态代码的 `status` 键及任何错误相关资讯的标准化 JSON 对象。

#### res.serverError(errors)

这个回应会将错误标准化为适当、可读取的 `Error` 对象。 `errors` 可以是一个或多个字串或 `Error` 对象。它会记录所有错误到 Sails 记录器（通常是终端机），并当用户端期望收到 HTML 时回应 `views/500.*` 检视文档，或当用户端期望收到 JSON 时回应一个 JSON 对象。在开发模式下，错误清单会包含在回应中。在生产环境下，实际的错误会受到抑制。

#### res.badRequest(validationErrors, redirectTo)

对于期望收到 JSON 的请求者，这个回应包含了 400 状态码及被作为 `validationErrors` 所传送的任何相关资料。

对于传统的（非 AJAX）网页表单，当使用者提交无效的表单资料，这个中间件遵循了最佳做法：

 - 首先，一个暂存变数可能被填入了一个字串或语义验证错误对象。
 - 然后，将使用者重新导向回 `redirectTo`，即发出错误请求的来源 URL。
 - 还有，控制器和／或检视可能使用暂存变数 `errors` 来显示讯息或突显无效的 HTML 表单栏位。


#### res.notFound()

当请求者期望收到 JSON，这个回应会发送 404 状态码及一个 `{status: 404}` 对象。

否则，将发送位于 `myApp/views/404.*` 内的检视。若找不到检视，那么便发送 JSON 回应。

#### res.forbidden(message)

当请求者期望收到 JSON，这个回应会发送 403 状态码及 `message` 的内容。

否则，将发送位于 `myApp/views/403.*` 内的检视。若找不到检视，那么便发送 JSON 回应。

### 自定义回应

要加入你自己的自定义回应方法，只需新增与方法名称相同的文档到 `/api/responses`。该文档应该导出函数，可以附带任何你喜欢的参数。

```
/** 
 * api/responses/myResponse.js
 *
 * This will be available in controllers as res.myResponse('foo');
 */

module.exports = function(message) {
   
  var 