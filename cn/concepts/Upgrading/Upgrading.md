# Upgrading

> TODO:
> move this to the appropriate part of the docs (shouldn't show up in reference-- instead it should appear at the top of the other migration guide/changelog stuff in the "Support" section)
>

Sails v0.11 is finally here.

For the most part, running `sails lift` in an existing v0.9 project should **just work**.  The core contributors have taken a number of steps to make the upgrade as easy as possible, and if you follow the deprecation messages in the console, you should do just fine.

Sails v0.11 comes with some big changes.  The sections below provide a high level overview of what's changed, major bug fixes, enhancements and new features, as well as a basic tutorial on how to upgrade your v0.9.x Sails app to v0.11.

The Sails/Waterline communities have been extremely supportive to newer users throughout the beta. If you run into problems, please reach out for help in IRC, the Google Group and on Stack Overflow.

If you believe you've encountered an upgrade issue not addressed in this document, please let the core team know by sending a pull request to [this file on Github](https://github.com/balderdashy/sails-docs/edit/master/reference/Upgrading.md).  If you don't know the answer to a question, explain the upgrade issue you've having in the appropriate section (or a new one), and someone will do their best to answer it/provide a migration strategy.

Thanks!

@mikermcneil

========================================

### Contents

|     | Jump to...        |
|-----|-------------------------|
|     | [File Uploads](#documentation/concepts/Upgrading?q=file-uploads) |
|     | [Blueprints](#/documentation/concepts/Upgrading?q=blueprints) |
|     | [Policies](#/documentation/concepts/Upgrading?q=policies) |
|     | [Associations](#/documentation/concepts/Upgrading?q=associations) |
|     | [Pubsub](#/documentation/concepts/Upgrading?q=pubsub) |
|     | [`.done()` vs. `.exec()`](#/documentation/concepts/Upgrading?q=done%28%29-vs-exec%28%29)
|     | [Generators](#/documentation/concepts/Upgrading?q=generators) |
|     | [Command-Line Tool](#/documentation/concepts/Upgrading?q=commandline-tool) |
|     | [Custom Server Responses](#/documentation/concepts/Upgrading?q=custom-server-responses) |
|     | [Legacy Data in `sails-disk`](#/documentation/concepts/Upgrading?q=legacy-data-stored-in-the-temporary-sailsdisk-database) |
|     | [Validations](#/documentation/concepts/Upgrading?q=validations-upgrade-to-validator-3x) |
|     | [Adapter/Connections Configuration](#/documentation/concepts/Upgrading?q=adapter%2Fdatabase-configuration) |
|     | [Blueprints/Controllers Configuration](#/documentation/concepts/Upgrading?q=controller-configuration) |
|     | [Layout Paths](#/documentation/concepts/Upgrading?q=layout-paths) |
========================================


### File Uploads
<a name="file-uploads"></a>
The Connect multipart middleware [will soon be officially deprecated](http://www.senchalabs.org/connect/multipart.html). But since this module was used as the built-in HTTP body parser in Sails v0.9 and Express v3, this is a breaking change for v0.9 Sails projects relying on `req.files`.

By default in v0.11, Sails includes [`skipper`](http://github.com/balderdashy/skipper), a body parser which allows for streaming file uploads without buffering tmp files to disk.  For run-of-the-mill file upload use cases, Skipper comes with bundled support for uploads to local disk (via [skipper-disk](https://github.com/balderdashy/skipper-disk)), but streaming uploads can be plugged in to any of its supported adapters.

For examples/documentation, please see the Skipper repository as well as the Sails documentation on `req.file()`.

#### Why?
A body parser's job is to parse the "body" of incoming multipart HTTP requests.  Sometimes, that "body" includes text parameters, but sometimes, it includes file uploads.

Connect multipart is great code, and it supports both file uploads AND text parameters in multipart requests. But like most modules of its kind, it accomplishes this by buffering file uploads to disk.  This can quickl