# Web Development

## CRUD

* **Forms**

  * The **action** attribute tells the browser what url to post the data to.  Forms that don't provide an action attribute will post/get to/from the same page.

  * The **method** attribute tells the browser which HTTP verb to use.

  * The **name **attribute of each input is the name of the field. Its value is the key to the key/value pair in the request body, the value of that pair being the value of the input field.

  * The submit button tells the browser that the form data should now be submitted.

* Validation

  * Make sure that server-side validation at least mirrors client side validation.

## Rendering Process

1. The browser creates the **DOM** \(a tree based object with standards and APIs for object manipulation\) from the **HTML** \(_one_ language for creating a DOM tree\) it receives. \(The HTML can come from the server, or from a script. This is the difference between server-side and client-side rendering\).
2. Styles \(**CSS**\) are loaded and parsed.
3. The DOM, minus invisible elements \(like the `<head>` tag, or elements with `display: none;`\) plus the styles, form the **render tree**. This is a list of DOM objects to be rendered on screen.
4. Coordinates are calculated for each object in the render tree. This creates the **layout** of the page.
5. Finally, all of this is displayed in the browser. This process is called **painting**.

6. Changes to the DOM should be reflected in the window, so a re-render occurs when the DOM changes. If the layout or content isn't changed \(background-color, color, visibility, etc.\), then the browser simply **repaints** the page. Otherwise \(content changing, window resizing with adaptive layouts, etc.\), it **reflows** which computes the layout and position of the element, all of its children, and every element after it. It then calls a final repaint. Reflows are expensive.

## Event Loop

1. Javascript is single threaded. == One call stack \(Stack structure that records where in the program you are\). == It can do one thing at a time. So each function that's called is pushed onto this one stack. The longer a function takes \(because of its depth, network requests, image processing, etc\), the longer it takes before the next line of code can be executed.
2. The browser has a **render queue**, which waits for an empty call stack before dequeuing a rerender; therefore if your call stack is constantly **blocked**, your render queue rarely dequeues. 
3. The fix for this is to use asynchronous code with tasks that take longer. Instead of being pushed onto the call stack, asynchronous functions go into the **event queue**, which also waits for the call stack to empty before dequeuing a task into the call stack.
4. Asynchronous code waits out of the way - via some web api - and once it's ready \(the data arrived, the countdown is done, etc\) it adds the callback to the event queue. 

## [Storage](http://ejohn.org/blog/dom-storage/)

* **Cookie**: A small text file containing key/value pairs that lives on your computer to carry information from one session to another. \(Remember the email, preferred font size, items in a shopping cart, etc.\). By default they are destroyed when the browser is closed, but are often set to persist longer than that. Only the root domain that created the cookie can retrieve it. Though, third party advertisers can store a cookie, and if used by another site can retrieve it to serve relevant ads. Only strings can be stored.
  * **session**: A type of cookie, used to store data on the server instead of the client. Some info is sent from the client, which is used to unlock other information on the server.
* **localStorage**: Same as cookies but you can also store javascript primitives, but not objects or arrays \(though you could JSON serialize them\). This is also primarily used for client-side reading and writing. Whereas cookies are typically for the server to interact with. So the question is, who needs the data: The client or the server?
* **sessionStorage**: Same as localStorage, but it doesn't persist tab to tab, window to window, or browser to browser.
* **Cache**: A folder full of temporarily stored static files \(HTML documents, images, scripts, etc.\) to improve page loading speed and reduce bandwidth. If a file is cached on a user's computer, then they don't have to download it from the server when they need it again.  

## Routing

* User clicks a link: `<a href="/hello">Hello</a>`
* On a webapp that uses **server side routing**:
  * The browser detects that the user has clicked on an anchor element.
  * It makes an HTTP GET request to the URL found in the `href` tag.
  * The server processes the request, and sends a new document \(usually HTML\) as a response
  * The browser discards the old webpage altogether, and displays the newly downloaded one.
* If the webapp uses **client side routing**:

  * The browser detects that the user has clicked on an anchor element, just like before.
  * A client side code \(usually the routing library\) catches this event, detects that the URL is not an external link, and then **prevents the browser **from making the HTTP GET request.
  * The routing library then  **manually changes the URL **displayed in the browser \(using the HTML5 history API, or maybe URL hashbangs on older browsers\)
  * The routing library then **changes the state of the client app. **For example, it can change the root component according to the route rules.

  * The app then processes state changes. It renders the new components, and if necessary, it requests new data from the server. But this time the response isn't necessarily an entire webpage, it may also be "raw" data, in which case the client-side code turns it into HTML elements.

* There are several upsides of client-side routing: you download less data to display new content, you can reuse DOM elements, display loading notifications to user etc. However, webapps that generate the DOM on server side are much easier to crawl \(by search engines\), thereby making SEO optimization easier. You should know where you want your app to use either.

## Responsive Design

* [9 tenants of responsive design.](http://blog.froont.com/9-basic-principles-of-responsive-web-design/)
* Flexbox [containers](https://medium.freecodecamp.com/an-animated-guide-to-flexbox-d280cf6afc35#.5dz9ogn1v) and [elements](https://medium.freecodecamp.com/even-more-about-how-flexbox-works-explained-in-big-colorful-animated-gifs-a5a74812b053#.5f0sx5o52).
* Use `box-sizing: border-box;` to make things easier when using responsive design.
* An [SVG](https://teamtreehouse.com/library/svg-basics) is an alternative to a raster photo. Typically used for icons and other simple images.

## Animations

* [Transition syntax](https://developer.mozilla.org/en-US/docs/Web/CSS/transition)
* [Keyframe & Animation syntax.](http://blog.teamtreehouse.com/css3-animation-demystified)
* [Pre-built animations.](http://animista.net)

## React

* [Stateless functional components when you can](https://hackernoon.com/react-stateless-functional-components-nine-wins-you-might-have-overlooked-997b0d933dbc).
* [Virtual DOM Benefits](http://stackoverflow.com/questions/21109361/why-is-reacts-concept-of-virtual-dom-said-to-be-more-performant-than-dirty-mode) 
* Two types of presentation components: logical and pure:
  * Logical presentation components manage their own internal state.
* * Pure **stateless** components simply present the data given to them via props.
  * Pure stateless components should be the goal, being the simplest, and in the future the most performant \(skipping certain checks\).
* Remember to break your components down as far as they should be. Look at the name of the component. Does everything within the render method belong to that name? For example, don't have the header and footer of the page in the `<ProductList />` component. Give them their own.

## Redux

* First questions when implementing redux: What state should be placed at the application level, vs the component level. Look at any components that manage or manipulate state, and ask these questions to get the answer:
  * Application State: 
    * Is the data already very high up in the component tree? 
    * Is the data coming from some already globally accessible data? Singleton, repository pattern, etc?
  * Component State:
    * Is the state tightly coupled with the component and has no dependencies on any other components/props?
    * Is the data only used in this one component? 
* Next, for all state data that you determined should be stored at the application level, what methods currently manipulate that state? You will define those methods as string constants. Make a folder in the same directory as your components folder called `actionTypes`, and add js files for each state object. For example, for jobs, you'll have `job.js`, and inside you'll have `export const ADD_JOB = 'job/ADD`\_`JOB';` ddsfsdf
* 
* ## Dev Tools
* Measuring Performance

  * Go to the network tab and check **Disable Cache**. Load the page again.
    * The status bar at the bottom shows you the total number of requests, total size of the page, and how long it took to download.
  * * In the Timeline or Waterfall section, sort by "Duration".
  * Go to the google pagespeed site, or download the chrome extension.

## **Performance**

#### Content Optimization

* Are there any 404 errors? Are all of the included assets necessary?
* Have you decided on performance budgets? Speed to load? Total page size?
* The first thing you should look at is the size of the assets. 
  * Images
    * Are they optimized for the needs of the users? 
    * Was the right sized image requested for the screen? Are you using thumbnails when smaller images are needed instead of loading the full image? \(Twice the size of the actual image in pixels for retina screens.\)
    * Is the quality of that image higher than needed? 
    * Are you using sprites for small images?
    * Are you using SVGs for anything that isn't a photo? \(Usually icons\) And [bundling](https://teamtreehouse.com/library/front-end-performance-optimization/combine-and-minify-assets/create-a-sprite-map) those SVG images.
  * Are you bundling and minimizing your file requests? 
* Any time you're requesting an outside resource \(a record from a database, a file, image, or video from another server, whatever\) it should be done asynchronously.
* Are you using a CDN  for pages that have to load a large **number** of assets? Browsers limit the amount of concurrent requests per server, CDNS spread your requests to many servers, allowing more concurrent requests.
* Are your files optimized for caching? 

#### Event Listeners

* When calling methods based on user input, it's usually a good idea to either debounce or [throttle](http://pastebin.com/igUFpKUQ) the method calls, so as not to overload your resources.

#### [Rendering](http://blog.letitialew.com/post/30425074101/repaints-and-reflows-manipulating-the-dom) [1](https://developers.google.com/web/fundamentals/performance/rendering/)

* **React**: Instead of having conditional styles to display if a condition is true, simply use a conditional to decide whether to even render the element.

* **React**: Should we be using arrow functions and binds when not necessary? See Airbnb React style guide.

* Avoid setting multiple inline styles; avoid setting styles individually.  These trigger a reflow for each dynamic style change.

* Use classNames of elements, and do so as low in the DOM tree as possible.  Changing the class attribute lets you apply multiple styles to an element with a single reflow.  But since this reflows all the element’s children, that means you don’t want to change the class on a wrapper div if you’re only targeting its first child.

* Batch your DOM changes and perform them “offline”.  "Offline" means removing the element from the active DOM \(e.g.$.detach\(\)\), performing your DOM changes and then re-appending the element to the DOM.

* Avoid computing styles too often.  If you must then cache those values.  E.g. store  var offsetHt = elem.offsetHeightonce instead of calling it multiple times.

* Apply animations with position fixed or absolute.  So the animation doesn’t affect the layout of other elements.

* Stay away from table layouts, they trigger more reflows than block layouts because multiple passes must be made over the elements.

## Misc

* The `window` object contains all global variables and methods \(which is why typing `window` is optional\), and the tab itself. You use it sometimes because there may be other variables/methods with the same name. Like namespacing.
* The `document` is the actual DOM object.
* Closure issue: i equals 3 WHEN you CALL it. The closure is using the most current value, which is the end of the loop.

```js
var funcs = [];
for (var i = 0; i < 3; i++) {
  funcs[i] = function() {
    console.log("My value: " + i); // when this is finally executed, i == 3
  };
}
for (var j = 0; j < 3; j++) {
  funcs[j]();
}

// solution:

var funcs = [];

function createfunc(i) {
    return function() { console.log("My value: " + i); }; // now "i" is bound to this copied value.
}

for (var i = 0; i < 3; i++) {
    funcs[i] = createfunc(i);
}

for (var j = 0; j < 3; j++) {
    funcs[j]();
}
```



