# 1 Avoid interaction with host (browser) objects (DOM, Canvas, WebSQL)

## THE PROBLEM:

- Interaction with host (browser) objects outside the javascript native environment raises unpredictability
- considerable performance lag

## THE SOLUTION:

- Keep the interaction with host objects to an absolute minimum.



# 2 DOM traversal using better Selectors & Store pointer references to in-browser objects (DOM cache)

- If you are not expecting your DOM to change you should store a reference to DOM or jQuery objects you are going to use when your page is created.

## Use ID's for selection

```
// jQuery will need to iterate many times until it finds the right element
var button = jQuery('body div.dialog > div.close-button:nth-child(2)')[0];

// A far more optimized way is to skip jQuery altogether.
var button = document.getElementById('dialog-close-button');

// But if you need to use jQuery you can do it this way.
var button = jQuery('#dialog-close-button')[0];
```

#3 Avoid Giving styles via JavaScript (Toggle Classes instead)

```

// This will incurr 5 screen refreshes
jQuery('#dialog-window').width(600).height(400).css('position': 'absolute')
                       .css('top', '200px').css('left', '200px');

// Let jQuery handle the batching
jQuery('#dialog-window').css({
     width: '600px',
     height: '400px',
     position: 'absolute',
     top: '200px',
     left: '200px'
);

// Or even better use a CSS class.
jQuery('#dialog-window').addClass('mask-aligned-window');

```

# 4 Use buffered DOM inside scrollable DIVs.
This is an extension of the fourth point above (Keep HTML super-lean), you can use this technique to remove items from DOM that are not being visually rendered on screen, such as the area outside the viewport of a scrollable DIV, and append the nodes again when they are needed. This will reduce memory usage and DOM traversal speeds. Using this technique the guys at ExtJS have managed to produce an infinitely scrollable grid that doesn’t grind the browser down to a halt.

# 5 Write code that reduces library dependencies to an absolute minimum.

You can reduce dependency on external libraries by making use of as much in-browser technology as you can, for example you can use document.getElementById('nodeId') instead of jQuery('#nodeId'), or document.getElementsByTagName('INPUT') instead of jQuery('INPUT') which will allow you to get rid of jQuery library dependency.

# 6 Pay special attention event handlers that fire in quick succession (ie, ‘mousemove’).

```
Remember to unbind events when they are not needed.
```

Browser events such as ‘mousemove’ and ‘resize’ are executed in quick succession up to several hundred times each second, this means that you need to ensure that an event handler bound to either of these events is coded optimally and can complete in less than 2-3 milliseconds.

Any overhead greater than that will create a patchy user experience, specially in browsers such as IE that have poor rendering capabilities.

# 7 Take advantage of reference types.
JavaScript, much like other C-based languages, has both primitive and reference value types. Primitive types such as strings, booleans and integers are copied whenever they are passed into a new function, however reference types such as arrays, objects and dates are passed only as a light-weight reference.You can use this to get the most performance out of recursive functions, such as by passing a DOM node reference recursively to minimise DOM traversal, or by passing a reference parameter into a function that executes within an iteration. Also, remember that comparing object references is far more efficient than comparing strings.