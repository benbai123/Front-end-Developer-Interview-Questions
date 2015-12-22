#### HTML Questions:

* What does a `doctype` do?
  * Ans: Short: Tell browser the HTML version of your page. Long: The `<!DOCTYPE>` declaration must be the very first thing in your HTML document, before the `<html>` tag. The `<!DOCTYPE>` declaration is not an HTML tag; it is an instruction to the web browser about what version of HTML the page is written in. In HTML 4.01, the `<!DOCTYPE>` declaration refers to a DTD, because HTML 4.01 was based on SGML. The DTD specifies the rules for the markup language, so that the browsers render the content correctly. HTML5 is not based on SGML, and therefore does not require a reference to a DTD.
  Tip: Always add the `<!DOCTYPE>` declaration to your HTML documents, so that the browser knows what type of document to expect.
  * Ref: [w3school](http://www.w3schools.com/tags/tag_doctype.asp)
  * !Ref: as usual, things usually more complex with some more detail (probably you never need them) [Activating Browser Modes with Doctype](https://hsivonen.fi/doctype/)
* What's the difference between standards mode and quirks mode?
  * Ans:
    * In quirks mode, layout emulates nonstandard behavior in Navigator 4 and Internet Explorer 5. This is essential in order to support websites that were built before the widespread adoption of web standards.
    * In full standards mode, the behavior is (hopefully) the behavior described by the HTML and CSS specifications.
    * I guess just need to follow standard today.
    * Ref: [Quirks Mode and Standards Mode (MDN)](https://developer.mozilla.org/en-US/docs/Quirks_Mode_and_Standards_Mode), [What is quirks mode? (SO)](http://stackoverflow.com/questions/1695787/what-is-quirks-mode)
* What's the difference between HTML and XHTML?
  * Ans: In XHTML
    * XHTML DOCTYPE is mandatory
    * The xmlns attribute in `<html>` is mandatory
    * `<html>, <head>, <title>, and <body>` are mandatory
    * XHTML elements must be properly nested
    * XHTML elements must always be closed
    * XHTML elements must be in lowercase
    * XHTML documents must have one root element
    * Attribute names must be in lower case
    * Attribute values must be quoted
    * Attribute minimization is forbidden
  * Ref: [HTML and XHTML (w3school)](http://www.w3schools.com/html/html_xhtml.asp)

* Are there any problems with serving pages as `application/xhtml+xml`?
  * Ans:
    * Serving pages as application/xhtml+xml triggered the XML parser instead of the HTML parser which is the "proper" way to parse XHTML, and if you had a single mistake (not follow XHTML) anywhere in your markup the browser would render an error page instead of working around it.
  * Ref: [What are the problems associated with serving pages with Content: application/xhtml+xml (SO)](http://stackoverflow.com/questions/351380/what-are-the-problems-associated-with-serving-pages-with-content-application-xh), [Article at Haker News](https://news.ycombinator.com/item?id=8917715)
* How do you serve a page with content in multiple languages?
  * Ans: 
    * Making sure the language is identified in the code of the page
      * Setting the primary language by apply language code to html element, e.g. `<html lang="en">` for html or `<html xmlns="http://www.w3.org/1999/xhtml" lang="en" xml:lang="en">` for xhtml
    * Handling multiple language properly
      * Change lang of specific block of your content if needed, e.g., `<blockquote lang=”fr”><p>Le plus grand faible des hommes, c'est l'amour qu'ils ont de la vie.</p></blockquote>`
      * Provide information with respect to the page of a link linking to, e.g. `<a href="" hreflang="fr">French</a>` (this link linking to a page that primary language is French) or `<a href="" lang="fr" hreflang="fr">Francais</a>` (this link linking to a French page and the link text is written in French)
    * Note Google tries to work out the main languages of your pages itself. In order to make language identification easier for Google, Google recommends only using one language per page.
    * Modify language direction properly
      * e.g. `<html dir="rtl">` (right-to-left) for Arabic, Persian and Urdu.
      * Also need to change layout (e.g. right aligned, block order, etc, by CSS) properly
    * Specify Character encoding, usually unicode `<meta http-equiv="Content-Type" content="text/html;charset=UTF-8">` (HTML 4) `<meta charset="UTF-8">` (HTML 5)
    * Different language probably need different font-size (and/or font-family)
      * Use pseudo class, e.g. `:lang(en) {font-size: 85%;} or :lang(zh) {font-size: 125%;}` (CSS) for `<html lang="en"> or <html lang="zh">` (HTML)
      * Specify class based on language, e.g. `<body class="en"> or <body class="zh">` then you can specify styles in CSS.
    * Content written in one language may take up more or less space on the page than another language.
      * Make sure your layout has enough flexibility
      * Or adjust the content for different language properly.
    * Ref: [7 Tips and Techniques For Multi-lingual Website Accessibility](http://www.nomensa.com/blog/2010/7-tips-and-techniques-for-multi-lingual-website-accessibility/)
* What kind of things must you be wary of when design or developing for multilingual sites?
  * Ans: I am not familiar with Design but here is a good ref, just list something from it:
    * Use a common default language (e.g., English, instead of Latin)
    * Provide good UI for change language
    * Apply different language based on IP (probably not a good idea, e.g., for traveler)
    * You'll need a mechanism to load different image by lang if you translate and make different images (with text) for different languages.
    * Again take care of text length.
    * Also take care of colors, ref: [Colours in Culture](http://infobeautiful4.s3.amazonaws.com/2015/05/1276_colours_in_culture.png)
    * Avoid hard-coded the contents that need to be translated.
    * Date (and Number) formate probably different in different language.

  * Ref: [
What kind of things one should be wary of when designing or developing for multilingual sites?](https://www.quora.com/What-kind-of-things-one-should-be-wary-of-when-designing-or-developing-for-multilingual-sites)
* What are `data-` attributes good for?
  * Ans: It is good for put any custom data on dom element, let us store extra information on standard without any other tricks.
    e.g.,
    ```html
      <button data-modal="#modal">show modal</button>
    ```
    Where data-modal denotes the selector used to select a div which is a modal box that will show up when button clicked.
* Consider HTML5 as an open web platform. What are the building blocks of HTML5?
  * Ans:
    * more semantic text markup
    * new form elements
    * vedio and audio
    * new javascript API
    * canvas and SVG
    * new communication API (WebSocket, SSE)
    * geolocation API
    * web worker API
    * new data storage
  * Ref: [HTML Questions:Front-end Developer Interview Questions](http://flowerszhong.github.io/2013/11/20/html-questions.html)
* Describe the difference between a `cookie`, `sessionStorage` and `localStorage`.
  * Ans:
    * Cookies are primarily for reading server-side, local storage (including `localStorage` and `sessionStorage`) can only be read client-side
    * Cookies will be sent in each HTTP header, the data in local storage will not.
    * Cookies give you a limit of 4096 bytes, local storage is as big as 5MB per domain.
    * Cookies must have expiration date, `localStorage` stores data with no expiration date, `sessionStorage` stores the data for only one session. The data stored in `sessionStorage` is deleted when the user closes the specific browser tab.
  * Ref: [HTML5 Local Storage](http://www.w3schools.com/html/html5_webstorage.asp), [Local Storage vs Cookies](http://stackoverflow.com/questions/3220660/local-storage-vs-cookies).
* Describe the difference between `<script>`, `<script async>` and `<script defer>`.
  * Ans:
    * The `<script>` tag is used to define a client-side script, such as a JavaScript. The `<script>` element either contains scripting statements, or it points to an external script file through the src attribute.
    * `<script async>` (or `<script async="async">` for xhtml): It specifies that the script will be executed asynchronously as soon as it is available. The script is executed asynchronously with the rest of the page (the script will be executed while the page continues the parsing)
    * `<script defer>` (or `<script defer="defer">` for xhtml): It specifies that the script is executed when the page has finished parsing. If async is not present and defer is present: The script is executed when the page has finished parsing
    * Note async and defer are only for external scripts.
  * Ref: [HTML script Tag](http://www.w3schools.com/tags/tag_script.asp), [HTML script async Attribute](http://www.w3schools.com/tags/att_script_async.asp), [HTML script defer Attribute](http://www.w3schools.com/tags/att_script_defer.asp).
* Why is it generally a good idea to position CSS `<link>`s between `<head></head>` and JS `<script>`s just before `</body>`? Do you know any exceptions?
  * Ans (short for both): In short, (generally) CSS `<link>` usually contains information that Browser need them to render most of HTML content and JS `<script>` doesn't.
  * Ans (for link):
    * From [HTML spec](http://www.w3.org/TR/html401/struct/links.html#edef-LINK), link may only appear in the head section in HTML 4.1, link can appear in the body section with the "itemprop" property in HTML5.
    * Also, if you leave the the styles somewhere in the <body>, the browser has to re-render the page (new and old when loading) when the styles declared has been parsed.
    * Put link in head help you to prevent FOUC.
  * Ans (for script):
    * script without `async` or `defer` will block parser to stop parsing the other HTML on your page, which may incur one or more network roundtrips and delay the time to first render of the page.
  * Exception (for link):
    * There is a case with respect to RWD, assume you have lots of media-query rules, browser will simply download all of them by default. If you really want to download only what you exactly need, you probably will want download CSS file with JS by some tricks. Two demo page below shows the two different cases.
    * Samples:
      * General media query: [Sample](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/HTML%20Questions/CSS%20media%20query%20general/media_query_test.html), [Sources](https://github.com/benbai123/benbai123.github.io/tree/master/examples/Front-end-Developer-Interview-Questions/HTML%20Questions/CSS%20media%20query%20general)
      * Tricky media query: [Sample](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/HTML%20Questions/CSS%20media%20query%20tricky/media_query_test.html), [Sources](https://github.com/benbai123/benbai123.github.io/tree/master/examples/Front-end-Developer-Interview-Questions/HTML%20Questions/CSS%20media%20query%20tricky)
        * Put the link to load in CSS content property. (see base.css)
        * Load CSS file as needed by JS. (see html file)
      * Note: Tricky way probably not a good way, don't focus on it.
  * Exception (for script): There are more cases for script
    * When the script is 3rd-party library with limitation so you cannot manage its position yourself (e.g., Modernizr.js), this is an exception.
    * If most of your script is ok with `async` or `defer`, put them at the top will not block parser and can load them earlier.
    * If the script is the major content provider (e.g., the content is map rendered by Google Map API which support async load).
    * If the style will be changed and causes lots of repaint during the script execution (just like Modernizr)
    * You need the script to render your content, e.g., your content is a set of number and you need to render them with some JS Charts library.
    * In short, when you cannot control it, when defer/async allowed or when the script is important for render content.
  * Ref:
    * For CSS: [What's the difference if I put css file inside <head> or <body>?](http://stackoverflow.com/questions/1642212/whats-the-difference-if-i-put-css-file-inside-head-or-body), [load external css file in body tag [duplicate]](http://stackoverflow.com/questions/4957446/load-external-css-file-in-body-tag)
    * For JS: [Where is the best place to put <script> tags in HTML markup?](http://stackoverflow.com/questions/436411/where-is-the-best-place-to-put-script-tags-in-html-markup), [Remove Render-Blocking JavaScript](https://developers.google.com/speed/docs/insights/BlockingJS).
* What is progressive rendering?
  * Ans: The main idea is render a visible content as soon as possible, but the initial content probably has lower quality or is not complete, then load more data to improve its quality or make it complete. e.g., Progressive image rendering.
  * Ref: [What is progressive rendering?](https://coronarenderer.freshdesk.com/support/solutions/articles/5000516260-what-is-progressive-rendering-), [The Lost Art of Progressive HTML Rendering](http://blog.codinghorror.com/the-lost-art-of-progressive-html-rendering/)
* Have you used different HTML templating languages before?
  * Ans: I've used JSP, JSF, ZK, Struts, node with express, Rails to write either pure HTML page or mixin with backend template Engine, also tried the template (either hard coded string or load external html file) of AngularJS.
  * Actually I am not sure what does HTML templating languages mean, if it means something like backbone or embr then the answer is "Just tried AngularJS".
