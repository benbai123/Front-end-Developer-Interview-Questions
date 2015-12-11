#### General Questions:

* What did you learn yesterday/this week?
  * Ans: Use JDBC/MySQL to do pagination and multiple level sort. (well, not front-end)
* What excites or interests you about coding?
  * Ans: To find something new and better, to improve something, to solve something hard.
* What is a recent technical challenge you experienced and how did you solve it?
  * Ans: Trapped by the life-cycle of Rake and Testtask (ruby/rails), digged into the source code of minitest gem and customize it as needed (overwrite its reporter and do what I want) to solve it.
* What UI, Security, Performance, SEO, Maintainability or Technology considerations do you make while building a web application or site?
  * Ans:
    * UI, make Dom structure as simple as possible, but still clear architecture. Also take care of reusability, including Coponent level and Dom element level. Be careful of memory leak.
    * Security, XSS - Encoding text as needed. CSRF - Make a private contract (e.g., extra token) with back-end. Also use SSL connection if needed.
    * Performance, Reduce Dom amout end JS code to run, implement Load on Demand and/or Render on Demand component, use throttle/debounce as needed, always declare variable at proper scope.
    * SEO, use html tag (title, meta-description, a)properly, do server-side rendering if needed.
    * Maintainability, Component, Naming, CSS Pre-processing, JS Modulize, File Structure, Split HTML to Fragments, Template, Use Framework (Angular, Backbone, etc), etc
* Talk about your preferred development environment.
  * Ans:
    * OS: Windows or Ubuntu
    * IDE: Eclipse, Sublime text
    * CVS: Git, SVN
    * Others: Jenkins, Selenium, Docker, Bash/Shell
* Which version control systems are you familiar with?
  * Ans: Git, SVN
* Can you describe your workflow when you create a web page?
  * Ans: I will starts from static HTML page, then make component for reuse if needed, and discuss with Back-end with respect to API as needed within the flow.
* If you have 5 different stylesheets, how would you best integrate them into the site?
  * Ans:
    * If it means all five sheets need to be used together, I'd just simply use compress tool to minify and merge them.
    * If it means five kind of Theme, I'll give a specific class for each, wrap them under the specified class with LESS or SASS, and apply different class to body if needed.
    * basically I cannot get what the meaning of "5 different sheets" and "integrate" here, should ask for detail before answering.
* Can you describe the difference between progressive enhancement and graceful degradation?
  * Ans:
    * progressive enhancement: from static map image to google map
    * graceful degradation: from google map to static map image
    * progressive enhancement: starts from partial then including all
    * graceful degradation: including all at the begining then enhance partial of them
    * for more information refer to [w3 wiki](https://www.w3.org/wiki/Graceful_degradation_versus_progressive_enhancement)
* How would you optimize a website's assets/resources?
  * Ans: several points
    * Reduce http request: combine js and css file, combine small icons (CSS sprite), avoid redirect and iframe if possible
    * Reduce file size: minify js and css, compress img
    * Reduce/Optimize network traffic: gzip, load on demand (e.g. load when scrolled into view port) Post-load Pre-loading, etc
    * Use CDN for high availability and high performance
    * Put assets to several different domain to improve concurrent downloading number
    * But do not use too many different domain to reduce DNS lookup
    * Do not use cookies for domain of static assets to reduce request header size
    * Use the Expires Header so browser will cache it
    * Add "Version" concept to assets so can update modified assets.
    e.g. use param src="assets/logo.png?v=v2"
    or handle it by path src="assets/v2/logo.png"
    or modify file name src="assets/logo2.png" (I prefer previous two)
    * place style at the top (in head)
    * place script at the bottom (just before end tag of body)
    * refer to [Web Site Optimization: 13 Simple Steps Article](http://www.sitepoint.com/web-site-optimization-steps/)for more information
    * also more detail for [DNS Lookup](https://developer.yahoo.com/blogs/ydn/high-performance-sites-rule-9-reduce-dns-lookups-7207.html)
* How many resources will a browser download from a given domain at a time?
  * What are the exceptions?
    * Ans: It depends, see [http://stackoverflow.com/questions/7456325/get-number-of-concurrent-requests-by-browser](http://stackoverflow.com/questions/7456325/get-number-of-concurrent-requests-by-browser)
* Name 3 ways to decrease page load (perceived or actual load time).
  * Ans: CSS Sprite, minify JS/CSS files, gzip content, load on demand, post-load preloading, move JS to bottom.
* If you jumped on a project and they used tabs and you used spaces, what would you do?
  * Ans: Either use tab or still use space with an auto convertion tool (convert spaces to tabs accordingly)
* Describe how you would create a simple slideshow page.
  * Ans: 2 ways, use javascript to change scrollLeft (scrollTop) or use CSS3 animation with predefined animation frames (Use radio button with CSS selector to control the CSS-only one)
  * TODO: Do simple example for both
  * Simple slider with JavaScript: [Sample Page](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/General%20Questions/slider_js.html) Sources: [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/General%20Questions/slider_js.html), [JS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/General%20Questions/js/slider_js.js), [CSS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/General%20Questions/css/slider_js.css)
  * Simple slider with pure CSS: [Sample Page](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/General%20Questions/slider_css.html) Sources: [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/General%20Questions/slider_css.html), [CSS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/General%20Questions/css/slider_css.css)
* If you could master one technology this year, what would it be?
  * Ans: Its almosst end of this year but...JDBC I guess.
* Explain the importance of standards and standards bodies.
  * Ans:
    * importance of standards: Make all browser vendor and F2E face & follow same baseline, save our life.
    * importance of standards bodies: So standards can comes out. 
    (I do not want to talking about the benefit of "following standards" here, kinda out of scope)
* What is Flash of Unstyled Content? How do you avoid FOUC?
  * Ans: Browser display unstyled content,
    * several possible reason:
      * @import style with IE version > 5
      * bad order by generate content and apply style with JavaScript
      * bad CSS and Image pair, e.g., change separated background-image when mouseover, the new background-image will need some time to load.
    * several possible solution:
      * add link or script tag to head to avoid @import with IE one
      * hide content at the begining then show it after everything ready, either use JS to modify style or write the CSS rule to display it in the last loaded stylesheet
      * use CSS Sprite merge images for different status into one
    * more information: [wiki](https://en.wikipedia.org/wiki/Flash_of_unstyled_content), [question at SO](http://stackoverflow.com/questions/11640238/how-to-stop-flash-of-unstyled-content)
* Explain what ARIA and screenreaders are, and how to make a website accessible.
  * Ans:
    * ARIA: WAI-ARIA, the Accessible Rich Internet Applications Suite, defines a way to make Web content and Web applications more accessible to people with disabilities. It especially helps with dynamic content and advanced user interface controls developed with Ajax, HTML, JavaScript, and related technologies.
    * screenreaders: A screen reader is a software application that attempts to identify and interpret what is being displayed on the screen (or, more accurately, sent to standard output, whether a video monitor is present or not). This interpretation is then re-presented to the user with text-to-speech, sound icons, or a Braille output device. Screen readers are a form of assistive technology (AT) potentially useful to people who are blind, visually impaired, illiterate or learning disabled, often in combination with other AT, such as screen magnifiers.
    * how to make a website accessible
      * Images & animations: Use the alt attribute to describe the function of each visual.
      * Image maps. Use the client-side map and text for hotspots.
      * Multimedia. Provide captioning and transcripts of audio, and descriptions of video.
      * Hypertext links. Use text that makes sense when read out of context. For example, avoid "click here."
      * Page organization. Use headings, lists, and consistent structure. Use CSS for layout and style where possible.
      * Graphs & charts. Summarize or use the longdesc attribute.
      * Scripts, applets, & plug-ins. Provide alternative content in case active features are inaccessible or unsupported.
      * Frames. Use the noframes element and meaningful titles.
      * Tables. Make line-by-line reading sensible. Summarize.
      * Check your work. Validate. Use tools, checklist, and guidelines at http://www.w3.org/TR/WCAG
    * Ref: [Screen Reader Wiki](https://en.wikipedia.org/wiki/Screen_reader), [ARIA at w3](http://www.w3.org/WAI/intro/aria), [How to make a website accessible at w3](http://www.w3.org/WAI/quicktips/)
* Explain some of the pros and cons for CSS animations versus JavaScript animations.
  * Ans: 
    * CSS: pros - Easier on simple or 3D transform, cons - worse control,
    * JS: pros - far more flexibility, better workflow for complex animations and rich interactivity, compatible with older browser, cons - write JS usually much more complex
    * Ref: [Myth Busting: CSS Animations vs. JavaScript](https://css-tricks.com/myth-busting-css-animations-vs-javascript/)
* What does CORS stand for and what issue does it address?
  * Ans: 
    * CORS: Cross-origin resource sharing, is a mechanism that allows restricted resources (e.g. fonts) on a web page to be requested from another domain outside the domain from which the resource originated.
    * CORS defines a way in which a browser and server can interact to safely determine whether or not to allow the cross-origin request.[2] It allows for more freedom and functionality than purely same-origin requests, but is more secure than simply allowing all cross-origin requests. It is a recommended standard of the W3C.
    * e.g., Public data API provider will allow cross domain AJAX so other site can fetch its data.
    * Ref: [CORS Wiki](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)
