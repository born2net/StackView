Backbone.StackView
====

Easy & powerful, Single Page App View manager
---

Backbone.StackView is a Backbone plugin for managing views using sliding, popup and fade transitions between Backbone views.
It has a simple programming interface that allows a developer to manage all Views in a Single Page Application with ease.

The only library requirements are backbone.js and any version of jQuery.

## Demo:
I built a real SPA (Single Page Application) using the StackView class.
In fact, it's a really nice boilerplate Backbone application that uses everything one needs in an App including (of course) StackViews, bootstrap (supports mobile scaling) Login, Localization, Cookie and a lot more.

View the Demo StackView app here:
https://secure.digitalsignage.com/boilerplate/public/index.html

For Backbone Boiler application source code go here:
https://github.com/born2net/Backbone-boilerplate-and-StackView


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
myStackView.selectView('#somePanel1');
```

This will allow you to easily select vanilla Backbone.views, and these views can reside anywhere in the DOM (if you are using Fader) and StackView will simply hide or show the selected Backbone.View.
Now what is so cool (and may not be evident right away when reading the docs) is that because you can select views via a View's instance (such as 'this' or someViewInstanceMember) as well as via its element id (#someID) you don't have to pass references to other sections of the code in order to select the view.
In other words, what I normally do in a SPA, is I register a StackView as a service (using the ComBroker) and other parts of the code ask for that service.
Once they get an instance of the StackView service, they can simply call on it to select a view using a hard coded element id as in:

```javascript
// get a the StackView service anywhere inside your app
self.m_stackFaderView = BB.comBroker.getService(BB.SERVICES['SCENE_FADER_VIEW']);
...
// Select a particular view, so easy ... and keeps with separation of concern
self.m_stackFaderView.selectView('#someID');

},

StackView base class holds a list of all Views is manages so you can always ask for a view by its ID. StackView will also fire an event when a View is selected and pass in the even the selected view instance so subscribers can handle the event.
Here is an example of a Backbone subscriber that's interested only in SELECTED_STACK_VIEW event that are "intended for him":

```javascript
listenWhenIamBroghtIntoView: function () {
   var self = this;
   self.listenTo(someStackView, 'SELECTED_STACK_VIEW', function (e) {
       if (e == self)
           self.render();
   });
   ...
},
```

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

And as mentioned earlier, a Backbone.StackView comes in three flavours, Fade, Slide and Popup. You can watch all 3 in the demo SPA link.

Here is an example of creating a popup modal view.
```javascript
_initModal: function () {
    var self = this;
    var popModalView = new Backbone.StackView.Modal({
         el: '#someModalId',
         animation: 'slide_top', //or 'fade' or ...
         bgColor: 'white'
     });
     var aView = new Backbone.View({el: '#someElemebt'});
     popModalView.addView(aView);

     var bView = new Backbone.View({el: '#anotherID'});
     popModalView.addView(bView);

     var cView =  = new Backbone.View({el: '#andAnotherID'});
     popModalView.addView(cView);

     popModalView.selectView(cView);
     // or popModalView.selectView('#andAnotherID');
}

```

Here is an example of StackView.Slider view.
We use constants in place of element IDs just to keep it from referencing it in source code, you don't have to.


```javascript

define(['jquery', 'backbone', 'StackView'], function ($, Backbone, StackView) {

    var ScreenLayoutSelectorView = BB.View.extend({

        /**
         Constructor
         @method initialize
         **/
        initialize: function () {
            var self = this;
            self.m_screens = [];

            self.listenTo(self.options.stackView, BB.EVENTS.SELECTED_STACK_VIEW, function (e) {
                if (e == self) {
                    self._render();
                }
            });

            $(this.el).find(Elements.PREVIOUS).on('click', function (e) {
                if (self.options.from == null)
                    return;
                self.options.stackView.slideToPage(self.options.from, 'left');
                return false;
            });
        }
    });

    return ScreenLayoutSelectorView;
});



define(['jquery', 'backbone'], function ($, Backbone) {

    var ResolutionSelectorView = BB.View.extend({

        /**
         Constructor
         @method initialize
         **/
        initialize: function () {
            var self = this;

            $(this.el).find(Elements.PREVIOUS).on('click', function (e) {
                if (self.options.from == null)
                    return;
                self.options.stackView.slideToPage(self.options.from, 'left');
                return false;
            });
        },

        /**
         Draw the UI for resolution selection
         @method render
         **/
        render: function () {
        }

    });

    return ResolutionSelectorView;

});


...

  var self = this;

  Elements.ORIENTATION_SELECTOR = '#orientationSelector';
  Elements.RESOLUTION_SELECTOR = '#resolutionSelector';
  Elements.SCREEN_LAYOUT_SELECTOR = '#screenLayoutSelector';

 self.m_resolutionSelectorView = new ResolutionSelectorView({
      stackView: self.m_campaignSliderStackView,
      from: Elements.ORIENTATION_SELECTOR,
      el: Elements.RESOLUTION_SELECTOR,
      to: Elements.SCREEN_LAYOUT_SELECTOR,
      model: new BB.Model({screenResolution: null})
  });

  self.m_screenLayoutSelectorView = new ScreenLayoutSelectorView({
      stackView: self.m_campaignSliderStackView,
      from: Elements.RESOLUTION_SELECTOR,
      el: Elements.SCREEN_LAYOUT_SELECTOR,
      to: Elements.CAMPAIGN,
      model: new BB.Model({screenLayout: null})
  });

  self.m_campaignSliderStackView.addView(self.m_resolutionSelectorView);
  self.m_campaignSliderStackView.addView(self.m_screenLayoutSelectorView);
  self.m_campaignSliderStackView.selectView(self.m_campaignSelectorView);

```



## StackView API

### `addView(i_view)`
Add a view to StackView using an element id or Backbone.View instance

### `getViews()`
Get all registered views

### `getViewByID(i_id)`
Find a registered backbone view by its id or cid

### `selectView(i_view)`
Select a view to present in the DOM, implementation varies per derived class

### `selectIndex(i_index)`
StackView.Fader only, Select a stack view using an offset index

### `slideToPage(i_toView, i_direction)`
StackView.SlideView only, select View using animated sliding of one view to the next.
Hardware acceleration for mobile is also supported, enable it via the included CSS.

### `closeModal(modal_id)`
StackView.Modal only, close via animation the currently opened modal window



