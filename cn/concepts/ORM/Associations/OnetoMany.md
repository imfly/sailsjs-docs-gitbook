# 一对多（One-to-Many）
### 概述

一对多关联表示一个模型可以关联到许多其他模型。要建立这种关联，要加入一个虚拟属性 `collection` 到模型。在一对多关联中，一边必需有 `collection` 属性，另一边必需包含一个 `modal` 属性。这让「Many」那侧知道当使用 `populate` 时，它需要取得哪些记录。

因为你可能想要一个模型有多个一对多关联到另一个模型，`collection` 属性必需要有一个 `via` 键。这说明了哪一边的关联 `modal` 属性会用来提供记录。

### 一对多例子

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
    pets:{
      collection: 'pet',
      via: 'owner'
    }
  }

}

```

使用 `sails console`

```sh

sails> User.create({name:'Mike',age:'21'}).exec(console.log)
null { pets: [Getter/Setter],
  name: 'Mike',
  age: 21,
  createdAt: Tue Feb 11 2014 17:49:04 GMT-0600 (CST),
  updatedAt: Tue Feb 11 2014 17:49:04 GMT-0600 (CST),
  id: 1 }

sails> Pet.create({name:'Pinkie Pie',color:'pink',owner:1}).exec(console.log)
null { name: 'Pinkie Pie',
  color: 'pink',
  owner: 1,
  createdAt: Tue Feb 11 2014 17:58:04 GMT-0600 (CST),
  updatedAt: Tue Feb 11 2014 17:58:04 GMT-0600 (CST),
  id: 2 }

sails> Pet.create({name:'Applejack',color:'orange',owner:1}).exec(console.log)
null { name: 'Applejack',
  color: 'orange',
  owner: 1,
  createdAt: Tue Feb 11 2014 18:02:58 GMT-0600 (CST),
  updatedAt: Tue Feb 11 2014 18:02:58 GMT-0600 (CST),
  id: 4 }

sails> User.find().populate('pets').exec(function(err,r){console.log(r[0].toJSON())});
{ pets: 
   [ { name: 'Pinkie Pie',
       color: 'pink',
       id: 2,
       createdAt: Tue Feb 11 2014 17:58:04 GMT-0600 (CST),
       updatedAt: Tue Feb 11 2014 17:58:04 GMT-0600 (CST),
       owner: 1 },
     { name: 'Applejack',
       color: 'orange',
       id: 4,
       createdAt: Tue Feb 11 2014 18:02:58 GMT-0600 (CST),
       updatedAt: Tue Feb 11 2014 18:02:58 GMT-0600 (CST),
       owner: 1 } ],
  name: 'Mike',
  age: 21,
  createdAt: Tue Feb 11 2014 17:49:04 GMT-0600 (CST),
  updatedAt: Tue Feb 11 2014 17:49:04 GMT-0600 (CST),
  id: 1 }

sails> Pet.find(4).populate('owner').exec(console.log)
null [ { name: 'Applejack',
    color: 'orange',
    owner: 
     { pets: [Getter/Setter],
       name: 'Mike',
       age: 21,
       id: 1,
       createdAt: Tue Feb 11 2014 17:49:04 GMT-0600 (CST),
       updatedAt: Tue Feb 11 2014 17:49:04 GMT-0600 (CST) },
    createdAt: Tue Feb 11 2014 18:02:58 GMT-0600 (CST),
    updatedAt: Tue Feb 11 2014 18:02:58 GMT-0600 (CST),
    id: 4 } ]

```

### 注意事项
> 请查看 [Waterline 文件](https://github.com/balderdashy/waterline-docs/blob/master/associations.md)取得这种类型的关联的更多资讯


<docmeta name="uniqueID" value="OnetoMany478093">
<docmeta name="displayName" value="One-to-Many">

