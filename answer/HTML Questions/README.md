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
* What are `data-` attributes good for?
* Consider HTML5 as an open web platform. What are the building blocks of HTML5?
* Describe the difference between a `cookie`, `sessionStorage` and `localStorage`.
* Describe the difference between `<script>`, `<script async>` and `<script defer>`.
* Why is it generally a good idea to position CSS `<link>`s between `<head></head>` and JS `<script>`s just before `</body>`? Do you know any exceptions?
* What is progressive rendering?
* Have you used different HTML templating languages before?
