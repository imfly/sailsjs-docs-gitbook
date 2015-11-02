# 多对多（Many-to-Many）
### 概述

多对多关联表示一个模型可以关联到许多其他模型，反之亦然。
因为两个模型都可以有许多关联模型，将需要建立一个新连接资料表来追踪这些关联。

Waterline 会看著你的模型，当它找到两个模型都有 collection 属性指向彼此时，会自动为你建立连接资料表。

因为你可能想要一个模型有多个多对多关联到另一个模型，`collection` 属性必需要有一个 `via` 键。这说明了哪一边的关联 `modal` 属性会用来提供记录。

使用 `User` 和 `Pet` 例子让我们来看看如何建立「一个 `User` 可能有很多 `Pet` 记录，和 `Pet` 可能有多个主人」的架构。


### 多对多例子

在这个例子中，我们将由 users 阵列和 pets 阵列开始。我们将建立资料到阵列的每个元素，然后关联所有的 `Pets` 与所有的 `Users`。如果一切运作正常，我们应该能够查询任何 `User`，看到他们「拥有」所有的 `Pets`。此外，我们应该能够查询任何 `Pet`，看到它被所有 `User` 拥有。


`myApp/api/models/pet.js`


```javascript

module.exports = {

  attributes: {
    name:'STRING',
    color:'STRING',

    // 加入一个参考到 User
    owners: {
      collection: 'user',
      via: 'pets',
      dominant:true
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

    // 加入一个参考到 Pet
    pets:{
      collection: 'pet',
      via: 'owners'
    }
  }

}

```


`myApp/config/bootstrap.js`

```javascript

module.exports.bootstrap = function (cb) {

// 在建立 users 之后，我们会在这储存他们来关联 pets
var storeUsers = []; 

var users = [{name:'Mike',age:'16'},{name:'Cody',age:'25'},{name:'Gabe',age:'107'}];
var ponys = [{ name: 'Pinkie Pie', color: 'pink'},{ name: 'Rainbow Dash',color: 'blue'},{ name: 'Applejack', color: 'orange'}]

// 这边进行实际的关联。
// 它需要一个宠物，然后迭代新建立的 Users 阵列，加入每一个到它的连接资料表var associate = function(onePony,cb){
  var thisPony = onePony;
  var callback = cb;

  storeUsers.forEach(function(thisUser,index){
    console.log('Associating ',thisPony.name,'with',thisUser.name);
    thisUser.pets.add(thisPony.id);
    thisUser.save(console.log);

    if (index === storeUsers.length-1)
      return callback(thisPony.name);
  })
};


// 这个回呼会在所有 Pets 建立后执行。
// 它送出每个新宠物和我们的 Users 到 'associate'
var afterPony = function(err,newPonys){
  while (newPonys.length){
    var thisPony = newPonys.pop();
    var callback = function(ponyID){
      console.log('Done with pony ',ponyID)
    }
    associate(thisPony,callback);
  }
  console.log('Everyone belongs to everyone!! Exiting.');

  // 这个回呼让我们离开 bootstrap.js 并继续启动应用程序！
  return cb();
};

// 这个回呼会在所有 Users 建立后执行。
// 它需要返回的 User 并储存到 storeUsers 阵列供稍后使用。
var afterUser = function(err,newUsers){
  while (newUsers.length)
    storeUsers.push(newUsers.pop());

  Pet.create(ponys).exec(afterPony);
};


User.create(users).exec(afterUser);

};
```

使用 `sails console` 启动应用程序

```sh

dude@littleDude:~/node/myApp$ sails console

info: Starting app in interactive mode...

Associating  Applejack with Gabe
Associating  Applejack with Cody
Associating  Applejack with Mike
Done with pony  Applejack
Associating  Rainbow Dash with Gabe
Associating  Rainbow Dash with Cody
Associating  Rainbow Dash with Mike
Done with pony  Rainbow Dash
Associating  Pinkie Pie with Gabe
Associating  Pinkie Pie with Cody
Associating  Pinkie Pie with Mike
Done with pony  Pinkie Pie
Everyone belongs to everyone!! Exiting.
info: Welcome to the Sails console.
info: ( to exit, type <CTRL>+<C> )

sails> null { name: 'Gabe',
  age: 107,
  createdAt: Wed Feb 12 2014 18:06:50 GMT-0600 (CST),
  updatedAt: Wed Feb 12 2014 18:06:50 GMT-0600 (CS