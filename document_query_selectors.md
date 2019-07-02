# Document Query Selectors
- supported in every modern browser from IE9 upwards

### References
https://developer.mozilla.org/en-US/docs/Web/API/Document_object_model/Locating_DOM_elements_using_selectors
https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector
https://developer.mozilla.org/en-US/docs/Web/API/Element/querySelectorAll
https://developer.mozilla.org/en-US/docs/Web/API/NodeList
https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors


## One or Many
### return the first descendent of the baseElement which matches the selector(s), or null:
  `element = baseElement.querySelector(selectors);`

### return a NodeList containing all matching Elements, or an Empty NodeList:
  `elementList = parentNode.querySelectorAll(selectors);`
- this is not an array, but `forEach()` will work everywhere except IE
- to create a true array
  `Array.from(NodeList)`
- **care**:
  - `document.querySelectorAll('selectorString')` does not return a live NodeList so changes in the DOM will not affect the collection, and changes you make to elements selected in this way will not affect the DOM
  - if you use CSS pseudo elements in your selector string then this will always return an empty NodeList although CSS pseudo classes can be used

## Selector Syntax
### Any valid CSS selector string can be used
- class: `baseElement.querySelectorAll('.classname')`
- id: `baseElement.querySelector('#idname')`
- type: `baseElement.querySelectorAll('nodename');`
- tag name: `baseElement.querySelectorAll('div')`
- attribute: `baseElement.querySelector('style:not([type])");`
- attribute value: `baseElement.querySelectorAll('[data-name="value"]')'`
- CSS pseudo class:`baseElement.querySelectorAll(':nth-child(4n)')`
- descendants of an element: `baseElement.querySelectorAll('#parentElementId childElementType')`
- and lots more

### combining selectors
- selectors separated by a comma are treated as `or` for matching
  - return all `<div>` elements and all `<span>` elements
  `baseElement.querySelectorAll('div, span')`
- descendants
  - return only `<span>` elements which have a parent of `<div>`
  `baseElement.querySelectorAll('div span')`
  - return only `<span>` elements that are the immediate child of a `<div>`
  `baseElement.querySelectorAll('div > span')`
- siblings
  - return only `<span>` elements which follow a `<p>` and have the same parent element
  `baseElement.querySelectorAll('p ~ span')`
  - return only `<span>` elements which immediately follow a `<p>` and have the same parent element
  `baseElement.querySelectorAll('p + span')`


- return the first <input> element with the name "login" (<input name="login"/>) located inside a <div> whose class is "user-panel main" (<div class="user-panel main">) and whose immediate parent element is <p> in the document:
  `var el = document.querySelector("div.user-panel.main input[name='login'] > p");`




### Other ways to select Elements
- `document.getElementById()` :
  https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementById
- `document.getElementsByTagName()` :
  https://developer.mozilla.org/en-US/docs/Web/API/Element/getElementsByTagName
- `document.getElementsByClassName()` :
  https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementsByClassName
