# 国际化（Internationalization）

### 概述

如果你的应用程序会触及从世界各地而来的人或系统，国际化（i18n）和本地化（l10n）会是你国际化策略重要的一部分。Sails 提供内建支持，用于侦测用户语言偏好设置和翻译静态单字／句子，这要归功于 [i18n-node](https://github.com/mashpie/i18n-node)（[npm](https://www.npmjs.org/package/i18n)）。



<!--
  Potentially cover this:
  *(but it might be obvious and not useful/necessary to include, not sure- could also be more confusing than helpful)*
Note that this built-in support is for **dynamically-rendered** (but otherwise **static**) content.  You can only use it in responses which are pre-processed on the server.  In other words, you can use these translations in your views, controller actions, and policies, but stuff in your assets folder.)

we do not recommend translating strings in the front-end of your application (e.g. the browser or an iOS app) for a variety of reasons, the most obvious being SEO, but also fragmentation. You can of course still do so- just don't use this built-in support from the i18n hook.
-->


### 用法


在检视内：
```ejs
<h1> <%= __('Hello') %> </h1>
<h1> <%= __('Hello %s, how are you today?', 'Mike') %> </h1>
<p> <%= i18n('That\'s right-- you can use either i18n() or __()') %> </p>
```


在控制器或政策内：
```javascript
req.__('Hello'); // => Hola
req.__('Hello %s', 'Marcus'); // => Hola Marcus
req.__('Hello {{name}}', { name: 'Marcus' }); // => Hola Marcus
```


或者，你已经知道语系 ID，你可以在应用程序内的任何地方使用 `sails.__` 翻译：

```javascript
sails.__({
  phrase: 'Hello',
  locale: 'es'
});
// => 'Hola!'
```



### 语系
i18n 挂勾（hook）会从工程的「locales」目录（默认是 `config/locales`）读取 JSON 格式翻译文档。每个文档对应一个 Sails 后端所支持的[语系](http://en.wikipedia.org/wiki/Locale)（通常是语言）。

这些文档包含特定的语系字串（为 JSON 键值对），你可以使用在检视、控制器等地方。

这里有一个语系例子文档（`config/locales/es.json`）：
```json
{
    "Hello!": "Hola!",
    "Hello %s, how are you today?": "¿Hola %s, como estas?",
}
```

请注意，语系档内的键（例如 "Hello %s, how are you today?"）有**区分大小写**且需要精准匹配。这里有几个不同思想流派的最佳翻译，要选择哪个翻译取决于未来最常会由谁编辑语系档与 HTML。特别是如果你会手动编辑，将键的名称全部小写会最提供最佳的可维护性。

例如，这里有另一个翻译在 `config/locales/es.json`：

```json
{
    "hello": "Hola!",
    "hello-how-are-you-today": "Hola %s, ¿cómo estás?",
}
```

以及这里 `config/locales/en.json`：

```json
{
    "hello": "Hello!",
    "hello-how-are-you-today": "Hello, how are you today?",
}
```


### 侦测和／或覆写请求的所需语系

使用新的语系代码呼叫 [`req.setLocale()`](https://github.com/mashpie/i18n-node#setlocale) 来覆写请求的自动侦测语言／本地化偏好设置：

```js
// 强制让请求使用德文：
req.setLocale('de');
//（这会使用在 `config/locales/de.json` 的字串来翻译）
```

默认情况下，node-i18n 会通过检查请求的 Language 标头来侦测所需的语言。Language 标头是设置在用户的浏览器，且它们大多是正确的，你可能需要灵活覆写所侦测到的语系并提供翻译。

例如，如果你的应用程序允许使用者选择偏好语言，你可能会建立一个[政策](http://beta.sailsjs.org/#/documentation/concepts/Policies)用来检查使用者会话（Session）内的自定义语言，如果存在的话，设置相应语系以便在后续的政策、控制器动作和检视使用：

```js
// api/policies/localize.js
module.exports = function(req, res, next) {
  req.setLocale(req.session.languagePreference);
  next();
};
```


<!--

  Alternatively, here's another extended example:
  (todo: at the very least pull this into a sep