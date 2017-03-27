# Web Development

## CRUD

* **Forms**

  * The **action** attribute tells the browser what url to post the data to.  Forms that don't provide an action attribute will post/get to/from the same page.

  * The **method** attribute tells the browser which HTTP verb to use.

  * The **name **attribute of each input is the name of the field. Its value is the key to the key/value pair in the request body, the value of that pair being the value of the input field.

  * The submit button tells the browser that the form data should now be submitted.

## Rendering

1. The browser creates the **DOM** \(a tree based object with standards and APIs for object manipulation\) from the **HTML** \(_one_ language for creating a DOM tree\) it receives. \(The HTML can come from the server, or from a script. This is the difference between server-side and client-side rendering\).
2. Styles \(**CSS**\) are loaded and parsed.
3. The DOM, minus invisible elements \(like the `<head>` tag, or elements with `display: none;`\) plus the styles, form the **render tree**. This is a list of DOM objects to be rendered on screen.
4. Coordinates are calculated for each object in the render tree. This creates the **layout** of the page.
5. Finally, all of this is displayed in the browser. This process is called **painting**.

6. Changes to the DOM should be reflected in the window, so a re-render occurs when the DOM changes. If the layout or content isn't changed \(background-color, color, visibility, etc.\), then the browser simply **repaints** the page. Otherwise \(content changing, window resizing with adaptive layouts, etc.\), it **reflows** which computes the layout and position of the element, all of its children, and every element after it. It then calls a final repaint. Reflows are expensive.

## Event Loop

## [Storage](http://ejohn.org/blog/dom-storage/)

* **Cookie**: A small text file containing key/value pairs that lives on your computer to carry information from one session to another. \(Remember the email, preferred font size, items in a shopping cart, etc.\). By default they are destroyed when the browser is closed, but are often set to persist longer than that. Only the root domain that created the cookie can retrieve it. Though, third party advertisers can store a cookie, and if used by another site can retrieve it to serve relevant ads. Only strings can be stored.
* **localStorage**: Same as cookies but you can also store javascript primitives, but not objects or arrays \(though you could JSON serialize them\). This is also primarily used for client-side reading and writing. Whereas cookies are typically for the server to interact with. So the question is, who needs the data: The client or the server?
* **sessionStorage**: Same as localStorage, but it doesn't persist tab to tab, window to window, or browser to browser.
* **session**: Used to store data on the server instead of the client. 
* **Cache**: A folder full of temporarily stored static files \(HTML documents, images, scripts, etc.\) to improve page loading speed and reduce bandwidth. If a file is cached on a user's computer, then they don't have to download it from the server when they need it again.  

## Responsive Design

* [9 tenants of responsive design.](http://blog.froont.com/9-basic-principles-of-responsive-web-design/)
* Flexbox [containers](https://medium.freecodecamp.com/an-animated-guide-to-flexbox-d280cf6afc35#.5dz9ogn1v) and [elements](https://medium.freecodecamp.com/even-more-about-how-flexbox-works-explained-in-big-colorful-animated-gifs-a5a74812b053#.5f0sx5o52).

## **Performance**

#### Content Optimization

#### Event Listeners

* When calling methods based on user input, it's usually a good idea to either debounce or [throttle](http://pastebin.com/igUFpKUQ) the method calls, so as not to overload your resources.

#### [Rendering](http://blog.letitialew.com/post/30425074101/repaints-and-reflows-manipulating-the-dom)

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



