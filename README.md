# GoGoWatcher Documentation

GoGoWatcher is a web analytics and user behavior tool. GoGoWatcher requires a subscription from **GoGo User** in order to be used.

# Quick Start

### GoGoWatcher

The GoGoWatcher tracks user activity. Generally speaking all you need to do to start tracking analytic data is to copy/paste the code snippit provided to you when you create a product in GoGo User.
```
<body>
    <!-- Your web page body -->

    <!-- This snippet needs to be added to the bottom of your <body> tag -->
    <script src="https://app.gogouser.com/dist/uiwatcher.min.js"></script>

    <script>
        window.addEventListener("load", function() {
            // accountId is your GoGo User account ID
            // token is your product watcher token
            const params = {
                accountId: "11111111-1111-1111-1111-111111111111",
                token: "22222222-2222-2222-2222-222222222222"
            }

            $gogoLoader.init(params);
    </script>
</body>
```

You can of course do more with the tool than the minimum. For instance, you can track custom analytic context values.
```
<body>
    <!-- Your web page body -->


    <!-- This snippet needs to be added to the bottom of your <body> tag -->
    <script src="https://app.gogouser.com/dist/uiwatcher.min.js"></script>

    <script>
        window.addEventListener("load", function() {
            // accountId is your GoGo Userr account ID
            // token is your product watcher token
            const params = {
                accountId: "11111111-1111-1111-1111-111111111111",
                token: "22222222-2222-2222-2222-222222222222"
            }

            // The init() method returns a promise. After init() completes the $gogo object is available for use.
            // The $gogo object is what is used to add custom analytic context values.
            $gogoLoader.init(params).then(function() {
                // You can add custom analytic context values.
                // If you track users, it's highly recommend to,
                // at a minimum, add the username to the context
                $gogo.watcher.addContext("username", "jdoe");

                // If your usernames aren't unique, then you can specify a clientId value to
                // further identify your users
                $gogo.watcher.addContext("clientId", "my-client-id");

                // You aren't limited in what context you want to track.
                $gogo.watcher.addContext("favorite-color", "Blue! No... Aaaaahhh.");
            });

        });
    </script>
</body>
```

### Single Page Apps (SPA)

If you're app is an SPA, then you can use the **newPage()** method to update the page you're tracking data for. It's recommended you call newPage() each time your SPA changes it's route.

```
$gogoLoader.init(params).then(function() {
    $gogo.newPage("login");

    // Route to "home" page
    $gogo.newPage("home");

    // Route to "dashboard" page
    $gogo.newPage("dashboard");
});
```

# Object Interface

### $gogoLoader object

This object is used to load the main `$gogo` object.

#### $gogoLoader.init(params)

This method initializes the `$gogo` object. This method is asycronous and returns a promise.
The method takes a single object parameter. The parameter has two required properties.

* **accountId** Your GoGo User accountId
* **token** GoGo Watcher token. Created and displayed when you create a **product/site** in **GoGo User**.

```
const params = {
    accountId: "11111111-1111-1111-1111-111111111111",
    token: "22222222-2222-2222-2222-222222222222"
}

$gogoLoader.init(params);
```

There are a few optional parameters that can be passed to the init method.

* **context (obj):** Default: `{}`. Set analytic context. Useful if you don't want to call `$gogo.watcher.addContext(key, value)` multiple times.
* **enabled (bool):** Default: `true`. Enable/Disable all data tracking. Useful if you want to conditionally enable/disable the tool. For instance, you may want it disabled in development environments and enabled in production.
* **replayEnabled (bool):** Default: `true`. Enable/Disable replay data tracking.

Here's an example using all available parameters (required and optional):

```
const params = {
    accountId: "11111111-1111-1111-1111-111111111111", // required
    token: "22222222-2222-2222-2222-222222222222", // required
    context: { // optional
        "clientId": "1234",
        "username": "jdoe",
        "favorite-color": "Red"
    },
    enabled: true, // optional
    replayEnabled: true // optional
}

$gogoLoader.init(params);
```

**NOTE:** You can call `$gogoLoader.init()` multiple times, but it will only load init parameters once per page load. This allows you to safely and efficiently call `$gogo` object (see below) methods throughout your code.

For example:

```
<script>
    const gogoParams = {
        accountId: "11111111-1111-1111-1111-111111111111",
        token: "22222222-2222-2222-2222-222222222222"
    }

    // Init on window load
    window.addEventListener("load", function() {
        $gogoLoader.init(gogoParams);
    });

    // Add a custom context if a specific button is clicked
    docuement.getElementById("your-custom-button").addEventListener("click", function() {
        // Wrapping $gogo calls in a $gogoLoader.init() promise chain is "safer",
        // but isn't necessary if you're possitive that $gogo has already been initialized.
        $gogoLoader.init(gogoParams).then(function() {
            $gogo.watcher.addContext("new-context-key", "new-context-value");
        });
    });
```

### $gogo object

This is the base library. It is initialized using the `$gogoLoader` object (see above).

#### $gogo.newPage(pageId)

Sets the **pageId**. This is useful for Single Page Apps (SPAs) or if you want to customize how you track specific pages.

```
$gogoLoader.init(params).then(function() {
    $gogo.newPage("login");
    $gogo.newPage("home");
});
```

### $gogo.watcher

User analytics and behavior object.

#### $gogo.watcher.addContext(key, value)

Adds a page context value. This method is useful for tracking custom analytic data per page.

```
$gogoLoader.init(params).then(function() {
    $gogo.watcher.addContext("clientId", "my-client-id");
    $gogo.watcher.addContext("username", "jdoe");
});
```

**NOTE:** GoGoWatcher has some specialized context values. These are reserved, so you should not use these keys with your own contexts.

 * **url-path (reserved):** This context is automatically set to window.location.pathname every page load.
 * **event-type (reserved):** This context is set to the javascript event that is being logged (i.e., click, change, load, etc).
 * **css-selector (reserved):** The css selector that triggered the javascript event.
 * **text-contect (reserved):** If the css selector that triggered the javascript event has text content, then this contect is set.
 * **dom-id (reserved):** If the DOM element that triggered the javascript event has an ID, then this context is set.
 * **page-id:** Set by `$gogo.newPage(pageId)`. Using this method is required by single page apps. If `$gogo.newPage(pageId)` is not called, then **page-id** will be set to **url-path** (see above). Do not set this context manually (i.e., `$gogo.watcher.addContext("page-id", "my_page")`). Doing so will produce undesired results. Use `$gogo.newPage("my_page")` to set this context.
 * **utm_source:** This context can get set automatically using customized links (i.e., google add links etc)
 * **utm_medium:** This context can get set automatically using customized links (i.e., google add links etc)
 * **utm_campaign:** This context can get set automatically using customized links (i.e., google add links etc)
 * **utm_term:** This context can get set automatically using customized links (i.e., google add links etc)
 * **utm_content:** This context can get set automatically using customized links (i.e., google add links etc)

#### $gogo.watcher.clearContext()

Clears the current page context.
```
$gogoLoader.init(params).then(function() {
    $gogo.watcher.addContext("clientId", "my-client-id");
    $gogo.watcher.addContext("username", "jdoe");
    $gogo.watcher.addContext("another-key", "another-value");

    // Clear all custom context values
    $gogo.watcher.clearContext();
});
```

#### $gogo.watcher.removeContext(key)

Removes a specific context by key.
```
$gogoLoader.init(params).then(function() {
    $gogo.watcher.addContext("clientId", "my-client-id");
    $gogo.watcher.addContext("username", "jdoe");
    $gogo.watcher.addContext("favorite-color", "another-value");

    // Remove only the "favorite-color" context
    $gogo.watcher.removeContext("favorite-color");
});
```

#### $gogo.watcher.enable()

Enables the watcher. By default the watcher is enabled, so you would only have to call this method if you manually disabled the watcher previously.
```
$gogoLoader.init(params).then(function() {
    $gogo.watcher.enable();
});
```

#### $gogo.watcher.disable()

Disables the watcher. This disables both heatmap data collection and replay data.
```
$gogoLoader.init(params).then(function() {
    $gogo.watcher.disable();
});
```

#### $gogo.watcher.enableReplay()

Enables replay tracking. Replays are enabled by default, so you would only have to call this method if you manually disabled replays. **NOTE:** The watcher must also be enabled (see `$gogo.watcher.enable()` above) to track any data.
```
$gogoLoader.init(params).then(function() {
    $gogo.watcher.enableReplay();
});
```

#### $gogo.watcher.disableReplay()

Disables replay tracking.
```
$gogoLoader.init(params).then(function() {
    $gogo.watcher.disableReplay();
});
```

#### $gogo.watcher.getSessionId()

Retrieves the current user's GoGoWatcher sessionId. This ID can be passed along to your help desk system (or other 3rd party solutions) and then used to find a specific user's replays.
```
$gogoLoader.init(params).then(function() {
    const sessionId = $gogo.watcher.getSessionId();

    console.log(sessionId);
});
```

#### $gogo.watcher.addError(errorEvent)

GoGoWatcher automatically logs any uncaught errors. If you'd like to manually log a specific error for any reason, then you may do so using this method.
```
$gogoLoader.init(params).then(function() {
    const type = "error";

    const errorEvent = new ErrorEvent(type, {
        error : new Error("Your custom error message."),
        message : "Your custom error message.",
        lineno : 402,
        filename : "my_file.js"
    });

    $gogo.watcher.addError(errorEvent);
});
```
