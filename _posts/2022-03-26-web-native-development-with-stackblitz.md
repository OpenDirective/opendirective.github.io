---
layout: post
title: Web native development with StackBlitz
---

<div class="message"><p>The web has evolved into the most powerful and ubiquitous app deployment platform. We're used to rich in-browser apps that connect to external services and support collaborative real-time experiences. Developer tooling is no different, with on-line development environments (IDEs) and management consoles for front and back ends.</p>

<p>But the servers are still distinct entities and not part of the apps themselves. That all changed when <a href="https://stackblitz.com/">StackBlitz</a> recently introduced <strong>WebContainers</strong> to the developer community.The rules just changed.</p></div>

## The web as a platform

The web has rapidly grown into a powerful device-independent platform for apps as well as content. New capabilities appear in browsers in a steady trickle but every now and then a significant jolt occurs. For example, standards like CSS Grid, WebAudio, WebAuthentication, Portable Web Apps (PWAs) and Web Assembly (WASM) have had a big impact on what developers can do with the web to provide rich functionality and excellent user experiences.

Google Maps was an early example of how the web supports rich and powerful user experiences. Now, users expect to work on their data in apps when they are online or offline and synchronised across multiple devices with different capabilities.

As many front ends have become more JavaScript centric, several hosted IDEs appeared that provide rich development environments for front end development. These show an editor side-by-side with a view of the web content.

A shout out here to Microsoft Visual Studio code for providing a solid web technology IDE based on their Monaco editor which is being used in developer focussed web apps. Plus a shout out to Google V8 browser core powered the initial desktop version of VS Code using Electron.

These online IDEs, and other web development services including no/low code, still use servers, though they are largely abstracted away from the developers. And of course developers often manage their own servers for ownership or more complex deployments. These servers support the deployment build chain and runtime for both web pages API endpoints tha rich web apps require. Servers also typically run developer tooling and code platforms as an alternative to using a local workstation.

But what if this separate backend server complexity could also be included in-browser along side the frontend code? What would that even mean? What new features would be enabled?

![A web browser showing files, terminal and preview](/public/stackblitz.png "A full stack app running in a web browser - spot the terminal")

Well you can now try this for yourself, thanks to the groundbreaking StackBlitz service built on their WebContainer technology. Here's an example that runs a full stack [SvelteKit app](https://stackblitz.com/edit/sveltejs-kit-template-default-cemscb?terminal=dev). it boots up in seconds. I think you will be impressed.

# WebContainters

Containers hit the developer world a while back and had a massive impact on system development practices and backend architectures. These lightweight virtual machines provide a standard way to package and deploy systems code. They are isolated yet share the services of the underlying Operating System (Linux or Windows)

What StackBlitz have created, and are building their business on, are containers that run in a web browser. Yes, that right, this is impressive feat is now feasible with the fantastic modern browsers. And what's more, they run really fast too.

WebContainers containers provide a complete linux-style OS with a kernel and minimal `bash` command line environment. This is accessed via terminal window in the StackBlitz web app. In order to provide developer tooling and a web server, you also get `nodejs` and a custom `npm` clone design for speed and security. I hear that more POSIX conformance is being considered too.

In addition, for more complex web development needs, `vitejs` is included. While ESM modules could be used explicitly with modern browsers, vite adds useful module import features such as node_modules module resolution and import of css files.

This is "no mean feat" and has to be seen to be believed. I don't know how much has been implemented from scratch or is a port like the embedded ARM ports of Linux. But the Browser context is distinct from even embedded platforms.

The key enabling technology here is WebAssembly (WASM) which enables code to be cross compiled to run in a JavaScript engine. Plus, Service Workers, which provide the required "process" for the container to run in, separate from the frontend HTML browser context. The container exposes the usual socket/port for the browser front end to connect to.

To provide a complete fullstack integrated development environment (IDE), StackBlitz adds a VS Code editor and file system browser. They also provide GitHub integration for version control. Soon, I understand GitHub will be available as the file system as well, providing code security and common developer workflows. The frontend preview can be opened in its own tab/window as well.

The final touch is a set of predefined containers covering a range of back and front end technologies. These are opened with a click, landing you in the IDE ready to go once the WebContainer boots and serves the web pages..
# The power of a URL for developers

What this means is that given a URL you get a fully functional nodejs server as well as the frontend app, which is also served from the container. This is all running in the browser window / tab. You can then use the powerful browser F12 developer tools to debug BOTH front and backend.

The reason is say this is "web native" development is that apart from running the backend in the browser, you get to leverage the core super power of the web. The URL!

URLs underpin the [RESTful](https://en.wikipedia.org/wiki/Representational_state_transfer) web architecture. They identify a resource and you can share them, embed them in content, use them in apps, even put them in a QR code. With StackBlitz that means there are now many ways to access a full stack app running in a web browser.

As developers often want to leverage existing code and modify it, StackBlitz provides as simple way to use the common "fork" pattern. This gives you a complete new copy of the full-stack, ready to work on. With the coming improved GitHub integration that's going to neatly fit existing developer workflows too. This could open up new ways to make and accept community contributions via Pull Requests.

As browser's support multiple tabs you can have multiple web apps running at once. For example, to compare a fork or Pull Request. They are completely isolated, neatly avoid the common problem experienced on developer machines of conflicting version fo tools or other dependencies.

Under the bonnet (hood), I expect a URL simply references a text based container configuration resource. Meaning not only low server costs for StackBitz but also, as static assets, they can be pushed to the edge. Thus providing very low latency for an even faster developer experience.

# Other benefits

This is much more than ultra cool technology. It could have a huge impact on how developers work together.

There are some good security gains too. For example, there's less over-the-wire traffic. The code also runs in the deliberately secure browser sandbox. There's also no server attack service to secure and currently WebContainers expose very few ports with access limited to the same browser.

Reliability is boosted by the being able to restart the container and build the stack with a simple browser refresh. The lightning fast speed makes this usable.

I'm convinced part of the reason the web exploded was the ability to "View Source" enabling developers to see how code worked. StackBlitz expands this powerful visibility to the server code as well. Of course the caveat is that much terrible code can easily get propagated.

For educational uses, embedding a URL in a course, blog or even a presentation provides instant access to running code! What could be better.

The compute model is basically Bring You Own Device (BYOD). There's no need to pay high PAYG server runtime costs.

# Limitations

For now at least, the backend is limited to a nodejs server and simple architectures. Thats may not be a restriction for some but there are many other tools out there. For example Ruby is still very popular as is PHP.

When you wish to deploy elsewhere there is no one-click option, but it's not that hard to do. Static websites, for example SSG generated, are easy enough as you can push the source to git and use the repository in a CI/CD service like Vercel or Netlify. The backend code can also be deployed to PaaS or other environments supporting server configuration.

However, I expect serverless will prove to be the best fit as they can run in the WebContainer as well deployed to common services. Refactoring stateless server code into multiple separate serverless functions is easy enough to do. Deployment could then be automated, perhaps using something like the Serverless project to provide vendor neutrality.

# Blue sky

It's going to be exciting to see what new developer ideas StackBlitz WebContainer seed.

For example, how about multiple WebContainers each running locally in its own browser tab providing a microservice like architecture? In browser Kubernetes orchestration anyone?

# Summary

In case you can't tell I'm *really* exited by StackBlitz WebContainers. I love the web with a geeky passion. I'm fascinated by developer tooling, workflows and system architectures (which often mirror company structures). In my career I've worked on embedded RTOSs, digital communications, systems code and full stack web. WebContainers bring everything together in one powerful concept. I expect great things.
