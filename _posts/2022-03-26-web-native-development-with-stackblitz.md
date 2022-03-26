---
layout: post
title: Web native development with StackBlitz
---

<div class="message">The web has evolved into powerful and ubiquitous app deployment platform. We're used to rich in-browser apps that save our data online and support collaborative real-time working experiences. For developers, however, while backend services have moved from on-prem to cloud and edge, much of the development work is still performed on local devices.

That all changed when <a href="https://stackblitz.com/">StackBlitz</a> recently presented <strong>WebContainers</strong> to waiting developers. </div>

## The web as a platform

The web has rapidly grown into a powerful device-independent platform for apps as well as content. New capabilities appear in browsers in a steady trickle but every now and then a significant jolt occurs. For example, standards like CSS Grid, WebAudio, WebAuthentication, Portable Web Apps (PWAs) and Web Assembly (WASM) have had a big impact on what developers can do with the web to provide rich functionality and excellent user experiences.

Google Maps was an early example of how the web supports rich and powerful user experiences. Now, Users expect to work on their data in apps when they are online or offline and on multiple devices with different capabilities.

Many web apps use a mashup of third party services accessed via HTTP APIs, with backend services and files hosted in cloud and edge services. This provides many business and technical benefits.

As many front ends have become more JavaScript centric, several developer services arrived to provide rich on-line app-like development environments. These show an editor side-by-side with a preview of the web content.

A shout out here to Microsoft Visual Studio code for providing a web native rich IDE experience based on their Monaco editor. Plus a shout out to Google and Electron who's V8 browser core powered the initial desktop version of VS Code, opening the possibilities..

Yet, the back ends for apps are still largely developed on a local device and then deployed, as used to be the most common workflow for frontend development. Services such as Microsoft Azure's "configuration as code" and low/zero code abstractions have mitigated this somewhat by simplifying backend development.

But what if backend development could also be done in-browser using a rich app? What would that even mean? What new ideas would be enabled?

![A web browser showing files, terminal and preview](public/stackblitz.png "A full stack app running in a web browser")

Well you can now discover this for yourself, thanks to the groundbreaking StackBlitz service built on WebContainers. Here's an example that runs  [SvelteKit app](https://stackblitz.com/edit/sveltejs-kit-template-default-cemscb?terminal=dev) in seconds. I think you will be impressed.

# WebContainters

Containers hit the developer world a while back and had a massive impact on system development practices and backend architectures. These light weight virtual machines provide a standard way to package and deploy systems code. They are isolated yet share the services of the underlying Operating System (Linux or Windows)

What StackBlitz have created, and are building a business on, are containers that run in a web browser. Yes, that right, this is feasible now with the fantastic modern browsers we have. And what's more, they run really fast too.

These containers provide a complete linux-style OS with a kernel and minimal `bash` command line environment. This is accessed via terminal window in the StackBlitz web app. So you can run a web server (and more), you also get `nodejs` and a custom `npm` clone design for speed and security. I hear that more POSIX conformance is being considered too.

Finally, for more complex web development needs, `vitejs` is included. While in the StackBlitz developer environment, ESM modules could be used explicitly, Vite adds useful import features such as node_modules module resolution.

This is no mean feat and has to be seen to be believed. I don't know how much has been implemented from scratch or is a port like the embedded ARM ports of Linux. But the Browser context is distinct from even embedded platforms.

The key enabling technology here is WebAssembly (WASM) which enables code to be cross compiled to run in a JavaScript engine. Plus, Service Workers, which provide the required "process" for the container to run in that is separate from the frontend HTML browser context. The container exposes the usual socket/port for the browser front end to connect to.

To provide a complete fullstack integrated development environment (IDE), StackBlitz adds a VS Code editor and file system browser. They also provide GitHub integration for version control. Soon I understand GitHub can be used as the file system as well, providing code security and usual developer workflows. The frontend can be opened out in its own tab/window as well.

The final touch is a set of predefined containers covering a range of backend and front end technologies. This are opened with a click, landing you in the IDE ready to go.
# The power of a URL for developers

What this means is that given a URL you get a full-stack nodejs and frontend app. All running together in the browser. And, you can use the powerful browser F12 developer tools to debug BOTH front and backend.

The reason is say this is "web native" development is that apart from running the backend in the browser, you get to leverage the core super power of the web. The URL!

URLs underpin the RESTful nature of the web! They identify a resource and you share them, embed in content, uses in apps, even put in a QR code. That means there are so many way you can access a full stack app in a web browser.

As developers often want to leverage existing code a modify it, StackBlitz provides as simple way to use the common "fork" pattern. This gives you a complete new full-stack copy ready to work on. With new GitHub integration that's going to fit existing developer workflows too, opening up new ways to make and accept community contributions via Pull Requests.

Under the bonnet, I expect a URL simply references the container configuration resource. Meaning not only low server costs for StackBitz but also, as static assets, they can be pushed to the edge. Thus providing very low latency for an even faster developer experience.

# Other benefits

This is much more than hyper cool technology. It will have a huge impact on how developers develop as indicated above.

There are some good security gains. For example there's less over-the-wire traffic. The code also runs in the deliberately secure browser sandbox. There's also no server attack service to secure and currently WebContainers expose very few ports with access limited to the same browser.

I'm convinced part of the reason the web exploded was "View Source" enabling developers to see how code worked. StackBlitz expands the visibility to the server code as well. Of course the caveat is that much terrible code got copied.

For educational uses, embedding a URL in a course, blog or even a presentation provides instant access to running code! What could be better.

The compute model is basically Bring You Own Device (BYOD). There's no need to pay high PAYG server runtime costs.

# Limitations

For now at lest, the backend is limited to a nodejs server. That's not a big restriction but there are many other options out there.

There is no one-click deploy of the server side, but it's not hat hard. Static websites, for example SSG generated are easy enough as you can push to git and use the repository in a CI?CD service like Vercel or Netlify. The backend code can also be deployed to PaaS or other environments letting

However, I expect serverless will prove ot be the best fit. After all mapping a stateless service to multiple serverless functions is easy enough. Deployment could then be automated, perhaps using something like the Serverless project for vendor neutrality.

# Blue sky

It's going to be exciting to see what new ideas this development seeds.

For example, how about multiple WebContainers running locally in browser providing microservice like architectures?

# Summary

In case you can't tell I'm *really* exited by StackBlitz WebContainers. I love the web with a geeky passion. I'm fascinated by developer tooling, workflows and system architectures (which often mirror company structures). In my career I've worked on embedded RTOSs, digital communications, systems code and full stack web. WebContainers bring everything together in one powerful concept. I expect great things.
