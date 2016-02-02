# Introducing jQuery

jQuery is the most popular JavaScript library on the Web. Roughly two in three websites use jQuery, and many devs are pretty much lost without it. It includes a powerful set of features that will work consistently across a wide range of browsers, and encourages a programming style that makes working with browsers more elegant.

jQuery is like a swiss army knife for front-end developers. You have a single function, with a bunch of attachments, that you can use to solve all sorts of common problems. jQuery has exceptionally powerful tools for working with the DOM, and includes support for animating changes, handling events and using AJAX. jQuery is also very easy to extend, using functions known as *plugins*, and there are thousands available online.

## Installation

jQuery Core is the main jQuery library, and it is what people refer to when they say "jQuery". The jQuery Foundation maintain other software, including jQuery UI, jQuery Mobile, jQuery Color and QUnit. In practice, most people just use jQuery Core, and that is what this tutorial covers.

jQuery is a single file that you can just point a script tag at. You can host your own copy, but it is generally better to use a content delivery network. Every CDN hosts jQuery, and jQuery has official releases on the MaxCDN network.

### Versions

There are actually two stable versions of jQuery. jQuery 1 supports pretty much every browser in use, but is larger and a little bit less efficient than jQuery 2, which only supports Internet Explorer 9 and above. If you are certain that you only need to support modern browsers, you can use jQuery 2 and enjoy slightly better performance.

The jQuery Foundation link to copies of all of their software at [code.jquery.com][1]. You can download anything you need from that page, and the copies are hosted by MaxCDN, so you can just link to them directly in your applications. For example, this tag installs jQuery 2.2.0 (minified):

~~~ html
<script src=https://code.jquery.com/jquery-2.2.0.min.js></script>
~~~

### The Dollar Thing

When you install jQuery, it creates two names, `jQuery` and `$`. Both names point to jQuery, and you can use them interchangeably.

If using `$` for jQuery conflicts with another library, you can add the following statement to the start of your code, and jQuery will restore `$`, so it references whatever it referenced before jQuery was loaded:

~~~ javascript
jQuery.noConflict();
~~~

[1]: https://code.jquery.com/

## Getting Started

jQuery can be invoked, like `$(args)`, and it also has some utilities bound to it, like `ajax` and `isArray`, which you reference as properties, like `jQuery.ajax(args)`.

When you invoke jQuery, you can it pass a function that will be invoked once the document is ready to be manipulated. This gives you an easy way to keep your code in its own scope, and it ensures that your code will execute as soon as the DOM is ready to manipulate (once all the html has been parsed).

~~~ javascript
$(function() {
    // Your script goes here. It will
    // execute once the document is ready...
});
~~~

That code is a more convenient way of doing this:

~~~ javascript
jQuery(document).ready(function(){
    // Your script goes here...
});
~~~

Once you have your boilerplate set up, you will generally use jQuery to operate on selections of DOM elements. You select elements by passing jQuery a *selector*, which is a string that will be parsed by Sizzle.

## Sizzle

Sizzle is jQuery's selector engine. It uses the same syntax as CSS, with a few extras, so you do not need to learn a whole new language if you know CSS already.

~~~ javascript
$("p") // selects all the paragraph elements
$("#foo") // selects the element with the id foo
$(".bar") // selects all the elements in the bar class
$("[src]") // selects all the elements with a src attribute
$("tr:even") // selects all the even table row elements
~~~

If you do not know any CSS, you should at least learn how to write basic selectors before you dive into jQuery. They are easy to quickly pick up, and every front-end dev needs to know them.

## jQuery Objects

When jQuery is invoked with a selector, it returns the selection as an array of zero or more DOM elements. These arrays are not native, and are known as *jQuery objects*. jQuery objects come with a comprehensive set of methods for operating on the elements that were selected, and you can add new methods as plugins.

You can also pass a HTMLElement or NodeList to jQuery, and it will return a jQuery object that wraps the argument, so you can use jQuery methods on it.

Note that while you use the jQuery function to select the elements you want to work on, you do all the real work with jQuery methods, which are bound to the jQuery objects that the jQuery function constructs and returns.

Using jQuery to select some elements, then applying methods to the selection, is normally achieved with in single expression. For example:

~~~ javascript
$("p").addClass("foo")
~~~

That expression selects all the paragraph elements, then invokes the `addClass` method on the resulting jQuery object, adding the `foo` class to each of the selected elements.

## Quick Review

To summarise what you have seen so far:

- jQuery is a function, and it has other utility functions bound to it, like `ajax` and `noConflict`.
- You can pass a function to jQuery as a convenient way of setting up your code in its own scope, and to ensure that your code will execute as soon as the DOM is ready.
- You normally pass jQuery a selector, and it returns an array of elements, wrapped in a jQuery object.
- jQuery objects come with lots of methods for working with the selection of elements they contain.

### Using The Docs

Keep in mind that you do not need to know many jQuery methods to get started with it. You really just need to know how jQuery works. It has excellent API docs, and its methods have highly intuitive names, so you can normally google for whatever you need in a few seconds, even when you do not know exactly what you are looking for. Once you understand what they call the "jQuery programming style", you can easily find the methods you need with a search engine.

## jQuery Style

Whenever it makes sense, jQuery methods return the selection they were invoked on. This allows for jQuery style pipelines, where methods are daisy chained together. Because each method returns the jQuery object that it was invoked on, you can recursively invoke methods on the result of the preceding invocation:

~~~ javascript
$(selector).method0(args0).method1(args1).method2(args2);
~~~

In the expression above, `method0` is invoked on the jQuery object returned by `$(selector)`. Once `method0` is done, it returns the object it was invoked on, allowing `method1` to be invoked on the same object. Likewise, `method1` returns the object, allowing `method2` to be invoked on it in turn. You will use these kinds of expressions a lot with jQuery, so it is important to understand them.

This style of programming, where data structures, especially arrays, are passed through a pipeline made from a daisy chain of method invocations is popular with JavaScript programmers, and is often called "jQuery syntax".

Note that in some cases, methods return a different selection to the one they are invoked on. For example, the `not` method removes elements from a selection, and the `find` method finds elements based on the selection. These methods return a different jQuery object that represents the new selection, allowing the pipeline to continue operating. Take the following expression as an example:

~~~ javascript
$("img").not(".thumbnail").css({border: "1px solid red"})
~~~

That expression first selects all the image elements, then removes any that are in the `thumbnail` class, then invokes the `css` method on the remaining images.

### Exceptions to the Rule

Some jQuery methods can only operate on a single element. In those cases, the method will always operate on the first element in the selection. This statement grabs a copy of the html inside the first section element on the page only:

~~~ javascript
let synopsis = $("section").html();
~~~

You often use jQuery to operate on a single element too:

~~~ javascript
let middle = $("#banner").width() / 2;
~~~

There are a handful of methods that do not return a jQuery object, and you can not chain invocations with them.

## The Can Do Method

jQuery has a method that can do anything. It is named `each`, and you use it whenever you need a quick custom method, but only need it once, so defining a plugin would be overkill.

You invoke the `each` method like any normal jQuery method, and pass it a function that defines the custom logic you want to apply. The function is invoked for each element. It receives the iteration index as an argument, and you can reference the element itself as `this`.

~~~ javascript
$("#profile li").each(function(index){ console.log(this.innerHTML) })
~~~

Note that `this` references a regular HTMLElement, not a jQuery object, but you can pass it to jQuery if you want to use jQuery methods on it.

~~~ javascript
$(#profile li).each(function(index){ console.log($(this).html()) })
~~~

## Contextualising Selections

jQuery normally searches the whole document for elements that match the selector, but you can provide a *context* for the search, so jQuery only searches a subtree.

When you pass jQuery a selector as the first argument, you can optionally pass another selector or another element as the second argument. The second argument defines the context. For example:

~~~ javascript
$("button", "#controls")
~~~

In that expression, jQuery finds the context (the element with the id `controls`), then searches it for button elements. jQuery then returns the buttons as a jQuery object.

## Naming Selections

If you assign a name to a jQuery object, a common convention is to start the name with a dollar character. This makes it easy to distinguish between jQuery objects and regular HTMLElements and NodeLists, which can save quite a lot of confusion in practice.

~~~ javascript
var $examples = $("code.example");
~~~

### Efficiency

It can be relatively expensive to gather a selection of elements, so it is good practice to assign names to selections that are used repeatedly, In loops and hot functions, you should avoid traversing the DOM to select the same elements multiple times. For example, this code traverses the DOM on every invocation:

~~~ javascript
function slow(args){ $(selector).method(args) }
~~~

This code achieves the same thing, but only traverses the DOM once, then references the selection on each invocation:

~~~ javascript
var $elements = $(selector);
function fast(args){ $elements.method(args) }
~~~

## Events

jQuery makes it easy to handle events. Events are generated when stuff happens, such as mouse clicks, key presses, form submissions and when elements finishing loading.

Events are represented by event objects, and jQuery defines and uses its own event objects that normalise the native events.

jQuery lets you select the elements you want to watch for events, define the type of event you want to respond to, and provide a function that is called whenever the chosen event occurs on any of the selected elements. For example:

~~~ javascript
$("li").on("click", function(event){ console.log(this) });
~~~

That code selects all the list items, and binds a handler to click events that just logs the clicked element in the console.

The `on` method takes a string that defines which type of event to listen for, and a function that is called whenever the event occurs. The function receives the event object as an argument, and `this` will point to the element that the event occurred on. Note that `this` references the regular HTMLElement, not a jQuery object. As with the `each` method, you can pass `this` to jQuery if you want to invoke jQuery methods on it.

### Event Methods

jQuery defines a set of convenience methods that wrap common events, so you can just pass the handler to the method as the only argument:

~~~ javascript
$("li").click(function(event){ console.log(this) })
~~~

There is [a list of all of the events][1] in the jQuery docs.

It is important to note that the `on` method works a bit differently to the wrapper methods, so the following two statements are not equivalent:

~~~ javascript
$("li").click(handler);
$("li").on("click", handler);
~~~

The first statement uses the `click` method, which only applies to list items that exist when `click` is invoked. The second statement uses the `on` method, which applies to all list items, even those that are created after `on` is invoked. This is important when you are creating elements dynamically. For example, if you append new items to a list, `on` will handle clicks on all of the items, while wrapper methods like `click` will only handle events on the items that existed when the method was invoked.

[1]: https://api.jquery.com/category/events

### Triggering Events

You can use the `trigger` method to trigger an event, passing the event name as an argument.

~~~ javascript
$(selector).trigger("click")
~~~

You can also use the convenience methods with no args to achieve the same thing.

~~~ javascript
$(selector).click()
~~~

When you are triggering events, you can optionally use custom event types.

~~~ javascript
var $element = $(selector);
$element.on("foo", handler);
$element.trigger("foo");
~~~

### The PreventDefault Event Method

Even if you handle an event, the event will continue to bubble up naturally, perhaps causing other things to happen too. Event objects have a `preventDefault` method that is used to stop this from happening. The following code just makes anchor tags stop working:

~~~ javascript
$("a").click(function(event){ event.preventDefault() })
~~~

In practice, you would obviously have some alternative logic in the callback function as well, maybe using AJAX to load content when the user clicks a link.

### Event Summary

Here we will just quickly reiterate a few key points to keep in mind when you are starting out with jQuery events:

- Events are non-native objects that are generated by jQuery when stuff happens.
- The `on` method is used to set up event handlers. It takes an event type and a callback. There are also convenience functions for the common events, that just take the callback.
- The `on` method applies to every element the selector matches, even if the element is created after `on` is invoked, while the convenience methods do not.
- The callback function is invoked to handle each event. The function receives the event object as an argument, and `this` references the element that fired the event.
- You can trigger an event on the elements in a selection with the `trigger` method, which takes a string that defines the event type. You can alternatively invoke the convenience methods with no args.

## Loose Ends

This tutorial has focussed on using jQuery to mutate the DOM and handle events, which is what you will primarily use jQuery for. We have not covered jQuery's AJAX support, the animation features or how to define and use plugins. You should definitely look into those things as you come across a need for them, but the features we covered here are what you need to get up and running with jQuery, and to understand its unique approach to front-end web development.

jQuery has a huge community and ecosystem, it is fun to use, and it makes such light work of browser programming that a framework is often redundant. Every front-end dev should have it in their tool kit.

