# Models

A model represents a collection of structured data, usually corresponding to a single table or collection in a database.  Models are usually defined by creating a file in an app's `api/models/` folder.

![screenshot of a Waterline/Sails model in Sublime Text 2](http://i.imgur.com/8uRlFi8.png)


<!--

// api/models/Product.js
module.exports = {
  attributes: {
    nameOnMenu: { type: 'string' },
    price: { type: 'string' },
    percentRealMeat: { type: 'float' },
    numCalories: { type: 'integer' }
  }
}
-->


### Using models

Models may be accessed from our controllers, policies, services, responses, tests, and in custom model methods.  There are many built-in methods available on models, the most important of which are the query methods: [find](http://beta.sailsjs.org/#/documentation/reference/waterline/models/find.html), [create](http://beta.sailsjs.org/#/documentation/reference/waterline/models/create.html), [update](http://beta.sailsjs.org/#/documentation/reference/waterline/models/update.html), and [destroy](http://beta.sailsjs.org/#/documentation/reference/waterline/models/destroy.html).  These methods are [asynchronous](https://github.com/balderdashy/sails-docs/blob/master/PAGE_NEEDED.md) - under the covers, Waterline has to send a query to the database and wait for a response.

Consequently, query methods return a deferred query object.  To actually execute a query, `.exec(cb)` must be called on this deferred object, where `cb` is a callback function to run after the query is complete.

Waterline also includes opt-in support for promises.  Instead of calling `.exec()` on a query object, we can call `.then()`, `.spread()`, or `.catch()`, which will return a [Bluebird promise](https://github.com/petkaantonov/bluebird).





### Model Methods (aka "static" or "class" methods)

Model class methods are functions built into the model itself that perform a particular task on its instances (records).  This is where you will find the familiar CRUD methods for performing database operations like `.create()`, `.update()`, `.destroy()`, `.find()`, etc.


###### Custom model methods

Waterline allows you to define custom methods on your models.  This feature takes advantage of the fact that Waterline models ignore unrecognized keys, so you do need to be careful about inadvertently overriding built-in methods and dynamic finders (don't define methods named "create", etc.)  Custom model methods are most useful for extrapolating controller code that relates to a particular model; i.e. this allows you to pull code your controllers and into reusuable functions that can be called from anywhere (i.e. don't depend on `req` or `res`.)

Model methods are generally asynchronous functions.  By convention, asynchronous model methods should be 2-ary functions, which accept an object of inputs as their first argument (usually called `opts` or `options`) and a Node callback as the second argument.  Alternatively, you might opt to return a promise (both strategies work just fine- it's a matter of preference.  If you don't have a preference, stick with Node callbacks.)

A best practice is to write your static model method so that it can accept either a record OR its primary key value.  For model records that operate on/from _multiple_ records at once, you should allow an array of records OR an array of primary key values to be passed in.  This takes more time to write, but makes your method much more powerful.  And since you're doing this to extrapolate commonly-used logic anyway, it's usually worth the extra effort.

For example:

```js
// in api/models/Monkey.js...

// Find monkeys with the same name as the specified person
findWithSameNameAsPerson: function (opts, cb) {

  var person = opts.person;

  // Before doing anything else, check if a primary key value
  // was passed in instead of a record, and if so, lookup which
  // person we're even talking about:
  (function _lookupPersonIfNecessary(afterLookup){
    // (this self-calling function is just for concise-ness)
    if (typeof person === 'object')) return afterLookup(n