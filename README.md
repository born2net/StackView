Backbone.StackView
====

Easy to use, powerful, Single Page App View manager
---

Backbone.StackView is a Backbone plugin for managing views using sliding, popup and fade transitions between Backbone views.
It has a simple programming interface that allows a developer to manage all Views in a Single Page Application with ease.

The only library requirements are backbone.js and any version of jQuery.

## Backbone.StackView.ViewPort
A ViewPort is a Backbone.View responsible for rendering other views inside of it (one at a time) and serves as a base class for other Backbone.StackView sub-classes.
Normally you will not instantiate Backbone.StackView.ViewPort directly but instead use one of it's derived classed:

- StackView.Slider
- StackView.Fader
- StackView.Modal

Backbone is an amazing un-opinionated MV* framework. One thing many developers struggle with is how to easily manage the show/hide of Backbone.Views while maintain good clean code; say hello to StackView.
With StackView you simple create normal Backbone.Views and pass in the element id where the StackView will manage its Sub views.
You can have many StackViews in a single application, each controlling different areas of the DOM.

Here is an example of creating a simple StackView.Fader (which will fade in and out of Views)
```javascript
var myStackView = new StackView.Fader({el: '#someElement'});
```

Next you can add Backbone.Views to myStackView as in:

```javascript
var view1 = new BB.View({el: '#somePanel1'});
var view2 = new BB.View({el: '#somePanel2'});
var view3 = new BB.View({el: '#somePanel3'});
myStackView.addView(view1);
myStackView.addView(view2);
myStackView.addView(view3);
```

To select a view using an actual Backbone.view instance simply run:

```javascript
myStackView.selectView(view1);
```
or if you would rather select a view using it's element id you can do that as well:

```javascript
myStackView.selectView('#somePanel1);
```

This will allow you to easily select vanilla Backbone.views, and these views can reside anywhere in the DOM, and StackView will simply hide or show (or slide or popup) the selectedView.




















You can also easily extend one of the StackViews and add your own functionality or override excising methods.
```javascript
/**
 The Core Application StackView between main modules (campaign / resources / settings etc)
 @class AppContentFaderView
 @constructor
 @return {object} instantiated AppContentFaderView
 **/
define(['jquery', 'backbone', 'StackView'], function ($, Backbone, StackView) {

    Backbone.SERVICES.APP_CONTENT_FADER_VIEW = 'AppContentFaderView';

    var AppContentFaderView = Backbone.StackView.Fader.extend({

        /**
         Constructor
         @method initialize
         **/
        initialize: function () {
            Backbone.StackView.ViewPort.prototype.initialize.call(this);
        }
    });

    return AppContentFaderView;
});
```

### `selectView()`
Returns the currently "active" view to be rendered in the view port.

License:
------------------------------------------------------------------------
- The SignageStudio Web Lite and Pepper SDK are available under GPL V3 3 https://github.com/born2net/signagestudio_web-lite/blob/master/LICENSE


