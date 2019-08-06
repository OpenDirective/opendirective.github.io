---
layout: post
title: Hyperapp, the frontend web framework your users might appreciate
---

<div class="message">
Hyperapp is a framework for building web interfaces and ideal for when you don't want a full-blow frameworka (and perhaps for when you do).
</div>

Right now is a "fascinating" time to be a frontend web developer. There's plenty of interesting tools and frameworks to help improve the developer experience and provide rich user experiences. The downside can be the steep developer learning curve with complex configurations.

But your users may not always be happy with their experience of your technical choices. The main popular frameworks, React, Vue and Angular, make heavy use of JavaScript running in the browser. Large JavaScript payloads need to be transmitted, loaded and parsed by the browser before they even start to do anything towards display. Any errors are usually fatal unlike errors that occur in processing declarative HTML and CSS.

The end result can be frustrated users who experience delays, broken display and crashes. This is most likely to happen if they have old or low specification hardware, old web browsers or poor connectivity. Unless the developers are lucky enough to be targeting a controlled homogeneous environment, like an intranet, then at least some of the users are very likely to be in this unhappy group. They are also unlikely to remain users for long.

More recent developer tooling advances aim to address these issues through techniques that preload some HTML or reduce the size of Javascript loaded at any time. For example, "server side rendering", "code splitting" and "tree shaking" are common practice. One recent framework, Svelte, reduces runtime size by doing as much work as possible at build time.

However, there is another developer approach to avoiding such user frustrations. That is to not have a framework at all. Or else, use one with a very small runtime. Remember, frameworks usually have to cover much wider requirements than your current project. They are often designed to solve the problems experienced by large teams working on very large websites. Thus, they may not be the best solution for you current project, even if you already know a specific framework really well.

If the requirements are close to what browser links and forms provide or you only need to dynamically update a small part then you probably don't need a framework and can just code to the DOM. That's the approach I took with a recent project [YouTube Current Time](https://github.com/music-practice-tools/youtube-current-time/blob/master/ytct-es6.js) that shows the playing time of a video while you scroll around a page. Using a framework would have been overkill and introduce more dependencies and complications that necessary.

With this no-framework approach it's usual to develop architectural patterns and structures that are effectively a custom project specific framework that completely matches the requirements, and no more. In fact, it is common to develop a minimal starter framework thay matches developer preferences and best practices. And this is where [hyperapp](https://github.com/jorgebucaran/hyperapp) is useful. Especially when a functional programming approach based on the [elm architecture](https://guide.elm-lang.org/architecture/) is preferred.

Version 2 of the tiny Hyperapp framework has almost landed; it's on master in github but the npm package has not yet been published. While the docs are not quite available yet the [source code](https://github.com/jorgebucaran/hyperapp) is small and relatively easy to digest. Of the 500 odd lines of source code, the first 400 are taken up with the [Virtual DOM](https://reactjs.org/docs/faq-internals.html) implementation and can be ignored if you want to concentrate on the framework proper. There is also a set oof fficial [examples](https://github.com/jorgebucaran/hyperapp/blob/master/docs/examples.md), plus [an implementation](https://github.com/zaceno/sevenguis-hyperapp) of [the 7GUIs](https://eugenkiss.github.io/7guis/) sample apps.

Here's an overview of the few elements that make up Hyperapp. They are mosty just functions as you'd expect in a functional programming framework

## App

The entry function that mounts and manages a DOM tree in the given DOM node. Is is passed: inital state, view and optional subscriptions and renders the view passing the state. User and other events are passed to Actions which may update the state or invoke an effect. Changes in state cause the view to be rerendered. It is possible to mount multiple apps if required.

## State

There is a single state tree for the entire app and it is only ever mutated in Actions, never directly. 

## VDOM

Hyperapp includes it's own Virtual DOM. As usual, this allows the view to describe the entire DOM tree using light weight virtual DOM nodes. The VDOM ensure only the virtual nodes that have changed are then updated in the actual DOM.

## View

Maps the State to DOM indirectly by using the VDOM `h()` function to build a virtual DOM tree. Abstractions can be used instead of `h()`, including `hyperapp/html` (or equivalent) helper functions for each HTML element, the `JSX` build time parsed HTML-like syntax or `hyperx` ES6 template strings parsed at run time.

## Tuples

Hyperapp uses 2-tuples consisting of a function and a parameter. As JavaScript doesn't have tuples, arrays of 2 elements are used.

## Actions

Actions are functions that are invoked by the framework and passed the State and an optional parameter. They can return a new State or else invoke an Effect and return it's resulting tuple. They are declared to the framework either as DOM node event handlers in the View or in Subscriptions passed to the App. They abstract DOM event handlers with the framework adding and removing handlers and subscriptions as required. 

References to an Action can be declared as the function itself (name or inline) or a Tuple containing the function and an argument. 

## Subscriptions

A way to attach Actions to events not specified with DOM node event handlers in the view.  For example, global DOM events or side effects such as timers. A Subscription is a Tuple of a function to call to set up the subscription and a props object to pass as the parameter to the function. The setup function is also passed a dsipatch function that it uses to 'send' the event to hypeapp. In addition, the setup function returns the function that hyperapp can call to teardown the subscription when it is no longer needed.

This may seem a little complex so see the [Clock example](https://github.com/jorgebucaran/hyperapp/blob/master/docs/examples.md#simple-clock) which uses [hyperapp/time](https://github.com/SteveALee/hyperapp/blob/master/lib/time/src/index.js) 'interval' effect.

The array of Subscriptions is passed to the App by a function which is called whenever the State changes. This function is passed the State and returns and array of subscriptions. 

## Effects (fx)

Effects are functions that perform side effects for the Ap; they can be thought of as "drivers" that perform all the external work of the App such as calling a RESTful API endpoint, giving focus to an element or saving data to local storage. This way the Actions remain pure functions which simplifies reasoning about logic and eases testing. 

Effects are invoked from Actions when they return a Tuple of the effect to perform as a function and an argument.

## Conclusion

And that's it. A small number of concepts and a tiny runtime make hyperapp a flexible framework suitable for small to medium applications. The universal use of pure functions with side effects managed separately makes unit and integration testing relatively easy without mocking or stubbing. Functional programming also reduces the need for other techniques like dependency injection. As there's so few moving parts, hyperapp is fertile ground for developing your own abstractions and patterns that perfectly fit your specific problem domain.

So that's hyperapp. Do have a look at the [examples](https://github.com/jorgebucaran/hyperapp/blob/master/docs/examples.md) and have a play. You only need an editor, a browser and possibly a local server (eg `npx serve`). Just like the "good old days" of web development.

