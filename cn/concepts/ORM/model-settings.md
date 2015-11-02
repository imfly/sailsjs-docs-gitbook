# 模型设置（Model Settings）

以下的属性可以指定在你的模型定义的上层，来覆写该模型的默认值。修改 [`config/models.js`](https://github.com/balderdashy/sails-docs/blob/master/PAGE_NEEDED.md) 来覆写所有模型共享的默认设置。






### `migrate`

```javascript
migrate: 'safe'
```

总之，此设置控制了 Sails 是否／如何尝试在你的结构自动重建 tables/collections/sets 等。

在生产环境中（NODE_ENV === "production"）Sails 总是使用 `migrate:"safe"` 来保护意外删除你的资料。然而在开发过程中，你有其他几个方便的选项：

 1. safe  - 永远不要自动迁移我的资料库。我会自己去做（手动）
 2. alter - 自动迁移，但尝试保留现有资料（实验性）
 3. drop  - 每次启动 Sails 时清除／删除所有资料并重建模型

当你启动 sails 应用程序时，waterline 会验证你的资料库的所有资料。这个标记告诉 waterline 资料毁损时该如何处理资料。你可以设置这个标记为 `safe`，将忽略毁损的资料并继续启动。你还可以将其设置为


| 自动迁移策略  | 说明 |
|-------------|----------------------------------------------|
|`safe`       | 永远不要自动迁移我的资料库。我会自己手动去做
|`alter`      | 自动迁移，但尝试保留现有资料（实验性）
|`drop`       | 每次启动 Sails 时清除／删除所有资料并重建模型


> 请注意，使用 `drop` 或 `alter` 可能失去你的资料。当心，永远不要在生产环境使用 `drop` 或 `alter`。



### `schema`

```javascript
schema: true
```

在支持无结构（Schemaless）资料结构资料库切换无结构（Schemaless）或结构（Schema）模式的标记。如果关闭，将允许你储存任意资料的记录。如果开启，只有定义在模型的 `attributes` 属性对象会被储存。

对于不需要结构的桥接器，如 Mongo 或 Redis，默认设置是 `schema:false`。



### `connection`

```javascript
connection: 'my-local-postgresql'
```

此模型将从已设置的资料库[连线](http://sailsjs.org/#/documentation/reference/sails.config/sails.config.connections.html)取得和储存资料。默认为 `localDiskDb`，默认的连线使用 `sails-disk` 桥接器。


### `identity`

```javascript
identity: 'purchase'
```

此模型的小写唯一键（Unique key），例如 `user`。默认情况下，会自动从它的文档名称自动推测模型的 `identity`。你永远不应该在模型改变这个属性。

### `globalId`

```javascript
globalId: 'Purchase'
```

这个标记变更了你可以存取模型的全局名称（如果启用了模型的全局化）。你永远不应该在模型改变这个属性。要停用全局，请参考 [`sails.config.globals`](http://sailsjs.org/#/documentation/concepts/Globals?q=disabling-globals)。



### `autoPK`

```javascript
autoPK: true
```

切换模型中自动定义主键的标记。此默认 PK 的细节依桥接器而有所不同（例如 MySQL 使用一个自动递增的整数主键，而 MongoDB 使用乱数字串 UUID）。在任何情况下，由 autoPK 产生的主键是唯一的。如果关闭，默认将不会建立主键，你将需要手动定义一个，例如：

```js
attributes: {
  sku: {
    type: 'string',
    primaryKey: true,
    unique: true
  }
}
```

### `autoCreatedAt`

```javascript
autoCreatedAt: true
```

切换模型中自动定义 `createdAt` 属性的标记。默认情况下，当记录建立时 `createdAt` 属性会自动设置为目前时间戳记，例如：

```js
attributes: {
  createdAt: {
    type: 'datetime',
    defaultsTo: function (){ return new Date(); }
  }
}
```

### `autoUpdatedAt`

```javascript
autoUpdatedAt: true
```
切换模型中自动定义 `updatedAt` 属性的标记。默认情况下，当记录被更新时 `updatedAt` 属性会自动设置为目前时间戳记，例如：

```js
attributes: {
  updatedAt: {
    type: 'datetime',
    defaultsTo: function (){ return new Date(); }
  }
}
```


### `tableName`

```java