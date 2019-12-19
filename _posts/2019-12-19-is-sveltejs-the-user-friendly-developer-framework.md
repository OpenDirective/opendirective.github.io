---
layout: post
title: Is svelte.js the user-friendly developer framework?
---

<div class="message">
Svelte provides an excellent Developer Experience (DX) whilst supporting a slick User Experience (UX), even on low powered devices. It does this through performing as much work as possible at build-time resulting in tight minimal code that when deployed directly interacts with the web platform in the browser.
</div>

In my [last post on hyperapp](./2019-08-04-hyperapp-micro-framework) I explained its interest in terms of avoiding user frustration with slow performance of websites, often due to heavy reliance on a lot of javascript. Hyperapp is a tiny payload and is reasonably fast so can deliver a good UX on many older devices and even poor connectivity as often experienced by mobile users.

Svelte also provides excellent UX performance but it does this by eschewing the currently popular dev friendly runtime VDOM technique, which may negatively impact the UX on all but the best devices. Svelte does this by using a compiler providing build-time generation of 'laser'precise' UI code that acts directly with the browser DOM and BOM APIs.

Additional improvements in speed and reliability can be gained through svelte's use of templated component markup. This transparently supports Progressive Enhancement (PE) of browser processed and reliable HTML with CS with a developer adding extended behaviours with the more fragile JavaScript.

Whatever framework They are using developers must have a good understanding of the web platform in order to create an performant, reliable and inclusive UX. The compiler backed template JavaScript-in-HTML approach makes this a natural approach compared to JSX HTML-in-JavaScript which might even be said to encourage deep abstractions away from the platform.

In other words, svelte supports modern component UI design patterns but with a compiler reducing runtime loading / processing times seen by the user and also allows developers to take direct advantage of the web platform strengths with minimal extra framework abstractions (such as React hooks). Of course, abstractions are possible but as decided by the developers, hopefully with a good cost/benefit tradeoff analysis. Svelte has preprocessor hooks to expand possibilities suc has CSS tooling but I sense keeping close to the platform is encouraged.

Svelte does provide a few abstractions but these are minimal and extremely useful. For example Stores are an approach to cross component data that are observable (AKA reactive). In fact, stores very closely follow the core APIs of functional reactive programming (FRP) libraries like RxJS. Another set of abstractions make animations and transitions easier to programme. These support both Javascript and CSS approaches too, providing flexibility.

A couple of other abstractions are really overloads of existing JavaScript syntax. While this may a first seem a bad idea in practice they work REALLy well and make sense given we have a compiler that transpiles to JavaScript. These are the use of 'export' to declare properties exposed by components and a '$' label to invoke automatic subcription / unsubscription to stores.

One concern that might come to mind with the use of a compiler is the ease of debugging the output code. In actual fact I found this to be a non problem. The code is clean and consistent using a few light abstractions that you might well develop yourself when working directly with the DOM. Even the component scope CSS is easy to work with with modern browser tooling, plus you can use global CSS as well.

Svelte is experiencing something of a major surge in interest right now and this is perfectly understandable given version 3.0 bringing together various excellent ideas, hitting a sweet spot for both DX and UK. Personally having been a heavy user of Macro Assemblers, the make tool with scripts and the awesomeness of generics using the C++ processor I fully appreciate the approach of removing work from run time in order to improve both DX and UX.

One final impressive "feature" of svelte is the excellent resources svelte's developer Richard Harris has provided for learning. There's a REPL and the tutorial provide an interactive place to explore which once completed leaves you with a solid understanding of svelte.

Svelte is relatively young but with a well led and fairly small community. Even so, it is poised to have a big impact. In fact, I just saw it prominently featured in the 'State of JavaScript' survey results, taking pride of place next to the big popular frameworks. Watch this space!

I explored much of svelte by developing an inclusive [random number "spinner" app](https://github.com/SteveALee/svelte-donut-spinner) as a fairly non-trivial sample. This might be useful as an example, in addition to the many other resources linked to from [the svelte website](https://svelte.dev/). Svelte has a related project, [sapper](https://sapper.svelte.dev/), which adds features required in a SPA and server side rendering with hydration of the frontend.

Recently, Jason Lengstorf [learnt svelte with Richard Harris](https://www.learnwithjason.dev/let-s-learn-svelte) a process that's fun and informative to watch.
