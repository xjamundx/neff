The standard way to use neff on the client & the server side is to check for enabled features using the `isEnabled()` API.

```javascript
var users = 500;
var moreUsers = neff.isEnabled("moreUsers");
if (moreUsers) {
    users++;
}
```

### Server Side Usage

#### NConf-Based Config

To get started with neff you'll want to make sure you have setup nconf to contain a `features` setting, which is an object with feature names and either `true` or `false` values. I'm currently doing this using a JSON file and my config looks like this:

```json
"features": {
    "feature1": true,
    "feature2": false
}
```

#### Express Helpers

Neff provides a middleware to add feature flag information to express views. Its usage is:

```
app.use(neff.helpers);
```

The first thing it allows you to do is access feature flags directly in your template such as (dust syntax):

```
{?feature-feature1}
MY HIDDDEN CODE
{/feature-feature1}

```

It also provides a string of class names you can insert into your `<body>` (dust syntax):

```html
<body class="{featureClasses}">
```

#### Express Router Limiter


This route is only available when the feature flag is enabled. Otherwise the user would see a server error page.

```javascript
var neff = require("neff");
app.get("/myhidden/route", neff.limit("feature1"), function(req, res) {
   res.send("You have found feature1!");
});
```

### Client Side Usage


Make sure your features are declared on your DOM.

```
<body class="feature-your-feature1 feature-your-feature2">
```

Once you've done that you can take advantage of the flags in 2 ways.


#### CSS

```css
/* Create a block that's hidden by default */
.my-cool-block {
	display: none;
}

/* Display that block when your flag is present */
.feature-your-feature .my-cool-block {
	display: block;
}
```

#### JavaScript

```javascript
define(["neff"], function(neff) {
	if (neff.isEnabled("your-feature")) {
		/* .. */
	}
});
```

### Thanks

Inspired heavily by https://npmjs.org/package/feature-gateway.

