---
layout: post
title: Hyperapp, the frontend web framework your users might appreciate
---

<div class="message">
Hyperapp is a framework for building web interfaces and ideal for when you don't want a full-blow framework and maybe for when you do.
</div>

Right now is a "fascinating" time to be a frontend web developer. There's plenty of interesting tools and frameworks to help improve the developer experience and provide nice rich user experiences, albeit often with a steep developer learning curve.

But your users may not always be happy with their experience of your technical choices. Several popular frameworks are heavy users of Javascript running in the browser. Large JavaScript payloads need to be loaded and parsed by the browser before they start to do anything. Errors are often fatal, unlike those that occur in declarative HTML and CSS.

The end result can be frustrated users experiencing delays and crashes. Especially if they have old or low specification hardware, old web browsers and / or poor connectivity. Unless you're lucky enough to be targeting a controlled homogeneous environment like an intranet then at least some of your uses are very likely to be in this "less well off than you" category. They are unlikely to remain your users for long.

More recent developer tooling aim to address these issues through techniques that preload some HTML or reduce the size of Javascript loaded at any time. For example, "server side rendering", "code splitting" and "tree shaking" are common practice and one framework, Sveltejs, goes reduces runtime size by doing as much as possible at build time.

However, a possibly more satisfying solution is to not use a framework at all. Or use one with a very small runtime. Remember, frameworks usually have to cover much wider requirements than yours and are often designed to solve the problems experienced by large teams working on very large websites. Thus they may not be the best solution at all for you current project, even if you already know a specific framework really well.

If browser links and forms are not enough interactively but you only need to dynamically update a small part of the User Interface then you probably don't need a framework at all. That's the approach I took with a recent project [YouTube Current Time](https://github.com/music-practice-tools/youtube-current-time/blob/master/ytct-es6.js) that shows the playing time of a video while you scroll around a page.

With this no-framework approach you will most likely develop architectural patterns and structure that are effectively your very own framework that completely matches your requirements, and no more. If like me, you prefer a functional programming approach and appreciate the [elm architecture](https://guide.elm-lang.org/architecture/) then the tiny [hyperapp](https://github.com/jorgebucaran/hyperapp) micro framework might interest you for jump-starting your next project.

Hyperapp V2 has almost landed; it's on master in github but the npm has not been published. While the docs are not quite available yet the [source code](https://github.com/jorgebucaran/hyperapp) is small and relatively easy to digest. Of the 500 odd lines of code, the first 400 are taken up with the [Virtual DOM](https://reactjs.org/docs/faq-internals.html) implementation and can be ignored if you want to concentrate on the framework proper. There is also a set official [examples](https://github.com/jorgebucaran/hyperapp/blob/master/docs/examples.md) and an implementation of [the 7GUIs](https://github.com/zaceno/sevenguis-hyperapp).

Here's an overview of the few elements of Hyperapp - they are mosty just functions as you'd expect in a functional programming framework

## State

There is a single state tree for the entire app and is only ever mutated in actions or subscriptions, never directly. Note however, you can mount multiple apps into a HTML page if required.

## VDOM

Hyperapp includes it's own Virtual DOM. As usually this allows the view to describe the entire User Interface and the VDOM ensure only yhte parts that have changed afer updated in the DOM.

## View

Maps the state to the User Interface using the VDOM `h()` function to build virtual HTML nodes. As usual, if you prefer, you can use abstractions including `hyperapp/html` (or equivalent) helper functions for each HTML element, `JSX` build time parsed HTML-like syntax or `hyperx` template strings parsed at run time.

## Actions

Functions that are called as the result of user actions. They abstract DOM events and are passed the current state, returning the new state.

## Subscriptions

Functions similar to Actions that are called when an external event occurs. For example timers, HTTP server responses and global DOM events.

## Effects (fx)

Functions that perform all side effects letting us use pure functions elsewhere. They are passed the effect to perform as a function and a parameter / payload. Thus they are effectively "drivers" encapsulating side effects and all have the same API (similar to ioctrl).

## Tuples

Most of the above functions are invoked with or return 2-tuples consisting of a function and a parameter. As JavaScript doesn't have tuples, arrays are used.

## Conclusion

And that's it. A small number of concepts and a tiny runtime providing a flexible framework suitable for small to medium applications. The universal use of pure functions with side effects managed separately effects makes testing relatively easy without mocking or stubbing. Functional programming also reduces the need for other techniques like dependency injection.

So that's hyperapp. Do have a look at the [examples](https://github.com/jorgebucaran/hyperapp/blob/master/docs/examples.md) and have a play. You only need an editor, a browser and possibly a local server (eg `npx serve`). Just like to "good old days" of web development.

