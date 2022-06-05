# Caching

- web cache is data from a website that a computer has temporarily saved for quick and simple future access (images, html, css, js, multimedia files etc)
- it can be stored anywhere between your server and the customer's browser: the closer to the browser the greater the speed benefit to that customer
- caching lowers
  - network costs
  - load times
- caching risks serving outdated content if the browser cannot validate cached content as fresh/stale
- caching risks exposing sensitive or personal data if user data is cached in anything other than a `private cache` which is often browser-based

## Application

- HTML5 provided application cache, making a web application accessible without an internet connection - advantages
  - Offline browsing - users can use the application when they're offline
  - Speed - cached resources load faster
  - Reduced server load - the browser will only download updated/changed resources from the server
- disadvantages
  - it's deprecated and didn't work terribly well <https://alistapart.com/article/application-cache-is-a-douchebag/> (mdn suggests using the cache api instead <https://developer.mozilla.org/en-US/docs/Web/API/Cache>)

## HTTP (aka site/page cache)

- a system that temporarily stores data when a web page is loaded for the first time
- caching is done from the client side
- the site can specify when the cached data should expire (time-based or update-based)
- a `browser cache` is a kind of site-cache: the end-user can clear the cache at will
- HTTP response headers are commonly used for caching
- HTTP is designed to cache as much as possible, so even if no Cache-Control is given, responses will get stored and reused if certain conditions are met. This is called heuristic caching.
- potential risks
  - avoid using the `no-store` cache directive as you lose the browser's back/forward history: instead use `no-cache` in combination with `private`
  - there is basically no way to delete responses that have already been stored with a long max-age: caching reduces access to the server, which means that the server loses control on that URL (no more requests reach the server due to caching at the browser or intermediate server caches) *unless* you use a managed cache like a CDN or service worker <https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API>
  - any stored response will remain for its max-age period unless the user manually performs a reload, force reload, or clear-history action

## Server

- a system that temporarily stores data on the server side, with no involvement from either the end-user or browser (sometimes considered to include CDNs, proxies & load balancers)
- examples
  - object caching: stored database queries
  - cdn caching: the server geographically closest to the end user serves the data, for faster response times
  - opcode caching: phpcode is precompiled & cached
