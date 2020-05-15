## About

A pair of modules meant to ease the transition
[off of AppCache](https://alistapart.com/article/application-cache-is-a-douchebag/)
and on to
[service workers](https://developers.google.com/web/fundamentals/primers/service-workers/).

Note: These libraries attempt to replicate the caching and serving behavior that AppCache offers, but does **not** include direct equivalents to the [`window.applicationCache` interface](https://developer.mozilla.org/en-US/docs/Web/API/Window/applicationCache), nor the related events that AppCache would fire in the `window` context.

## Installation

There are two modules to install: one that is used from within the `window`
context in your web app, and the other that's used in the context of your
service worker.

```sh
npm install --save-dev appcache-polyfill-window
npm install --save-dev appcache-polyfill-sw
```

As an alternative to local installation & serving, you can load both libraries from a NPM CDN, like https://unpkg.com/ or https://www.pika.dev/.

## Usage

### Window client

```html
<script type="module">
  import {init} from '/path/to/appcache-polyfill-window/build/index.modern.js';

  // Optional: define a callback that runs whenever caches are updated.
  // This is *rough* replacement for listening for AppCache updates.
  function myCachePopulatedCallback(urls) {
    // urls is an array of updated URLs
    // Your logic goes here.
  }

  init({
    cachePopulatedCallback: myCachePopulatedCallback,
  }).then(() => navigator.serviceWorker.register('sw.js'));
</script>
```

### Service worker client

```js
importScripts('/path/to/appcache-polyfill-sw/build/index.umd.js');

self.addEventListener('fetch', (event) => {
  // Alternatively, examine event.request and only use the
  // appcachePolyfill.handle() logic for a subset of requests.
  event.respondWith(appcachePolyfill.handle(event));
});
```

## Feedback

Please [open an issue](https://github.com/googlechromelabs/sw-appcache-behavior/issues) with feedback or bug reports if you run in to problems.
