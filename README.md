Logger
======

Nothing special or innovative, just a standard logger that does what I want. Working on making this compatible in both both the browser and Node.js environments.


Usage
=====
The following examples all assume logging is enabled. For details on how to enable (and disable) logging, see the next section, [Enabling and Disabling Logging](#enabling-and-disabling-logging)

Browser Global
--------------

### index.html

```html
<script src="/path/to/logger.js"></script>
<script>
    // Note that window.Logger is now available.
    var LOG = new Logger({
        module: "foo",
        prefix: "Foo Module: "
    });

    LOG.debug("here is a debug message");
    LOG.info("here is an info message");
    // => debug - Foo Module: here is a debug message
    // => info  - Foo Module: here is an info message
</script>
```

## Browser AMD

### index.html

```html
<script data-main="app.js" src="/path/to/require.js"></script>
```

### app.js

```javascript
requirejs(['logger'], function(Logger) {
        var LOG = new Logger({
            module: "bar",
            prefix: "Bar Module: "
        });
        LOG.info("here is an info message");
        // => info  - Bar Module: here is an info message
});
```

## Node.js Module

```javascript
    var LOG = require("./logger");
    LOG.debug("here is a debug message");
    // Note that output is currently only optimized for the browser.
    // This looks pretty bad on the Node console but can easily be fixed in the future.
    // => %cdebug - undefined:here is a debug message color:gray
```

Enabling and Disabling Logging
==============================

Browser
-------
Regardless of how the `Logger` constructor is loaded, enabling and disabling logging is determined by the query string. An example query string might look like this: `?log=foo;bar&level=warn`. The full API is as follows:

### log

In general, the value for the *log* parameter should be a semicolon-delimited list of module names. Only Logger instances which have a matching `module` property in this list will emit messages. The value *all* is a special case; when `log=all` is present in the query string, all Logger instances will emit messages. To selectively _disable_ logging from certain modules, prefix the module name with a "-". For example, if we have `?log=foo;bar;-baz` in the query string, then Loggers for the `foo` and `bar` modules will emit messages, while Loggers for the `baz` module will remain silent.

### level

This is exactly what you expect. Set the logging level threshold. Only messages at or above this threshold will be emitted. Possible values include `debug`, `info`, `warn,` and `error`.
