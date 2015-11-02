# Waterline: SQL/noSQL Data Mapper (ORM/ODM)


Sails comes installed with a powerful [ORM/ODM](http://stackoverflow.com/questions/12261866/what-is-the-difference-between-an-orm-and-an-odm) called [Waterline](https://github.com/balderdashy/waterline), a datastore-agnostic tool that dramatically simplifies interaction with one or more [databases](http://www.cs.umb.edu/cs630/hd1.pdf).  It provides an abstraction layer on top of the underlying database, allowing you to easily query and manipulate your data _without_ writing vendor-specific integration code.

### Database Agnosticism

In schemaful databases like [Postgres](), [Oracle](), and [MySQL](), models are represented by tables.  In [MongoDB](), they're represented by Mongo "collections".  In [Redis](), they're represented using key/value pairs.  Each database has its own distinct query dialect, and in some cases even requires installing and compiling a specific native module to connect to the server.  This involves a fair amount of overhead, and garners an unsettling level of [vendor lock-in](http://stackoverflow.com/questions/29868/how-important-is-it-to-choose-and-stick-to-a-technology-stack) to a specific database; e.g. if your app uses a bunch of SQL queries, it will be very hard to switch to Mongo later, or Redis, and vice versa.

Waterline query syntax floats above all that, focusing on business logic like creating new records, fetching/searching existing records, updating records, or destroying records.  No matter what database you're contacting, the usage is _exactly the same_.  Furthermore, Waterline allows you to [`.populate()`](http://sailsjs.org/#/documentation/reference/waterline/queries/populate.html) associations between models, _even if_ the data for each model lives in a different database.  That means you can switch your app's models from Mongo, to Postgres, to MySQL, to Redis, and back again - without changing any code.  For the times when you need low-level, database-specific functionality, Waterline provides a query interface that allows you to talk directly to your models' underlying database driver (see [.query()](http://beta.sailsjs.org/#/documentation/reference/waterline/models/query.html) and [.native()](http://beta.sailsjs.org/#/documentation/reference/waterline/models/native.html).)



### Scenario

Let's imagine you're building an e-commerce website, with an accompanying mobile app.  Users browse products by category or search for products by keyword, then they buy them.  That's it!  Some parts of your app are quite ordinary; you have an API-driven flow for logging in, signing up, order/payment processing, resetting passwords, etc. However, you know there are a few mundane features lurking in your roadmap that will likely become more involved.  Sure enough:

##### Flexibility

_You ask the business what database they would like to use:_

> "Datab... what?  Let's not be hasty, wouldn't want to make the wrong choice.  I'll get ops/IT on it.  Go ahead and get started though."

The traditional methodology of choosing one single database for a web application/API is actually prohibitive for many production use cases.  Oftentimes the application needs to maintain compatibility with one or more existing data sets, or it is necessary to use a few different types of databases for performance reasons.

Since Sails uses `sails-disk` by default, you can start building your app with zero configuration, using a local temporary file as storage.  When you're ready to switch to the real thing (and when everyone knows what that even is), just change your app's [connection configuration]().



##### Compatibility

_The product owner/stakeholder walks up to you and says:_

> "Oh hey by the way, the products actually already live in our point of sale system. It's some ERP thing I guess, something like "DB2"?  Anyways, I'm sure you'll figure it out- sounds easy right?"

Many enterprise applications must integrate with an existing database.  If you're lucky, a one-time data migration may be all that's necessary, but more commonly, the existing dataset is still b