raja-polyfill
=============

Express-dom plugin that installs <script> tags polyfills depending on user agent;
and emitting Vary directive for proxy cache.


```
var app = require('express')();
var dom = require('express-dom');
var polyfill = require('raja-polyfill');
app.get('/mypage', dom('myview').use(polyfill({
	"classlist": "https://raw.githubusercontent.com/eligrey/classList.js/master/classList.js",
	"custom-elements": "/js/webcomponents-lite.js"
})));
```

The polyfill plugin accepts an object where keys are caniuse-db features,
and values are the url that go in script tags.

As shown in the example above, what is actually loaded in the script tag does
not necessarily match exactly the tested feature: here we just test
custom-elements to install [many more polyfills](https://github.com/webcomponents/webcomponentsjs/blob/master/webcomponents-lite.js#L53).



using with a cache
------------------

raja-polyfill sets response header Vary: User-Agent so that proxies can
properly cache responses by User-Agent.

Better caching strategy can be achieved with User Agent version negotiation:
raja-polyfill sets this header in the response:
User-Agent-Versions: ie < 9, ff < 43, chrome < 10, opera < 22
along with Vary: User-Agent-Versions of course.

This allows the cache to compare user-agent version with the ranges given
in User-Agent-Versions for each url variant.

