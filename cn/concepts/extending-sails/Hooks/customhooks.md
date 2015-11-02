# Understanding Hooks
### Why Should I Use Hooks? 
Hooks exist to provide a way for you to execute custom code as your app is lifted.  This is one of the better places to build lower level functionality that might need direct interaction with sails core. 

### What Are They?
On a base level, a hook is just a function that gets imported and immediately executed when you lift your app.  This function is called with the `sails global` object as an argument and returns a `definition` object with various methods and properties that provide structure.

### How Do They Work?
Sails uses hooks to provide most of it's core functionality.  Sails has a hook for it's http server, pubsub functionality, interfacing with an ORM (waterline by default), managing Grunt tasks, etc.  Sails even uses a hook for loading your custom hooks.  It's called `userhooks` and it runs after the http server but before the logger.  It's one of the last things that happens as you lift your app.

### Where Are They?
Like we said before, Sails relies on hooks internally.  If you want to take a look at them, they can be found in `myApp/node_modules/sails/lib/hooks`.  While these are a great resource when writing your own custom hooks, I would be careful not to change them unless you know what you're doing.  Eventually it will become easier to use drop-in replacements to the hooks in sails-core.

User defined hooks are located in `myApp/api/hooks`.  This folder isn't created when you create a new project.  Should it be?  Who knows.  Either way, if you're making your own hook, you'll start off by creating a folder called `hooks` in `myApp/api`.


## The Simplest User Hook

myApp/api/hooks/simple/index.js

```javascript

module.exports = function mySimpleHook(sails) {

    return {
        talkAboutCats: function(adjective){
            console.log('Cats are',adjective,'!')
        },

        // Runs automatically when the hook initializes
        initialize: function (cb) {

            var hook = this;

            // You must trigger `cb` so sails can continue loading.
            // If you pass in an error, sails will fail to load, and display your error on the console.

            hook.talkAboutCats('great');

        return cb();
        }
    }
};


```

While this isnt a very useful hook, it demonstrates how simple hooks really are.  When your sails app lifts, Sails imports the file `myApp/api/hooks/simple/index.js`, then executes the function with the `sails global` object as a parameter.  It then executes the `initialize` method on the object returned.  If that isn't found, Sails quietly gives up and moves on.   Otherwise, the hook registers sucesfully and can later be accessed on the `sails global` object with `sails.hooks.talkAboutCats('cruel')`. 



## What Else Can I Do?

To answer that, let's look at one of the hooks that `sails core` uses internally.  Below is the CSRF hook.  Sails uses it to validate CSRF tokens.

myApp/api/hooks/cathook/index.js

```javascript
module.exports = function(sails) {

  // This hooks dependencies

  var _ = require('lodash'),
  util = require('sails-util'),
  Hook = require('../../index');

  // Returning the hooks definition object

  return {

    defaults: {
      csrf: false
    },


    // See `configure` notes below

    configure: function () {
      if (sails.config.csrf === true) {
        sails.config.csrf = {
          grantTokenViaAjax: true,
          protectionEnabled: true,
          origin: ''
        };
      }
      else if (sails.config.csrf === false) {
        sails.config.csrf = {
          grantTokenViaAjax: false,
          protectionEnabled: false,
          origin: ''
        };
      }
      else {
        _.defaults(typeof sails.config.csrf === 'object' ? sails.config.csrf : {}, {
          grantTokenViaAjax: true,
          protectionEnabled: true,
          origin: ''
        });
      }
    },

    // See `defaults` notes below

    defaults: {

    },

    // See `routes` notes below

    routes: {

      // In this hook, both the `before` and `after` objects
      // use a wildcard 