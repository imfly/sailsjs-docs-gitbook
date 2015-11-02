# 默认任务（Default Tasks）

### 概述

Sails 内的 asset pipeline 是一组能增加工程一致性和效率的 Grunt 任务设置。整个前端资源工作流程可完全自定义，它提供了一些可立即使用的默认任务。Sails 可以很容易的[设置新任务](/#/documentation/concepts/Assets/TaskAutomation.html?q=task-configuration)，以满足你的需求。

这些 Sails 默认的 Grunt 组件设置可协助你：
- 自动编译 LESS
- 自动编译 JST
- 自动编译 Coffeescript
- 自定义的资源自动注入、压缩及合并
- 建立网站公用目录
- 监视和同步文档
- 优化生产环境的资源

### 默认 Grunt 任务行为

以下是包含在 Sails 工程的 Grunt 任务及每个任务的简短说明。此外，还包含了每个任务的使用说明连结。

##### clean

> 这个 grunt 任务是用来清理 sails 工程里 `.tmp/public/` 的内容。

> [使用说明](https://github.com/gruntjs/grunt-contrib-clean)

##### coffee

> 从 `assest/js/` 将 coffeeScript 文档编译成 Javascript 并放到 `.tmp/public/js/` 目录。

> [使用说明](https://github.com/gruntjs/grunt-contrib-coffee)

##### concat

> 合并 javascript 和 css 并将合并后的文档放到 `.tmp/public/concat/` 目录。

> [使用说明](https://github.com/gruntjs/grunt-contrib-concat)

##### copy

> **dev 任务设置**
> 从 sails 资源文件夹复制 coffeescript 和 less 以外的所有目录与文档到 `.tmp/public/` 目录。

> **build 任务设置**
> 从 `.tmp/public/` 目录复制所有目录及文档到 www 目录。

> [使用说明](https://github.com/gruntjs/grunt-contrib-copy)

##### cssmin

> 压缩 css 文档并放到 `.tmp/public/min/` 目录。

> [使用说明](https://github.com/gruntjs/grunt-contrib-cssmin)

##### jst

> 预先将 Underscore 模板编译成 `.jst` 文档。（也就是说，它需要模板文档并将其转换成微小的 javascript 函数）。这可以加速在用户端的模板呈现，及减少频宽的消耗。

> [使用说明](https://github.com/gruntjs/grunt-contrib-jst)

##### less

> 将 LESS 文档编译成 CSS。只有 `assets/styles/importer.less` 会被编译。这让你可以自行控制顺序，即在其他样式之前汇入你的相依（Dependencies）、混入（Mixins）、变数（Variables）、重置（Resets）等等。

> [使用说明](https://github.com/gruntjs/grunt-contrib-less)

##### sails-linker

> 自动为 javascript 文档注入 `<script>` 标签以及为 css 文档注入 `<link>` 标签。还可以自动连接输出文档到使用 `<script>` 标签的预先编译模板。这个任务的详细说明可以在[这里](https://github.com/balderdashy/sails-generate-frontend/blob/master/docs/overview.md#a-litte-bit-more-about-sails-linking)找到，但最大的改变是*只有*当文档包含 `<!--SCRIPTS--><!--SCRIPTS END-->` 和/或 `<!--STYLES--><!--STYLES END-->` 才会做 script 和 stylesheet 注入。这些都包含在新 Sails 工程默认的 **views/layout.ejs** 文档。如果不想在工程使用连接器，只需删除这些标签。

> [使用说明](https://github.com/Zolmeister/grunt-sails-linker)

##### sync

> 保持目录同步的 grunt 任务。它与 grunt-contrib-copy 非常类似，但仅会尝试复制那些真正有改变的文档。它明确的从 `assets/` 文件夹同步文档到 `.tmp/public/`，并覆盖任何已存在的文档。

> [使用说明](https://github.com/tomusdrw/grunt-sync)

##### uglify

> 压缩用户端 javascript 资源。

> [使用说明](https://github.com/gruntjs/grunt-contrib-uglify)

##### watch

> 当被监视的文档类型被新增、修改或删除，执行预先定义的任务。监视 `assets/` 文件夹的文档异动，并重新执行对应的任务（例如编译 less 和 jst）。这让你可以看到应用程序的资源变更，而无需重新启动 Sails 服务器。

> [使用说明](https://github.com/gruntjs/grunt-contrib-watch)

<docmeta name="uniqueID" value="DefaultTasks764297">
<docmeta name="displayName" value="Default Tasks">

