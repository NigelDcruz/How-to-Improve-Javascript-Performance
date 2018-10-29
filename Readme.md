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
