# 生命周期回呼（Lifecycle callbacks）

### 概述

Sails 公开了一些模型的生命周期回呼，在做某些动作之前或之后会自动被呼叫。例如，我们有时候会使用生命周期回呼在建立或更新帐号模型前自动加密密码。另一个使用情况是当工程的 `name` 属性更新时自动重新产生网址。

##### `create` 的回呼

  - beforeValidate: fn(values, cb)
  - afterValidate: fn(values, cb)
  - beforeCreate: fn(values, cb)
  - afterCreate: fn(newlyInsertedRecord, cb)

##### `update` 的回呼

  - beforeValidate: fn(valuesToUpdate, cb)
  - afterValidate: fn(valuesToUpdate, cb)
  - beforeUpdate: fn(valuesToUpdate, cb)
  - afterUpdate: fn(updatedRecord, cb)

##### `destroy` 的回呼

  - beforeDestroy: fn(criteria, cb)
  - afterDestroy: fn(destroyedRecords, cb)


### 例子

如果你想在密码储存到资料库前先加密，你可以使用 `beforeCreate` 生命周期回呼。

```javascript
var bcrypt = require('bcrypt');

module.exports = {

  attributes: {

    username: {
      type: 'string',
      required: true
    },

    password: {
      type: 'string',
      minLength: 6,
      required: true,
      columnName: 'encrypted_password'
    }

  },


  // 生命周期回呼
  beforeCreate: function (values, cb) {

    // 密码加密
    bcrypt.hash(values.password, 10, function(err, hash) {
      if(err) return cb(err);
      values.password = hash;
      // 呼叫 cb() 时带入一个参数，会返回错误。当某些条件失败要取消整个操作时很有用。
      cb();
    });
  }
};
```


<docmeta name="uniqueID" value="Lifecyclecallbacks631538">
<docmeta name="displayName" value="Lifecycle callbacks">

