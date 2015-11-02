# 穿透关联（Through Associations）
### 概述

多对多穿透关联的行为和多对多关联相同，且会自动为你建立例外的连接表。这使你可以附加额外的属性到连接表内的关联。

不幸的是，他们尚未支持。请不要担心，有一个简单的解决方法。

你可以通过使用一个额外的模型为中介来实现这一目标。你可以使用多个一对多关联到中介模型，取代两个模型间的多对多关联。







<docmeta name="uniqueID" value="ThroughAssociations740718">
<docmeta name="displayName" value="Through Associations">

