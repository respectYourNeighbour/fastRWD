# fastRWD
RWD draft to load fast: https://www.filamentgroup.com/lab/performance-rwd.html

When discussing how multi-device code correllates to page loading performance, one of the primary measures developers typically point to determine success is total page weight. But weight doesn’t necessarily need to increase the time a user needs to wait to use a page. A page typically becomes usable much sooner than when it finishes loading. How we load assets matters just as much as how many assets we’re loading.

A more useful benchmark for evaluating page speed from the user's perspective is the time it takes for a page to become usable. This is “perceived performance” , because it refers to the performance metric that is most easily perceived by the naked eye, and likely to be most meaningful metric to our users.

To measure perceived performance, we need to find out how long it takes for a page to start rendering visually in the browser. Webpagetest.org is a fantastic tool you can use for this purpose.

Webpagetest requests the site in a real browser and device, analyzes how it loads, and provides tons of information. Some portions of Webpagetest’s results are directly related to perceived performance, for example, “Start Render”.

Rendering a complex layout this quickly requires careful considerations
## Shortening the critical path
“critical path” is used to describe the time between when a page is requested and rendered.

CSS and JavaScript requests can significantly increase the time it takes a page to render. That’s because by default, browsers will delay page rendering until they finish loading, parsing, and executing all of the CSS and JavaScript files referenced in the head of the page.
