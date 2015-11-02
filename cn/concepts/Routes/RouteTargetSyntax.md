# Custom Routes
### Overview

Sails allows you to explicitly route URLs in several different ways in your **config/routes.js** file.  Every route configuration consists of an **address** and a **target**, for example:

```
'GET /foo/bar': 'FooController.bar'
^^^address^^^^  ^^^^^^target^^^^^^^
```

### Route Address

The route address indicates what URL should be matched in order to apply the handler and options defined by the target.  A route consists of an optional verb and a mandatory path:

```
'POST  /foo/bar'
^verb^ ^^path^^
```

If no verb is specified, the target will be applied to any request that matches the path, regardless of the HTTP method used (**GET**, **POST**, **PUT** etc.).  Note the initial `/` in the path--all paths should start with one in order to work properly.

####Wildcards and dynamic parameters

In addition to specifying a static path like **foo/bar**, you can use `*` as a wildcard:

```
'/*'
```
    
will match all paths, where as:

```
'/user/foo/*'
```
    
will match all paths that *start* with **/user/foo**.

You can capture the parts of the address that are matched by wildcards into named parameters by using the `:paramName` wildcard syntax instead of the `*`:

```
'/user/foo/:name/bar/:age'
```

Will match the same URLs as:

```
'/user/foo/*/bar/*'
```
    
but will provide the values of the wildcard portions of the route to the route handler as `req.param('name')` and `req.param('age')`, respectively.

#### Regular expressions in addresses

In addition to the wildcard address syntax, you may also use Regular Expressions to define the URLs that a route should match.  The syntax for defining an address with a regular expression is:

`"r|<regular expression string>|<comma-delimited list of param names>"`

That's the letter "**r**", followed by a pipe character `|`, a regular expression string *without delimiters*, another pipe, and a list of parameter names that should be mapped to parenthesized groups in the regular expression.  For example:

`"r|^/\\d+/(\\w+)/(\\w+)$|foo,bar": "MessageController.myaction"`

Will match `/123/abc/def`, running the `myaction` action of `MessageController` and supplying the values `abc` and `def` as `req.param('foo')` and `req.param('bar')`, respectively.

Note the double-backslash in `\\d` and `\\w`; this escaping is necessary for the regular expression to work correctly!

#### About route ordering

When using wildcards or regular expressions in your addresses, keep in mind that the ordering of your routes in **config/routes.js** matters; URLs are matched against addresses in the list from the top down.  If you have two configurations in this order:

    '/user': 'UserController.doSomething',
    '/*'   : 'CatchallController.doSomethingElse'
    
then a request to `/user` will not be matched by the second configuration unless the first configuration's handler calls `next()` in its code, which is discouraged (only [policies](http://beta.sailsjs.org/#/documentation/concepts/Policies) should call `next()`).  Unless you're doing something very advanced, it is safe to assume that every request will be handled by at most one route in your **config/routes.js** file.

### Route Target

The address portion of a custom route specifies which URLs the route should match.  The *target* portion specifies what Sails should do after the match is made.  A target can take one of several different forms.  In some cases you may want to chain multiple targets to a single address by placing them in an array, but in most cases each address will have only one target.  The different types of targets are discussed below, followed by a discussion of the various options that can be applied to them.

#### Controller / action target syntax

The most common type of target is one which binds a route to a custom [controller action](http://beta.sailsjs.org/#/documentation/concepts/Controllers?q=actions).  The following four routes are equivalent:

```
'GET /foo/go': 'FooController.myGoAction',
'GET /foo/go': 'Foo.myGoAction',
'GET /foo/go': {controller: "Foo", action: "myGoAction"},
'GET /