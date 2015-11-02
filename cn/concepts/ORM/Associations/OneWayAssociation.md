# 单向关联（One Way Association）
### 概述

单向关联就是一个模型关联到另一个模型。你可以查询该模型并提供所关联的模型。但是，你不能查询被关联的模型并提供关联它的模型。

### 单向关联例子

在这个例子中，我们关联了一个 `User` 到 `Pet`，而不是 `Pet` 到 `User`。

`myApp/api/models/pet.js`

```javascript

module.exports = {

  attributes: {
    name:'STRING',
    color:'STRING'
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

sails> Pet.create({name:'Pinkie Pie',color:'pink'}).exec(console.log)
null { name: 'Pinkie Pie',
  color: 'pink',
  createdAt: Tue Feb 11 2014 15:45:33 GMT-0600 (CST),
  updatedAt: Tue Feb 11 2014 15:45:33 GMT-0600 (CST),
  id: 5 }

sails> User.create({name:'Mike',age:21,pony:5}).exec(console.log);
null { name: 'Mike',
  age: 21,
  pony: 5,
  createdAt: Tue Feb 11 2014 15:48:53 GMT-0600 (CST),
  updatedAt: Tue Feb 11 2014 15:48:53 GMT-0600 (CST),
  id: 1 }

sails> User.find({name:'Mike'}).populate('pony').exec(console.log);
null [ { name: 'Mike',
    age: 21,
    pony: 
     { name: 'Pinkie Pie',
       color: 'pink',
       id: 5,
       createdAt: Tue Feb 11 2014 15:45:33 GMT-0600 (CST),
       updatedAt: Tue Feb 11 2014 15:45:33 GMT-0600 (CST) },
    createdAt: Tue Feb 11 2014 15:48:53 GMT-0600 (CST),
    updatedAt: Tue Feb 11 2014 15:48:53 GMT-0600 (CST),
    id: 1 } ]


```
### 注意事项
> 请查看 [Waterline 文件](https://github.com/balderdashy/waterline-docs/blob/master/associations.md)取得这种类型的关联的更多资讯

> 因为我们只形成一个关联于一个模型，`Pet` 没有归属于 `User` 模型的数量限制。如果我们想要，我们可以改变这一点，让 `Pet` 正好关联到一个 `User` ，且 `User` 正好关联到一个 `Pet`。

<docmeta name="uniqueID" value="OneWayAssociation708096">
<docmeta name="displayName" value="One Way Association">

