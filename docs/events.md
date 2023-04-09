# Events

Infinite Ajax Scroll provides a wide range of events which we can use to hook into its functionality and customize where needed.

## Working with events

You can bind and unbind to events with the `on`, `once` and `off` methods.

```javascript
let ias = new InfiniteAjaxScroll('.container', options);

function handler(event) {
  console.log('New items have been appended', event);
}

ias.on('appended', handler);
ias.off('appended', handler);
ias.once('appended', handler);
```

## Reference

### ready

This event is triggered when the DOM is ready. Right after this event Infinite Ajax Scroll will bind (unless the [`bind`](./options.md#bind) option is set to `false`).

### binded

This event is triggered when Infinite Ajax Scroll binds to the scroll and resize events of the scroll container. This mostly happens right after the DOM is ready, but this can be configured with the `bind` option.

### unbinded

Triggered when Infinite Ajax Scroll removed its listeners from the scroll and resize events.

Infinite Ajax Scroll will only unbind when we call the `unbind` method.

### scrolled

Triggered when the user scrolls inside the scroll container.

| property | type | description |
| :--- | :--- | :--- |
| scroll.y | integer | Current vertical scroll position from the top of the page (can be negative) |
| scroll.x | integer | Current horizontal scroll position from the left of the page (can be negative) |
| scroll.deltaY | integer | Delta between current vertical scroll position and previous position |
| scroll.deltaX | integer | Delta between current horizontal scroll position and previous position |

The delta values can be used to determine the scroll direction. A positive value means scrolling down (deltaY) or to the right (deltaX). A negative value means the opposite direction.

### resized

Triggered when the user resizes the scroll container.

| property | type | description |
| :--- | :--- | :--- |
| scroll | object | Object with x y coord of the current scroll position |

### hit

Triggered when the user has hit the scroll threshold for the next page due to scrolling or resizing.

| property | type | description |
| :--- | :--- | :--- |
| distance | int | The distance to the scroll threshold in pixels |

### top

*Introduced in Infinite Ajax Scroll 3.1.0*

Triggered when the user has hit the top of the scroll area for the previous page due to scrolling or resizing.

| property | type | description                            |
| :--- | :--- |:---------------------------------------|
| distance | int | The distance to the scroll top in pixels |

### next

Triggered right after the `hit` event. Indicating that the next page will be loaded.

| property | type | description |
| :--- | :--- | :--- |
| pageIndex | int | The page index of the next page (the page that is about to be loaded) |

> pageIndex is zero indexed. This means the index starts at 0 on the first page.

For example to notify the user about loading the next page, you can do:

```js
ias.on('next', function(event) {
  // pageIndex is 0-indexed, so we add 1
  alert(`Page ${event.pageIndex+1} is loading...`);
});
ias.on('nexted', function(event) {
    alert(`Page ${event.pageIndex+1} is loaded and added to the page.`);
});
```

### nexted

Trigger when loading and appending the next page is completed.

| property | type | description |
| :--- | :--- | :--- |
| pageIndex | int | The page index of the next page (the page that is finished loading) |

### prev

*Introduced in Infinite Ajax Scroll 3.1.0*

Triggered right after the `top` event. Indicating that the previous page will be loaded.

| property | type | description                                                           |
| :--- | :--- |:----------------------------------------------------------------------|
| pageIndex | int | The page index of the prev page (the page that is about to be loaded) |

> pageIndex is zero indexed. This means the index starts at 0 on the first page.

For example to notify the user about loading the previous page, you can do:

```js
ias.on('prev', function(event) {
  // pageIndex is 0-indexed, so we add 1
  alert(`Page ${event.pageIndex+1} is loading...`);
});
ias.on('preved', function(event) {
    alert(`Page ${event.pageIndex+1} is loaded and prepended to the page.`);
});
```

### preved

*Introduced in Infinite Ajax Scroll 3.1.0*

Trigger when loading and prepending the previous page is completed.

| property | type | description |
| :--- | :--- | :--- |
| pageIndex | int | The page index of the next page (the page that is finished loading) |

### load

This event is triggered before the next page is requested from the server.

| property | type | default |description |
| :--- | :---  | :--- | :--- |
| url | string | | The url that is about to be requested |
| xhr | XMLHttpRequest | | The configured XMLHttpRequest that is going to be used (see [MDN](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)) |
| method | string | `"GET"` | The request method to use, e.g. "GET", "POST", etc. |
| body | mixed | `null` | Body of the request in case of POST (see [MDN](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/send#Parameters)) |
| nocache | boolean | `false` | Disables cache busting mechanism |
| responseType | string | `"document"` | The expected type of response, eg. "document", "json", etc. (also see [responseType](./options.md#responseType) option) |
| headers | Object | `{'X-Requested-With': 'XMLHttpRequest'}` | Key-value object containing request headers |

You can use this event to modify any of the above properties.

For example to disable the cache busting you can do:

```js
ias.on('load', function(event) {
  event.nocache = true; // prevent IAS from adding a timestamp query param to the url
});
```

### loaded

This event is triggered when the next page is requested from the server, right before the items will be appended.

| property | type | description |
| :--- | :--- | :--- |
| items | array | Array of items that have loaded and will be appended. These items match the selector given in the `next` option |
| url | string | The url that is about to be requested |
| xhr | XMLHttpRequest | The configured XMLHttpRequest that is going to be used |

### error

This event is triggered when an error occurred while loading the next page.

| property | type | description |
| :--- | :--- | :--- |
| url | string | The url that is about to be requested |
| method | string | The method used to fetch the url ("GET", "POST", etc) |
| xhr | XMLHttpRequest | The configured XMLHttpRequest that was used |

### append

This event is triggered before the items are about to be appended.

| property | type | description |
| :--- | :--- | :--- |
| items | array | Array of items that will be appended |
| parent | Element | The element to which the items will be appended |
| appendFn | function | Function used to append items to the container |

See [src/append.js](../src/append.js) for the default append function.

### appended

This event is triggered after the items have been appended.

| property | type | description |
| :--- | :--- | :--- |
| items | array | Array of items that have been appended |
| parent | Element | The element to which the items have been appended |

### prepend

*Introduced in Infinite Ajax Scroll 3.1.0*

This event is triggered before the items are about to be prepended.

| property | type | description                                      |
| :--- | :--- |:-------------------------------------------------|
| items | array | Array of items that will be prepended            |
| parent | Element | The element to which the items will be prepended |
| prependFn | function | Function used to prepend items to the container  |

See [src/prepend.js](../src/prepend.js) for the default prepend function.

### prepended

*Introduced in Infinite Ajax Scroll 3.1.0*

This event is triggered after the items have been prepended.

| property | type | description                                        |
| :--- | :--- |:---------------------------------------------------|
| items | array | Array of items that have been prepended            |
| parent | Element | The element to which the items have been prepended |

### last

Triggered when the last page is appended.

```javascript
ias.on('last', () => {
  console.log('User has reached the last page');
})
```

[Read more on how we can inform the user about reaching the last page](advanced/last-page-message.md)

### first

*Introduced in Infinite Ajax Scroll 3.1.0*

Triggered when the last page is appended.

```javascript
ias.on('first', () => {
  console.log('User has reached the first page');
})
```

### page

Triggered when the user scrolls past a page break. The event provides information about the page in view.

| property | type | description |
| :--- | :--- | :--- |
| pageIndex | int | The page index of the current page |
| url | string | Url of the page |
| title | string | Title of the page |
| sentinel | Element | Sentinel element. Element used to determine on which page the user is |

> pageIndex is zero-based. This means the index starts at 0 on the first page.

One use case for this event is to update the browser url and title:

```javascript
ias.on('page', (event) => {
  // update the title
  document.title = event.title;

  // update the url
  let state = history.state;
  history.replaceState(state, event.title, event.url);
})
```

[View this behaviour in a live demo](https://infiniteajaxscroll.com/examples/articles/)

### prefill

This event is triggered when Infinite Ajax Scroll starts prefill.

```javascript
ias.on('prefill', () => {
  // do something before prefill starts
})
```

### prefilled

This event is triggered when prefill is finished.

```javascript
ias.on('prefilled', () => {
  // do something when prefill finished
})
```

