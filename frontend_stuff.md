# Frontend - Stuff around web requests, rendering, scripts/images

## Web requests - how they work

Sources
<https://flaviocopes.com/http-request/>
<https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/How_the_Web_works>

- level 1 explanation
  - client(personal device) sends request to server
  - server(machine storing data) sends response to client

- level 2 explanation
  - you enter a url / click on a link / whatever
  - the browser goes to the DNS server, and finds the real address of the server matching the url
  - browser sends an HTTP request message to the server, asking it to send a copy of the website to the client (you go to the shop and order your goods). This message,
    - this and all other data sent between the client and the server, is sent across your internet connection using TCP/IP.
  - if the server approves the client's request, the server sends the client
    - a "200 OK" message
    - the website's files to the browser as a series of small chunks called data packets
  - the browser assembles the small chunks into a complete web page and displays it to you (see rendering!)

## Rendering

### Critical Path Rendering aka what happens when

Two separate tree structures:

- html
  - data arrives at the browser in packets of bytes
  - bytes are converted into characters based on the html-encoding
  - characters are converted into units of parsing: tokens, then into nodes
  - the nodes are linked to form the Document Object Model (a tree structure)
- css
  - just like html: bytes->characters->tokens->nodes
  - not a DOM this time, a CSS Object Model (CSSOM)

The DOM and CSSOM are combined to make the Render Tree, which is used to calculate the layout - the position and size of each element on the screen
The computed layout is used to paint the elements i.e. display them to the user

JavaScript adds a whole other complication as it can alter both the DOM and the CSSOM:

- when the browser encounters a `<script>` tag, rendering is halted until the script has finished executing i.e. DOM waits for script **unless** the script is tagged `async`
- the converse is true for css: the JS execution waits for the CSSOM

### Client vs server-side rendering

Article from 2019 does a good job of explaining things <https://web.dev/rendering-on-the-web/>

SSR: Server-Side Rendering - rendering a client-side or universal app to HTML on the server
CSR: Client-Side Rendering - rendering an app in a browser, generally using the DOM
Rehydration: 'booting up' JavaScript views on the client to reuse the server-rendered HTML’s DOM tree and data
Prerendering: running a client-side application at build time to capture its initial state as static HTML

The best option depends on your use case - as always, it depends on where your bottlenecks are

- Server side
  - Server Rendering: dynamic content is possible, but it's slower (adding html caching can speed things up a lot, as can service worker caching in a )
  - Static Rendering: fastest option, but only for content that doesn't need to be personalised/customised
- Client side
  - rendering pages directly in the browser using JS can be hard to keep performant especially on mobile, particularly as the amount of JS required grows in proportion to the application
- Universal Rendering
  - uses both Server Rendering and Client-side Rendering
  - server rendering means the page becomes visible more quickly as navigation requests are handled server-side
  - rehydration on the client side then adds interactive content via a second rendering
  - this looks good but can be a very frustrating user experience as none of the interactive functionality is available until after the client side rendering has completed
- Streaming Server Rendering
  - html is sent in chunks which can be rendered progressively e.g. React's `renderToNodeStream()` async process
- Service workers
  - use streaming server rendering for initial/non-JS navigations, and then have the service worker take on rendering of HTML for navigations after it has been installed
  - probably worth exploring: <https://developer.chrome.com/docs/workbox/service-worker-overview/>

## Scripts/images - the big, the bad, the ugly

images: resize and compress, then use lazy loading to speed up the before-the-fold page load time & reduce data/bandwidth for those who never actually scroll down <https://css-tricks.com/the-complete-guide-to-lazy-loading-images/>

scripts: js execution blocks DOM rendering, CSSOM construction blocks js execution
The bigger the js bundle, the slower the page load, especially on mobile, and some pure js sites may be completely non-functional in low-data conditions
