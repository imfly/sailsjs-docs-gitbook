# 一对一（One-to-One）
### 概述

一对一关联表示一个模型可能只与另一个模型关联。为了使模型知道它与其他哪些模型关联，外键必需包含在记录中。

### 一对一例子

在这个例子中，我们关联了一个 `Pet` 到 `User`。在这种情况下 `User` 可能只有一个 `Pet`，但是 `Pet` 并不局限于单一 `User`。


`myApp/api/models/pet.js`

```javascript

module.exports = {

  attributes: {
    name:'STRING',
    color:'STRING',
    owner:{
      model:'user'
    }
  }

}


```

`myApp/api/models/user.js`

```javascript

module.exports = {

  attributes: {
    name:'STRING',
    age:'INTEGER',
    pony:{
      model: 'pet'
    }
  }

}
```

使用 `sails console`

```sh

sails> User.create({ name: 'Mike', age: 21}).exec(console.log);
null { name: 'Mike',
  age: 21,
  createdAt: Thu Feb 20 2014 17:12:18 GMT-0600 (CST),
  updatedAt: Thu Feb 20 2014 17:12:18 GMT-0600 (CST),
  id: 1 }
  
sails> Pet.create({ name: 'Pinkie Pie', color: 'pink', owner: 1}).exec(console.log)
null { name: 'Pinkie Pie',
    color: 'pink',
    owner: 1,
    createdAt: Thu Feb 20 2014 17:26:16 GMT-0600 (CST),
    updatedAt: Thu Feb 20 2014 17:26:16 GMT-0600 (CST),
    id: 2 }
    
sails> Pet.find().populate('owner').exec(console.log)
null [ { name: 'Pinkie Pie',
    color: 'pink',
    owner: 
     { name: 'Mike',
       age: 21,
       id: 1,
       createdAt: Thu Feb 20 2014 17:12:18 GMT-0600 (CST),
       updatedAt: Thu Feb 20 2014 17:12:18 GMT-0600 (CST) },
    createdAt: Thu Feb 20 2014 17:26:16 GMT-0600 (CST),
    updatedAt: Thu Feb 20 2014 17:26:16 GMT-0600 (CST),
    id: 2 } ]

sails> User.find().populate('pony').exec(console.log)
null [ { name: 'Mike',
    age: 21,
    createdAt: Thu Feb 20 2014 18:11:15 GMT-0600 (CST),
    updatedAt: Thu Feb 20 2014 18:11:15 GMT-0600 (CST),
    id: 2,
    pony: undefined } ]

sails> User.update({name:'Mike'},{pony:2}).exec(console.log)
null [ { name: 'Mike',
    age: 21,
    createdAt: Thu Feb 20 2014 17:12:18 GMT-0600 (CST),
    updatedAt: Thu Feb 20 2014 17:30:58 GMT-0600 (CST),
    id: 1,
    pony: 2 } ]

sails> User.findOne(1).populate('pony').exec(console.log)
null { name: 'Mike',
  age: 21,
  createdAt: Thu Feb 20 2014 17:12:18 GMT-0600 (CST),
  updatedAt: Thu Feb 20 2014 17:30:58 GMT-0600 (CST),
  id: 1,
  pony: 
   { name: 'Pinkie Pie',
     color: 'pink',
     id: 2,
     createdAt: Thu Feb 20 2014 17:26:16 GMT-0600 (CST),
     updatedAt: Thu Feb 20 2014 17:26:16 GMT-0600 (CST),
     owner: 1 } }

```
### 注意事项
> 请查看 [Waterline 文件](https://github.com/balderdashy/waterline-docs/blob/master/associations.md)取得这种类型的关联的更多资讯


<docmeta name="uniqueID" value="OnetoOne169258">
<docmeta name="displayName" value="One-to-One">

