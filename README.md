# UiWatcher Documentation

UiWatcher is a web analytics tool. UiViewer is an accompanying tool that is used to visualize the data collected by UiWatcher. UiWatcher/UiViewer require a subscription from **UXT** in order to be used.

# Versions

The most recent version of UiWatcher will have the name **uiwatcher.min.js**. If you prefer to use a specific release of the tool you can use the following naming convention: **uiwatcher-MAJOR.MINOR.PATCH.min.js**.
  
The current version is: **uiwatcher-0.0.1.min.js**

# Quick Start

### UiWatcher
The UiWatcher tracks user activity.
```
<script src="http://www.uiwatcher.com/dist/uiwatcher.min.js"></script>

<script>
    var watcher;

    window.onload = function() {
        // accountId is your UXT account ID
        // token is your product's watcher token
        // pageId is the unique page that you are tracking activity for
        let params = {
            accountId: "11111111-1111-1111-1111-111111111111",
            token: "22222222-2222-2222-2222-222222222222",
            pageId: "login"
        }

        watcher = new UiWatcher(params);

        // You can add custom analytic context values.
        // If you track users, it's highly recommend to,
        // at a minimum, add the username to the context
        watcher.addContext("client", "my-client");
        watcher.addContext("username", "jdoe");
    };

</script>
```

### UiViewer
The UiViewer will visualize the data captured by the UiWatcher.
```
<script src="http://www.uiwatcher.com/dist/uiwatcher.min.js"></script>

<script>
    // The UiViewer instance varriable will be assigned to this var.
    // NOTE: This instance varriable must be globally accessable.
    var viewer;

    window.onload = function() {
        // name is the name of the UiViewer instance varriable (see assignment below)
        // accountId is your UXT account ID
        // token is your product's viewer token (NOTE: watcher and viewer tokens are distinct per product)
        // pageId is the unique page name that you are viewing activity for
        let params = {
            name: "viewer",
            accountId: "11111111-1111-1111-1111-111111111111",
            token: "33333333-3333-3333-3333-333333333333",
            pageId: "login"
        }

        // "viewer" is the UiViewer instance varriable (must be passed as a param to the constructor)
        viewer = new UiViewer(params);
    };

</script>
```

### Single Page Apps (SPA)

If you're app is an SPA, then you can use the **setPageId()** method to update the page you're tracking data for. It's recommended you set the Page ID each time your SPA changes it's route.

```
// NOTE: Both the UiWatcher and UiViewer have a setPageId() method.

watcher.setPageId('login');

// Route to "home" page
watcher.setPageId('home');

// Route to "dashboard" page
watcher.setPageId('dashboard');


// Same process for viewer
viewer.setPageId('login');

// Route to "home" page
viewer.setPageId('home');

// Route to "dashboard" page
viewer.setPageId('dashboard');

```

# Object Interface

### UiWatcher

#### UiWatcher(params)
The constructor takes a single object parameter. The parameter has three required properties.

* **accountId** Your UXT accountId
* **token** UiWatcher token. Created and displayed when you create a **product** in **UXT**.
* **pageId** The page name that uniquely describes the page your are currently tracking data for.

```
let params = {
    accountId: "11111111-1111-1111-1111-111111111111",
    token: "22222222-2222-2222-2222-222222222222",
    pageId: "login"
}

let watcher = new UiWatcher(params);
```

#### UiWatcher.setPageId(pageId)
Sets the **pageId**. This is useful for Single Page Apps or if you want to customize how you track specific pages.

```
watcher.setPageId('login');
watcher.setPageId('home');
```

#### UiWatcher.addContext(key, value)
Adds a page context value. This method is useful for tracking custom analytic data per page.

```
watcher.addContext("client", "my-client");
watcher.addContext("username", "jdoe");
```

#### UiWatcher.clearContext()
Clears the current page context.
```
watcher.clearContext();
```

### UiViewer

#### UiViewer(params)
The constructor takes a single object parameter. The parameter has four required properties.

* **name** The UiViewer instance name. Must be a global var.
* **accountId** Your UXT accountId
* **token** UiViewer token. Created and displayed when you create a **product** in **UXT**.
* **pageId** The page name that uniquely describes the page your are currently viewing data for.

```
let params = {
    name: "viewer",
    accountId: "11111111-1111-1111-1111-111111111111",
    token: "33333333-3333-3333-3333-333333333333",
    pageId: "login"
}

// "viewer" is the UiViewer instance varriable (must be passed as a param to the constructor)
viewer = new UiViewer(params);
```

#### UiViewer.setPageId(pageId)
Sets the **pageId**. This is useful for Single Page Apps or if you want to customize how you view specific pages.

```
viewer.setPageId('login');
viewer.setPageId('home');
```

#### UiViewer.update()
Updates the current view. Applying any filter options and then re-drawing all the data visualizations.

```
viewer.update();
```

#### UiViewer.toggle()
Toggles (shows/hides) the UiViewer control pannel.

```
viewer.toggle();
```

#### UiViewer.draw()
Re-draws all the data visualizations.

```
viewer.draw();
```
