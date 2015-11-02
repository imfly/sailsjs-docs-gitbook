# 任务自动化（Task Automation）

### 概述

[`tasks/`](./#!documentation/anatomy/tasks) 目录包含了一系列 [Grunt 任务](http://gruntjs.com/creating-tasks)和它们的[组件设置](http://gruntjs.com/configuring-tasks)。

任务主要是用在打包前端资源（如 stylesheets、scripts 及用户端标记模板），但它们也可以用在自动化重复各种开发时的琐事，从 [browserify](https://github.com/jmreidy/grunt-browserify) 编译到[资料库迁移](https://www.npmjs.org/package/grunt-db-migrate)皆可使用。

为了方便起见，Sails 打包了一些[默认任务](./#!documentation/grunt/default-tasks)，但随著[数以百计](http://gruntjs.com/plugins)的插件可供选择，你可以几乎毫不费力的使用任务自动完成任何事情。如果没有你需要的，你可以[撰写](http://gruntjs.com/creating-tasks)并[发布自己的 Grunt 插件](http://gruntjs.com/creating-plugins)到 [npm](http://npmjs.org)！

> 如果你以前从未使用过 [Grunt](http://gruntjs.com/)，一定要查看[新手上路](http://gruntjs.com/getting-started)指南，因为它解释了如何建立 [Gruntfile](http://gruntjs.com/sample-gruntfile) 以及安装和使用 Grunt 插件。


### Asset pipeline

Asset pipeline 是让你组织要注入到检视的资源的地方，可以在 `tasks/pipeline.js` 文档找到它。设置这些资源很简单，使用 grunt [任务文档组件设置](http://gruntjs.com/configuring-tasks#files)和[匹配模式](http://gruntjs.com/configuring-tasks#globbing-patterns)。它们被分为三个部分。

##### 要注入的 CSS 文档
这是一个 css 文档阵列，会注入到 html 的 `<link>` 标签。这些标签会放在所有检视的 `<!--STYLES--><!--STYLES END-->` 注解之间。

##### 要注入的 Javascript 文档
这是一个 Javascript 文档阵列，会注入到 html 的 `<script>` 标签。这些标签会放在所有检视的 `<!--SCRIPTS--><!--SCRIPTS END-->` 注解之间。文档会依照在阵列中的顺序被注入（也就是你应该按照文档相依关系来调整注入的顺序）。

##### 要注入的模板文档
这是一个 html 文档阵列，会编译成 jst 函数并放在一个 jst.js 文档。这些文档会注入到 `<script>` 标签，放在 html 的 `<!--TEMPLATES--><!--TEMPLATES END-->` 注解之间。

> 如果你想改变它们的话，相同的 grunt 匹配模式和任务文档组件设置也使用在一些任务组件设置文档自身。

### 任务组件设置（Task configuration）

每个已设置的任务都是一组规则，Gruntfile 会遵循此规则执行。他们位于 [`tasks/config/`](/#/documentation/anatomy/myApp/tasks/config) 目录且可完全自定义。你可以修改、忽略或取代任何一个 Grunt 任务，以满足你的需求。你也可以加入自己的 Grunt 任务，只需在此目录新增一个 `someTask.js` 文档来设置新的任务，然后用适当的父任务注册它（请查看 `grunt/register/*.js` 内的文档）。请记住，Sails 具备一套实用的默认任务，是为了让你在无需任何组件设置下执行。

##### 设置自定义任务

设置一个自定义任务到你的工程非常简单，使用 Grunt 的[设置](http://gruntjs.com/api/grunt.config)和[任务](http://gruntjs.com/api/grunt.task) APIs 允许你建立自己的任务模组。让我们实践一个建立新任务取代已存在任务的简单例子。比方说，我们希望 [Handlebars](http://handlebarsjs.com/) 模板引擎来取代默认具备的 underscore 模板引擎：

* 第一步是在终端机使用以下指令安装 handlebars 的 grunt 插件：

```bash
npm install grunt-contrib-handlebars --save-dev
```

* 建立组件设置文档在 `tasks/config/handlebars.js`。这是我们要放 handlebars 设置的地方。

```javascript
// tasks/config/handlebars.js
// --------------------------------
// handlebar 任务组件设置。

module.exports = function(grunt) {

  // 我们使用 grunt.config api 的 set 方法来设置一个对象到定义的字串。
  // 在这个例子中，'handlebars' 任