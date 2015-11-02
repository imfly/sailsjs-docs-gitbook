# sails.log()
### 概述

下列方法接受以逗点分隔的参数，没有数量与资料型态限制。如同 `console.log`，作为参数传入 Sails 日志记录器的资料会使用 Node 的 [`util.inspect()`](http://nodejs.org/api/util.html#util_util_inspect_object_options) 自动美化，以方便阅读。因此，适用于标准 Node.js 约定，也就是说，如果你使用 `inspect()` 方法记录一个对象，它会自动执行并返回将被写入到终端机的字串。相同的，对象、日期、阵列和大多数其它资料型态会使用 `util.inspect()` 内建的逻辑来美化（例如，你会看到 `{ pet: { name: 'Hamlet' } }` 而不是 `[object Object]`。）



### `sails.log()`

默认的日志功能，会将「debug」等级的日志输出到 `stderr`。

```js
sails.log('hello');
// -> debug: hello.
```

### `sails.log.error()`

将「error」等级的日志输出到 `stderr`。

```js
sails.log.error('Unexpected error occurred.');
// -> error: Unexpected error occurred.
```

### `sails.log.warn()`

将「warn」等级的日志输出到 `stderr`。

```js
sails.log.warn('File upload quota exceeded for user','request aborted.');
// -> warn: File upload quota exceeded for user- request aborted.
```

### `sails.log.debug()`
_`sails.log()` 的别名_

### `sails.log.info()`

将「info」等级的日志输出到 `stderr`。

```js
sails.log.info('A new user (', 'mike@foobar.com', ') just signed up!');
// -> info: A new user ( mike@foobar.com ) just signed up!
```


### `sails.log.verbose()`

将「verbose」等级的日志输出到 `stderr`。
可用于截取应用程序的详细资讯，你可能只会在少数情况下使用。

```js
sails.log.verbose('A user initiated an account transfer...')
// -> verbose: A user initiated an account transfer...
```


### `sails.log.silly()`

将「silly」等级的日志输出到 `stderr`。
可用于截取应用程序的完整资讯，你可能只会在少数情况下使用。

```js
sails.log.silly('A user probably clicked on something..?');
// -> silly: A user probably clicked on something..?
```





<docmeta name="uniqueID" value="sailslog321347">
<docmeta name="displayName" value="sails.log()">

