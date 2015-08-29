## How to use

### 1. Smooth scroll within your page

If Zenscroll is included in your page it will automatically animate the scrolling to anchors on the same page. This works even with content you dynamically load via ajax, as Zenscroll uses a generic click handler.

Since this is implemented a progressive enhancement, all internal links still work even in very old browsers. Note that internal links are intentionally not added to the history to save the users from having to hit the Back button too many times afterwards.

If you want some links to be excluded from this, then start with the path of the page. E.g., instead of writing `<a href="#about">` write  `<a href="/#about">`. 

You can also disable this automatic smoothing for all internal links by executing:

````js
zenscroll.setup(null, null, true)
````


### 2. Scroll to the top of an element

````js
var about = document.getElementById("about")
zenscroll.to(about)
````

Note that Zenscroll intentionally leaves a few pixels (by default 9px) from the edges of the screen or scolling container. If you have a fixed navigation bar or footer bar then you probably need more than that. Or you may want to set it to zero. You can globally override the default value by calling `zenscroll.setup()` (see below) or with the `edgeOffset` parameter of the constructor when you create a scroller for a DIV.

### 3. Scroll to a specific vertical position

````js
zenscroll.toY(123)
````

### 4. Scroll an element into view 

If the element is already fully visible then no scroll is performed. Otherwise Zenscroll will try to make both top & bottom of element visible, if possible. If the element is higher than the visible viewport then it will simply scroll to the top of the element. 

````js
zenscroll.intoView(image1)
````

Tip: If you resize an element with a transition of 500ms, you can postpone calling zenscroll with that amount of time:

````js
image.classList.remove("is-small")
setTimeout(function () { 
    zenscroll.intoView(image2) 
}, 500)
````


### 5. Scrolls the element to the center of the screen

````js
zenscroll.center(image2)
````

If you want you can also define an offset. The top of the element will be upwards from the center of the screen by this amount of pixels. (By default offset is the half of the element’s height.)

````js
var duration = 500 // miliseconds
var offset = 200 // pixels
zenscroll.center(image2, duration, offset)
````

Note that a zero value for offset is ignored. You can work around this by using `zenscroll.toY()`.

### 6. Set the duration of the scroll

The default duration is 999 which is ~1 second. The duration is automatically reduced for elements that are very close. You can specifically set the duration via an optional second parameter. (Note that a value of zero for duration is ignored.)

Examples:

````js
zenscroll.toY(70, 100) // 100ms == 0.1 second
````

````js
zenscroll.to(about, 500) // 500ms == half a second
````

````js
zenscroll.center(image2, 2000) // 2 seconds
````

### 7. Scroll inside a scrollable DIV

Anything you can do within the document you can also do inside a scrollable element. You just need to instantiate a new scoller for that element.

Example:

````html
<div id="container">
  <div id="item1">ITEM 1</div>
  <div id="item2">ITEM 2</div>
  <div id="item3">ITEM 3</div>
  <div id="item4">ITEM 4</div>
  <div id="item5">ITEM 5</div>
  <div id="item6">ITEM 6</div>
  <div id="item7">ITEM 7</div>
</div>

<script>
  var c = document.getElementById("container")
  var defaultDuration = 500
  var edgeOffset = 4
  var cScroll = zenscroll.newFor(c, defaultDuration, edgeOffset)
  var target = document.getElementById("item4")
  cScroll.center(target)
</script>
````

Obviously you can use all other scroll methods and parameters with the scrollable container. Two more examples:

````js
cScroll.toY(35)
````

````js
cScroll.intoView(target, 750)
````

### 8. Change settings

It’s easy to change the basic parameters of scrolling:

- You can set the default value for duration. This will be valid for internal scrolls and all your direct scroll calls where you don’t specify a duration.
- Change the edge offset (the spacing between the element and the screen edge). This is very useful if you have a fixed navigation bar or footer bar: set it to at least the height of your fixed bar.
- You can also turn on or off the automatic smoothing for internal links.

````js
var defaultDuration = 777
var edgeOffset = 42
var disableInternalLinkSmoothing = false
zenscroll.setup(defaultDuration, edgeOffset, disableInternalLinkSmoothing)
````

If you don’t want to change a value just omit the parameter or pass `null`. For example, the line below sets the default duration, while leaving other settings unchanged:

````js
zenscroll.setup(500)
````

Sets the the spacing between the edge of the screen (or a DIV) and the target element you are scrolling to, while leaving the default duration unchanged:

````js
zenscroll.setup(null, 5)
````

Disable the automatic smoothing for internal links, while leaving both other settings unchanged:

````js
zenscroll.setup(null, null, true)
````


### 9. Controlling the smooth operation

To check whether a scoll is being performed right now:

````js
var isScrolling = zenscroll.moving()
````

To stop the current scrolling:

````js
zenscroll.stop()
````