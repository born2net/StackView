Backbone.StackView
====

Create a single Page Application with Backbone.StackView
---

Backbone.StackView is a Backbone plugin for managing views and view transitions.
It has a simple programming interface that allows a developer to manage all Views in a Single Page Application with ease.

The only requirements are backbone.js and any version of jQuery.

## Backbone.StackView.ViewPort

A ViewPort is a Backbone.View responsible for rendering other views inside of it (one at a time) and serves as a base class for other Backbone.StackView sub-classes.
Normally you will not instantiate Backbone.StackView.ViewPort directly but instead use on of it's derived classed:
- StackView.Slider
- StackView.Fader
- StackView.Modal


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


=======
StackView
=========

Backbone.js Single Page Application manager of Views allowing the show/hide of a Backbone View.
>>>>>>> remotes/origin/master
