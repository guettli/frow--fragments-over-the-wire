# HTML over the wire

A list of web frameworks which receive HTML snippets from the server.

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
* You can stick to one programming language. If I can avoid JavaScript, than I will avoid it.
* Testing is simplified. You don't need to test your JS code, if there is no JS code.
* SEO: You can be sure that all content which is not loaded ondemand/lazy is visible and indexable by search engine bots. Modern search engines can execute JavaScript, but it is bit unclear how far this goes. If you send JSON from the server to the client, it could be the case that a search engine does not index this data properly. 

# Libraries

* [turbolinks](https://github.com/turbolinks/turbolinks)
* [htmx](https://github.com/bigskysoftware/htmx)
* [Stimulus](https://stimulusjs.org/)
* [unpoly](https://github.com/unpoly/unpoly)

# Turbolinks

[turbolinks](https://github.com/turbolinks/turbolinks) reloads the whole page with a XMLHttpRequest. If this page contains additionaly sources (like a new JS/CSS file), then Turbolinks loads it. 

# HTMX

[htmx](https://github.com/bigskysoftware/htmx) is the successor to intercooler.js. It swaps parts of the page (not the whole page like Turbolinks).

Example:

```
<button hx-post="/clicked" hx-swap="outerHTML">
    Click Me
  </button>
  ```

# Stimulus

You sprinkle your HTML with controller, target, and action attributes. Then you write a compatible controller to process the event.

It looks dated, since the last release is almost two years old (Januaryx 2019).

# Unpoly

[Unpoly](https://github.com/unpoly/unpoly) allows you to swap parts of the page, but provides many additional features.

Some of the additional features:

* Faster response times: Outsmart latency by preloading pages, following links earlier, caching responses and keeping a persistent CSS / JS environment.
* Branch off interactions into modals, popups or drawers. Return to the main page when you're done.
* Validate forms against server rules while filling in fields.

# Conclusion

Reloading the whole page via XMLHttpRequest is not enough. Turbolinks is out. HTMX is simple, that's nice, but Unpoly gives me much more. I will use Unpoly.


# Related

* [We're breaking up with JavaScript Frontends](http://triskweline.de/unpoly-rugb/#/). These slides contain nice charts visualizing the growth of complexity during the last years.
* [Marcus Obst: New Wave of HTML](https://marcus-obst.de/wiki/Javascript%20-%20New%20Wave%20of%20HTML)
* [Building GitHub-style Hovercards with StimulusJS and HTML-over-the-wire](https://boringrails.com/articles/hovercards-stimulus/)

# WOL

Some other articles I wrote: [Thomas working out loud](https://github.com/guettli/wol)

# Feedback needed!

Do you know a fancy html-over-the-wire library, or is there something wrong or missing?

Please speak up: Create an issue at github, or send me an email: guettliml@thomas-guettler.de








