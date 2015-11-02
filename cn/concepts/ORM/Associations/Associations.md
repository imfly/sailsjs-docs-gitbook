# 关联（Associations）

使用 Sails 和 Waterline，你可以跨多个资料储存区来关联模型。这代表，即使你的使用者储存在 [PostgreSQL](http://www.postgresql.org/)，而他们的相片储存在 [MongoDB](http://www.mongodb.com/)，你可以与资料进行互动，就好像他们储存在相同的资料库中。你也可以使用相同桥接器跨越不同[连线](http://beta.sailsjs.org/#/documentation/reference/sails.config/sails.config.connections.html)（即资料储存区／资料库）的关联。举个能派上用场的例子，你的应用程序需要从公司的资料中心的 [MySQL](http://www.mysql.com/) 资料库存取／更新旧的食谱资料，但也要从云端的全新 MySQL 资料库存取材料资料。

<docmeta name="uniqueID" value="Associations913185">
<docmeta name="displayName" value="Associations">

