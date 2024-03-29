# FrOW: HTML Fragments over the Wire

# What is HTML Fragments over the Wire?

To understand FrOW I would like to have a short look at the history of web development.

* The old way: Full page
* Current hype: SPA ("JSON over the wire" together with React or Vue)
* static site generators

# The old way: Full page


In the "good old days" before SPA (Single Page Applications) the browser loaded whole pages from the server.

The user could send data to the serve by filling out a form, and [Post/Redirect/Get](https://en.wikipedia.org/wiki/Post/Redirect/Get) was used to process the data.

The full page reload is slow and introduces navigational issues. Imagine you scrolled to a particular place on the page, and then you fill in a form. After the submit your browser would get a redirect and then reload the fresh page again. Now the position on the page was reset. That's not what you want.

# Current hype: SPA (JSON over the wire)

Most current frameworks decouple the backend and the frontend:
The backend provides an API (REST or GraphQL) and sends data (usually in JSON data format)
to the client. The client renders this data via JS.

Popular examples: React, Angular, Vue, ....

```
Server ---[JSON]--> Client (React/Angular/Vue) --> HTML
```

This has several benefits:

* Developers don't need to know everything. Some can focus on the backend, some can focus on the frontend.
* The same API could be used for several purposes: Once by the frontend in the browser, once for integrating a third party application.
* With tools like react-native you can create native apps for mobile devices.
* ...

But if this is so great, why was SSR (server side rendering) invented? I won't answer this question, there are thousand blog posts copying the reason from each other to hype this new thing.

# static site generators

If you internet page is just a digital business card, you don't need dynamic content. Even for blogs with just
few new pages per week you don't need an solution which executes code on the server. 

SSG (static site generators) are great for readonly pages. In this case it does not matter much if you use a SPA or
if you create several pages.

# FrOW: HTML Fragments over the Wire

Different use-cases need different solutions. And I think most software projects are between both ends.

As soon as you want to receive some input from the user, the SSG approach with pure static content does not work anymore.

For big companies, which have several software development teams, the clear cut between front-end and back-end is fine.

But for mid-sized proejcts this cut can introduce an additional overhead.

I am very happy that today (autumn 2020) more developers realize that there is a way to keep things simple by sending html snippets/fragments
from the server to the client.

It usualy works like this: The first http response to the client is a full html page. This response contains a head and a body tag. This page does not need JS to be rendered. This provides great web vitals since the screen can be rendered immediately (without waiting for JavaScript loading data from JSON-APIs).

Then updates to the page get done via html fragments. These fragments usualy don't contain a head or body tag.

```
Server ---[HTML-Fragment]--> Client
```

I think html-over-the-wire has these benefits:

* Much smaller technology stack. It is much easier to learn (for example onboarding new employees).
* Input validation needs to be done on the server side anyways. No need to re-invent this in the frontend. If you use a library like the django forms library you get both in one step: A form in HTML format and a way to validate the input the server receives.
* You can stick to one programming language. If I can avoid JavaScript, than I will avoid it.
* Testing is simplified. You don't need to test your JS code, if there is no JS code.
* SEO: You can be sure that all content which is not loaded on-demand/lazy is visible and indexable by search engine bots. Modern search engines can execute JavaScript, but it is bit unclear how far this goes. If you send JSON from the server to the client, it could be the case that a search engine does not index this data properly. 

Call it SSR (Server side rendering) or not. If I compare modern SSR frameworks like Nextjs/Nuxtjs with a 
Django/Rails application plus htmx/unpoly/hotwire, then one thing is clear for me: html-over-the-wire is boring and simple. That's why I will choose it.

# Use case

Imagine you have one html page, and this page contains three forms. If the user submits one form, the page should not get reloaded. Only one form
should get submitted, the other two forms should not send data to the server. After the form was processed on the server, the HTML fragment returned by the 
server should be displayed.

# Do it yourself

Maybe you don't need React/Vue or an other fancy thing. You can create great pages with HTML+CSS+JS.

You can implement html-over-the-wire yourself with [XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest), [fetch()](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) or a library like [axios](https://github.com/axios/axios).

But have a look at the libraries below, they provide you with nice features which help you do things in a declarative way (by using html attributes),
instead of writing code (imperative).

# Libraries

* [htmx](https://github.com/bigskysoftware/htmx)
* [unpoly](https://github.com/unpoly/unpoly)
* [VanillaJS](http://vanilla-js.com/)
* [hotwire](https://hotwired.dev/)
* [Structured Page Fragments](http://youtube.github.io/spfjs/)


## HTMX

[htmx](https://github.com/bigskysoftware/htmx) is the successor to intercooler.js. It swaps parts of the page (not the whole page like Turbolinks).

Example:

```
<button hx-post="/clicked" hx-swap="outerHTML">
    Click Me
  </button>
  ```

## Unpoly

[Unpoly](https://github.com/unpoly/unpoly) allows you to swap parts of the page, but provides many additional features.

Some of the additional features:

* Faster response times: Outsmart latency by preloading pages, following links earlier, caching responses and keeping a persistent CSS / JS environment.
* Branch off interactions into modals, popups or drawers. Return to the main page when you're done.
* Validate forms against server rules while filling in fields.

Written in Coffeescript.


## VanillaJS

VanillaJS is a joke. It does not really exist. It just means: Don't use a framework, use HTML+CSS+JavaScript.

See [Do it yourself](#do-it-yourself)

## Hotwire

[Hotwire](https://hotwired.dev/) builds upon [turbo](https://turbo.hotwired.dev/). The primary focus seems to be
to deliver page changes over WebSocket. I don't plan to create a WebSocket based solution. Maybe hotwire is really great.
I don't know. I have not found an easy to understand example while browsing their pages.

## Structured Page Fragments 
[Structured Page Fragments](http://youtube.github.io/spfjs/) is the framework used by youtube before the polymer update. It sends a mix of JSON and HTML over the wire.

# Rethinking “Smart” re-use of APIs

If you use the html-fragments-over-the-wire approach, you don't need a JSON-API for your frontend. 

First a short story: Several years ago, when I hear the first time of Angular, I was amazed. I thought:

> This is the future: The server sends clean JSON to the client. And this API can be used for the GUI and for a machine-to-machine communication (RPC). 

But this is nonsense. You want to improve a GUI daily. But an API for machine-to-machine communication must be stable. Have you have thought about doing A-B-testing for a RPC?

Related slide: [Rethinking “Smart” re-use of APIs](https://docs.google.com/presentation/d/12dgaBnUgl4cmEkiOhUJL5hsbGQ6hB5sslDuozmBjVUA/edit#slide=id.gb6b0109123_0_2)

If you want performant machine-to-machine communication, then you might want to use gRPC and not a JSON based solution.



# Conclusion

I settled on htmx. I developed a prototype with it, and it is straight forward, well-documented and has a healthy community.

# Related

* [HTMX Frontend Revolution (Slides)](https://docs.google.com/presentation/d/12dgaBnUgl4cmEkiOhUJL5hsbGQ6hB5sslDuozmBjVUA/edit?usp=sharing)
* [We're breaking up with JavaScript Frontends](http://triskweline.de/unpoly-rugb/#/). These slides contain nice charts visualizing the growth of complexity during the last years.
* [Marcus Obst: New Wave of HTML](https://marcus-obst.de/wiki/Javascript%20-%20New%20Wave%20of%20HTML)
* [Building GitHub-style Hovercards with StimulusJS and HTML-over-the-wire](https://boringrails.com/articles/hovercards-stimulus/)
* [Using HTML as the Media Type for your API](https://blog.jonm.dev/posts/using-html-as-the-media-type-for-your-api/)
* [Django htmx fun](https://github.com/guettli/django-htmx-fun) A simple demo application how to use htmx with Django.

# WOL

Some other articles I wrote: [Thomas working out loud](https://github.com/guettli/wol)

# Feedback needed!

Do you know a fancy html-over-the-wire library, or is there something wrong or missing?

Please speak up: Create an issue at github, or send me an email: guettliml@thomas-guettler.de








