# fastRWD
RWD draft to load fast: https://www.filamentgroup.com/lab/performance-rwd.html

When discussing how multi-device code correllates to page loading performance, one of the primary measures developers typically point to determine success is total page weight. But weight doesn’t necessarily need to increase the time a user needs to wait to use a page. A page typically becomes usable much sooner than when it finishes loading. How we load assets matters just as much as how many assets we’re loading.

A more useful benchmark for evaluating page speed from the user's perspective is the time it takes for a page to become usable. This is “perceived performance” , because it refers to the performance metric that is most easily perceived by the naked eye, and likely to be most meaningful metric to our users.

To measure perceived performance, we need to find out how long it takes for a page to start rendering visually in the browser. Webpagetest.org is a fantastic tool you can use for this purpose.

Webpagetest requests the site in a real browser and device, analyzes how it loads, and provides tons of information. Some portions of Webpagetest’s results are directly related to perceived performance, for example, “Start Render”.

Rendering a complex layout this quickly requires careful considerations
## Shortening the critical path
“critical path” is used to describe the time between when a page is requested and rendered. The loading process is like a path between point A and point B that requires a number of steps to complete. Many of these steps are under our control.

For example, CSS and JavaScript requests can significantly increase the time it takes a page to render. That’s because by default, browsers will delay page rendering until they finish loading, parsing, and executing all of the CSS and JavaScript files referenced in the head of the page.

Ideally, we want to shorten our critical path so that it can be completed in the fewest, shortest steps possible and without any detours (read: external requests). Additionally, we may want to tell the browser that it can handle certain steps independently of rendering the page. We can instruct the browser to perform its page loading steps in one of two ways: either request files asynchronously so that they can load and execute while the page is being rendered, or include the code inline directly in our HTML page.

## Going async
One approach to avoiding blocking requests is to request files in an asynchronous manner so that they load and execute on their own schedule, independent of page rendering. For JavaScript files, we can do this easily in modern browsers by adding an async attribute to a script element.

But... async is only supported in the latest browsers (IE 10+), and, a script element offers us no means of qualifying whether a request should be made in the first place (we typically only load our DOM framework and other scripting enhancements in browsers that support certain features, after all). For these reasons, we typically add a small bit of JavaScript to request our files asynchronously: we maintain loadJS for just this purpose.

As for asynchonously requesting CSS files: HTML offers no native means of requesting a stylesheet asynchronously, so we need to use a bit of JavaScript to make an asynchronous stylesheet request as well. We maintain the loadCSS project for just this purpose.

## Inlining code
Another way to avoid making blocking external requests is to include the contents of files you would have requested directly in the HTML document. This is known as inlining. CSS files can be inlined in an HTML document inside a style element, and JavaScript files can be inlined inside a script element.

Inlining is great in that it saves us from making slow, render-blocking requests, and it still allows us to execute code before the page is rendered (which is desirable in some cases). But inlining also has some downsides in that it prevents the browser from caching the code itself for reuse on subsequent pages, and it’s also very easy to overuse, as inline code still needs to be parsed before the page content can be rendered so you don’t want to include anything unnecessary. Basically, inlining is useful for critical portions of CSS and JS that need to be parsed before the page is rendered, but we want to use it sparingly.

## Which code should I "inline"
In some cases, blocking page rendering while CSS and JavaScript is parsed is desirable. For example, we want the browser to load all of the CSS necessary for the page to render before the browser starts rendering it, because if we don’t, users will see a flash of unstyled content. Also, some JavaScript works best if it executes before a page renders as well, such as an HTML5 shim that enables older versions of IE to recognize new HTML elements as it parses them (feature tests and class additions are other examples).

So generally, a lot of our CSS and JS can be safely requested asynchronously, but usually not all of it. The “critical” parts that need to be present for rendering the initial page should be inlined in our HTML. The trick is figuring out how to divide it up between inline and async.
