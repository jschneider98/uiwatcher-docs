# UiWatcher Documentation

UiWatcher is a web analytics and user behavior tool. UiWatcher requires a subscription from **GoGo User** in order to be used.

# Quick Start

### UiWatcher
The UiWatcher tracks user activity. Generally speaking all you need to do to start tracking analytic data is to copy/paste the code snippit provided to you when you create a product in GoGo User.
```
<body>
    <!-- Your web page body -->

    <!-- This script tag can be added to your <head> tag if you prefer -->
    <script src="https://app.gogouser.com/dist/uiwatcher.min.js"></script>

    <!-- This snippet needs to be added to the bottom of your <body> tag -->
    <script>
        window.addEventListener("load", function() {
            // accountId is your GoGo User account ID
            // token is your product's watcher token
            const params = {
                accountId: "11111111-1111-1111-1111-111111111111",
                token: "22222222-2222-2222-2222-222222222222"
            }

            $ui.initWatcher(params);
    </script>
</body>
```

You can of course do more with the tool than the minium. For instance, you can track custom analytic context values.
```
<body>
    <!-- Your web page body -->

    <!-- This script tag can be added to your <head> tag if you prefer -->
    <script src="https://app.gogouser.com/dist/uiwatcher.min.js"></script>

    <!-- This snippet needs to be added to the bottom of your <body> tag -->
    <script>
        window.addEventListener("load", function() {
            // accountId is your GoGo Userr account ID
            // token is your product's watcher token
            const params = {
                accountId: "11111111-1111-1111-1111-111111111111",
                token: "22222222-2222-2222-2222-222222222222"
            }

            $ui.initWatcher(params);

            // You can add custom analytic context values.
            // If you track users, it's highly recommend to,
            // at a minimum, add the username to the context
            $ui.watcher.addContext("username", "jdoe");

            // If your usernames aren't unique, then you can specify a clientId value to further identify your users
            $ui.watcher.addContext("clientId", "my-client-id");

            // You aren't limited in what context you want to track.
            $ui.watcher.addContext("favorite-color", "Blue! No... Aaaaahhh.");
        });
    </script>
</body>
```

### Single Page Apps (SPA)

If you're app is an SPA, then you can use the **newPage()** method to update the page you're tracking data for. It's recommended you call newPage() each time your SPA changes it's route.

```
$ui.newPage('login');

// Route to "home" page
$ui.newPage('home');

// Route to "dashboard" page
$ui.newPage('dashboard');
```

# Object Interface

### $ui object
This is the base library.


#### $ui.initWatcher(params)
This method initializes the UiWatcher object. The method takes a single object parameter. The parameter has two required properties.

* **accountId** Your GoGo User accountId
* **token** UiWatcher token. Created and displayed when you create a **product** in **GoGo User**.

```
const params = {
    accountId: "11111111-1111-1111-1111-111111111111",
    token: "22222222-2222-2222-2222-222222222222"
}

$ui.initWatcher(params);
```

#### $ui.newPage(pageId)
Sets the **pageId**. This is useful for Single Page Apps (SPAs) or if you want to customize how you track specific pages.

```
$ui.newPage('login');
$ui.newPage('home');
```

### UiWatcher
User analytics and behavior object. Must call $ui.initWatcher(params) to initialize.

#### UiWatcher.addContext(key, value)
Adds a page context value. This method is useful for tracking custom analytic data per page.

```
$ui.watcher.addContext("clientId", "my-client-id");
$ui.watcher.addContext("username", "jdoe");
```

#### UiWatcher.clearContext()
Clears the current page context.
```
$ui.watcher.clearContext();
```

#### UiWatcher.removeContext(key)
Removes a specific context by key.
```
$ui.watcher.removeContext('favorite-color');
```

#### UiWatcher.enable()
Enables the watcher.
```
$ui.watcher.enable();
```

#### UiWatcher.disable()
Disables the watcher. This disables both heatmap data collection and replay data.
```
$ui.watcher.disable();
```

#### UiWatcher.enableReplay()
Enables replay tracking. NOTE: The watcher must also be enabled (see UiWatcher.enable() above) to track any data.
```
$ui.watcher.disableReplay();
```

#### UiWatcher.disableReplay()
Disables replay tracking.
```
$ui.watcher.disableReplay();
```

#### UiWatcher.getSessionId()
Retrieves the current user's UiWatcher sessionId. This ID can be passed along to your help desk system (or other 3rd party solutions) and then used to find a specific user's replays.
```
const sessionId = $ui.watcher.getSessionId();

console.log(sessionId);
```

#### UiWatcher.addError(errorEvent)
UiWatcher automatically logs any uncaught errors. If you'd like to manually log a specific error for any reason, then you may do so using this method.
```
const type = 'error';

var errorEvent = new ErrorEvent(type, {
    error : new Error('Your custom error message.'),
    message : 'Your custom error message.',
    lineno : 402,
    filename : 'my_file.js'
});

$ui.watcher.addError(errorEvent);
```
