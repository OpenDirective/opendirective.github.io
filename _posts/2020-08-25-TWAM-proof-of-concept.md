---
layout: post
title: TWAM Proof of Concept
---

<div class="message">
A new project with yet a different stack. This time it's a Jamstack static site with a serverless back end using many of Netlify's services. On this platform Eleventy provides templated page build with no front end framework in sight. Google Sheets is used as a database. This post explains some of the technical decisions made and challenges met.
</div>

## Digitising Tools with a Mission's process

Tools with a Mission ([TWAM](https://www.twam.uk/)) recycle and refurbish tools, sewing machines and computers. It then sends them to the developing world for livelihood creation. The processes are reasonably complex and currently use a mixture of Google Apps tools, including GMail and Sheets. I'm working in a small team to help move more of the process to an integrated 'web app' experience. This will support applications for tools, evaluation, approval and other steps through to shipping in containers.

This is a pretty standard setup of app with forms plus a back office with a clear workflow. Thus we looked at the usual candidates for forms and workflow. I personally find such systems to be a quite restrictive, especially if thy use a GUI and cannot be data or code driven. Given TWAM's embedded use of GApps we also considered Google Actions script which is basically a hosted JavaScript environment with some support for creating web apps and APIs.

In the end however, my positive experience with using Netlify Deployments along with Eleventy for https://musicpracticetools.net meant I was keen to try more of the Netlify portfolio of services. However, for the database, rther than the promoted no SQL Fauna, I decided to try using Google Sheets. Using sheets should open up the possibility of easy access to the data and ad-hoc processing using office software skills rather than full developer skills. In addition, email is used for inter person messaging and for automatic notifications. Again, why code a new solution when a good one exists and fits in with people work patterns. So the stack chosen for the proof of concept is:

* Eleventy with nunjucks templates
* Good ol' HTML, CSS and Javascript - ie no framework
* Google Sheets API
* Email
* Netlify deploys, forms, functions, identity and role based gating
* Netlify dev

This supports a representative slice of functionality including user management, form filling with editing and notifications. The proof of content flow is: someone makes an application and can check back later to see if successful Someone else reviews a subset of applications filtered by country and adds evaluation notes and assigns an accept/reject status.

## Tricky areas

I'd already ironed out any small issues with Eleventy and nunjucks in my last project so just used the same config again. Netlify deploys are smooth with github integration and pretty much just work; as do the forms and functions which wrap AWS Lamda, making them a "doddle" to use.

The fun and games came with Google Sheets and Netlify identity used in conjunction with gating.

### Google Sheets

Google provide a REST API for Sheets and the Action Script environment with support for creating HTTP APIs. They also provide a npm package binding to ALL their APIs, though there is a simpler independent package just for sheets. We can most easily call the JavaScript binding from our Netlify Functions rather than poking the REST API via fetch. I decided to use the simpler `google-spreadsheet` package, though it turns out I might switch to the Google monster package as it supports batching API calls for efficiency.

But we soon hit an major issue. For some reason the latest v4 of the Google API does not support any filtering when fetching sheet rows. And v3 is about to be dropped. That's obviously a scalability problem! We donet want to load all columns and rows into memory for processing. I looked at using Action Script to create an API which does work but there are two issues. 1) the script have to be edited in Google's in browser editor which is a terrible experience compared to something like VS Code. Perhaps more serious is the fact that in order to expose the API endpoint the Google Apps setting to allow anyone to share items publicly must be enabled. That's a potential security issue and might not be welcome.

After much head scratching I found a work around to this. YOu can use a sheet formula to process the sheet data as required and return the filtered row set. Further more, I decided to reduce coupling and error possibilities, that rather than keeping the formula in the spreadsheet we'd push it from the app and read the results back. In addition to have chance of handling multiple accesses at once a new sheet is created and then deleted. So the process to get a filtered row set is

1) Create a new worksheet in the spreadsheet
2) Push the Formula to it
3) Fetch the results
4) Delete the worksheet

This seems to work well but does seem a little "hacky" and I need to test with multiple simultaneous updates. It is also slow, taking around 4 s. This appears to be purely communication time with so many round trips as adding a thousand rows to the sheet makes little difference. The official Google npm package provides batching mode which should speed things up. You need some good sheet formula skills but can develop in the sheet and move to JavaScript once it all works. In order to further reduce coupling and hardcoded values the formula returns the size of the result set and column names as well as the data itself to the app. Th eapp can rad that metadata and use it to fetch the correct cells.

### Netlify Identity and Role Based Gating.

Identity along with account management is always a complex topic. Fortunately, Netlify provide a register / login widget that is perfectly serviceable, though nowhere near as flexible or polished as the one from Auth0. I found I could improve the UX to my liking with a little script, though have not yet succeeded with the CSS tweaks I want. This is based on JWT tokens as you pretty much expect these days. These are flexible and you can pass the user's id_token to your Functions to provide access control. This way Netlify manages the users and their details are passed around in the tokens.

The feature that I really appreciate is Role Based gating. This means access to specific app page URIs is controlled right on the CDN edge without any function code required. When a user logs in a Function is called and this can return the users roles. Then a config file is used to define which roles are required per URI. And it just works.

Well almost. I hit a lot of strange behavours and errors with access, including an apparent failure when logging in on another browser when still logged in. This is where Netlify's Community forum came to the rescue in the form of one Jon Sullivan. As a volunteer Jon is incredibly knowledgeable and definitely goes the extra mile, and then some. After a long an fascinating discussion covering how authentication and gating interact and how to handle refresh tokens I now have code which is reliable. It just took a very long time to get there.

## Summary

A important point I should make clear is that ALL this functionality from Netlify is on the free plan! that's very impressive indeed and ideal for a charity with very little cash to spare.

The stack threw me a few big challenges but now it is working well and I think will scale well to the final app.

The code is available at https://github.com/OpenDirective/TWAM-App
