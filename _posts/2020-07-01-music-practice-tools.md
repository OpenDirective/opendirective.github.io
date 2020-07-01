---
layout: post
title: Music Practice Tools
---

<div class="message">
As an adult learner of bass guitar I find there's much to get to grips with. In addition to instrument technique there's a raft of musicality skills. As a confirmed geek I've long thought about various neat tool projects using the web platform which is now very musically capable. And then COVID hit us and my lessons became remote, requiring distant sharing of information. Add to that my aspirations of keeping a musical journal and [musicpracticetools.net](https://musicpracticetools.net) was born.
</div>

## Tools for practice and remote lessons

Fortunately, my bass teacher encourages me to develop all-round skills and a comprehensive daily practice has evolved. I have a list to work through and while I could do with a paper checklist, I liked the idea of something interactive. Plus it could integrate with a timer so each section could be timed with an indication to move on to the next.

Every week we have a video call and holding my scruffy transcriptions up to the camera turned out a pain. It'd be much better if I could share nicely formatted dots via a web url. And what if the dots could be played so I could check the transcriptions?

Then there's the annoyance of repeated changes of bpm on the metronome and picking random keys or exercise numbers.

My existing [Youtube current timer](https://github.com/music-practice-tools/youtube-current-time) will be really useful when practicing. And a series of links to the sections would be even more help and provide a cool way to animate song structure.

Other gadgets like a tuner seem unnecessary, though perfectly possible, as the solutions I have already are fine.

Finally, there's the idea of keeping a journal, with notes, ideas, thoughts, a collections of tunes I've work on or would like to learn, and more.

## But what is it exactly?

So while this could be a web I had something a bit different in mind. Basically, a blog but enriched with on-page widgets to provide interactive tools.

Unlike app's which have interactive settings a blog has a definite write and public life cycle. That might become a problem but when it's just for myself that's really not an issue. I'm happy to hack enriched markdown in an editor and deploy. An app has the advantage of an easy route to monetization via SaaS, but eventually I decide to make it open source in case others find it useful during these times.

## The Technical Stack

It has to be easy create pages and publish and also develop new widgets. That means some sort of publishing framework that also supports client side components. While WordPress is an established solution I'd much prefer static hosting to n-tier dynamic architectures. There's plenty of static site generators and even the static generation options for component frameworks like Next, NuXT and Svelte. In the end [11ty](https://www.11ty.dev/) won me over with it's light weight simplicity and flexibility. While it works with many templating languages for creating pages, Mozilla's [nunjucks](https://mozilla.github.io/nunjucks/) offers most functionality in 11ty.

For client side we need to create widgets that enhance the static content. I'm happy as a "pig in muck" crafting good ol' DOM manipulation myself but it does get tedious and "boiler-platey". So something is called for that helps without imposing the full overkill weight and fixed abstractions of a runtime framework like [insert your favourite here]. I'm keen to use svelte as I think it gets so much right, including minimal runtime. However, it is usually used for building complete single tree component apps and that's not what is wanted here. It will build web components and that certainly a possibility, though at the expense of complicating the build process. There are other interesting lightweight possibilities including [Hyperapp](https://hyperapp.dev/) and [lit-element](https://lit-element.polymer-project.org/) but in the end I stumbled upon [alpinejs](https://github.com/alpinejs/alpine) via [Tailwind UI](https://tailwindui.com/) and it really hit's the sweet spot.

Now what about the musical widgets using the web platform? Many moons ago when I was involved in Mozilla I was mightily impressed by the work at Seneca College led by David Humpreys. That work included a new audio API they were working on. Eventually that led the incredible [Web Audio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API) that we now have to play with. However this is no easy-peasy high level musical tool like Microsoft DirectPlay (now depreciated). The Web Audio API requires a lot of bit twiddling so it's wonderful to have the excellent [ToneJS](https://tonejs.github.io/) library to make it muc easier. For managing abstract musical concepts like scales we have [tonaljs](https://github.com/tonaljs/tonal).

And then there's [abcjs](https://paulrosen.github.io/abcjs/) based on the text musical score language semi-standard [abc](http://abcnotation.com/wiki/abc:standard:v2.1#first_and_second_repeats). This adds the icing on the cake providing SVG "dots" that can also be played. The only wrinkle is there is no WYSIWYG editor but rather you describe the notes in the abc language. But thats fine with me and works OK in a publishing workflow.

## making it

Dev and Build is Nodejs based (what else these days) but only for server side 11ty. The client side is pure un-built un-bundled un-munged HTML, CSS and JavaScript (like your grandma used to make), using global scripts. And why not? it works just fine and I'll bundle etc if and when I need to. I did look at browser modules but there's a few quirks to solve (like modules aer defered which messes up code in the main html file)

An what about deployment. Well, GitHub pages can be bent to work with out Jeckyl they have limitations. GitHub also has Actions now to provide Continuous Deployment and there's Zeit (I mean Vercel) now. But [Netlify](https://www.netlify.com/) really seem to call me with their good range of services and free pricing tier. They also support Serverless Functions for if/when we do need backend functionality. Netlify also have a CMS using GitHub for storage that would allow for an alternative in-browser editing / admin UI if ever required.

## Outro

So there we have it. A perfectly serviceable stack for a slightly left-field blog/journal/app thing. So far it has  proven more than adequate. I create widgets using client side Javascript, sometimes with Alpine, and 11ty shortcodes to make the markdown page editing experience quick and clean. That required a little trick with nodemono and browsersync to get watched rebuild of the shortcodes but works really well. We're really lucky to have so much great open source software to build on. Hopefully this adds a little something to that.

Now I can concentrate much more on practicing bass.

Oh, wait, I just thought of a really cool new feature to add.....
