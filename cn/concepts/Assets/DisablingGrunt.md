# 禁用 Grunt（Disabling Grunt）

要禁用整合在 Sails 的 Grunt，只需删除 Gruntfile（和/或 [`tasks/`](/#/documentation/anatomy/myApp/tasks) 文件夹）。你还可以禁用 Grunt hook。只要像这样在 `.sailsrc` hooks 设置 `grunt` 属性为 `false`：

```json
{
    "hooks": {
        "grunt": false
    }
}
```

### 我可以为 SASS、Angular、用户端 Jade 模板等自定义任务吗？

是的！只需取代 `tasks/` 目录中对应的 grunt 任务，或新增一个。如同 [SASS](https://github.com/sails101/using-sass) 例子。

如果你仍然想使用 Grunt 做其他用途，但不想要任何默认的网页前端工作，只要删除工程的资源文件夹并从 `grunt/register/` 和 `grunt/config/` 文件夹移除前端相关任务。你还可以在往后的工程执行 `sails new myCoolApi --no-frontend` 来省略资源文件夹和前端相关 Grunt 任务。你也可以用社区的产生器或[建立自己的](https://github.com/balderdashy/sails-generate-generator)产生器来取代 `sails-generate-frontend` 模组。这让 `sails new` 可以建立原生 iOS 应用程序、Android 应用程序、Cordova 应用程序、SteroidsJS 应用程序等等的模板。


<docmeta name="uniqueID" value="DisablingGrunt970874">
<docmeta name="displayName" value="Disabling Grunt">

