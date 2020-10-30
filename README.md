# HTML over the wire

A list of frameworks which receive HTML snippets from the server.

# Current hype: JSON over the wire

Most current frameworks decouple the backend and the frontend:
The backend provides an API (REST or GraphQL) and sends data (usualy in JSON data format)
to the client. The client renders this data via HTML.

Popular examples: React, Angular, Vue, ....

```
Server ---[JSON]--> Client (React/Angular/Vue) --> HTML
```

This has several benefits:

* Developers don't need to know everything. Some can focus on the backend, some can focus on the frontend.
* The same API could be used for several purposes: Once by the frontned in the browser, once for intergrating a third party application.
* With ServiceWorkers and IndexedDB you can write web applications which work even if the network connection is down. Example: You can create a blog post while being offline. It gets uploaded as soon as you are online again.
* With tools like react-native you can create native apps for mobile devices.
* ...

# New hype: HTML over the wire

```
Server ---[HTML]--> Client
```

Like infected with gold fever, people seem to be incapable to differentiate whether JS-over-the-wire is worth the extra effort.

I am very happy that today (autumn 2020) more developers realize that there is a way to keep things simple by sending html snippets/fragements
from the server to the client.

The next hype is coming: HTML over the wire

I think html-over-the-wire has these benefits:

* Much smaller technology stack. It is much easier to learn (for example onboarding new employees).
* Input validation needs to be done on the server side anyways. No need to re-invent this in the frontend. If you use a library like the django forms library you get both in one step: A form in HTML format and a way to validate the input the server receives.
* You can stick to one programming language. I like Python, it is simple and obvious. If I can avoid JavaScript, than I will do it.
* Testing form and input handling is simplified. For example you can use fake client to test your form handling [Django Client.post()](https://docs.djangoproject.com/en/dev/topics/testing/tools/#django.test.Client.post). 
* SEO: You can be sure that all content which is not loaded ondemand/lazy is visible and indexable by search engine bots. Modern search engines can execute JavaScript, but it is bit unclear how far this goes. If you send JSON from the server to the client, it could be the case that a search engine does not index this data properly. 

Drawbacks of html-over-the-wire:

* You can't write offline-first web apps.
* You could write a native app this way (with the help of a wrapper), but it is likely that you won't get it into the Apple Store.


# Libraries

* [htmx](https://github.com/bigskysoftware/htmx)
* [unpoly](https://github.com/unpoly/unpoly)

# Related

* [We're breaking up with JavaScript Frontends](http://triskweline.de/unpoly-rugb/#/). These slides contain nice charts visualizing the growth of complexity during the last years.

# WOL

Some other articles I wrote: [Thomas working out loud](https://github.com/guettli/wol)

# Feedback needed!

Do you know a fancy html-over-the-wire library, or is there something wrong or missing?

Please speak up: Create an issue at github, or send me an email: guettliml@thomas-guettler.de








