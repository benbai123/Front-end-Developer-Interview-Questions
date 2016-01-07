#### CSS Questions:

* What is the difference between classes and ID's in CSS?
  * Ans:
    * ID shold be unique in whole page but classes can be applied to multiple elements.
    * ID can work with url hash [Sample](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/CSS%20Questions/id_anchor_test.html#top), [Source](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/CSS%20Questions/id_anchor_test.html)
* What's the difference between "resetting" and "normalizing" CSS? Which would you choose, and why?
  * Ans:
    * "resetting" aims to remove all built-in browser styling, "normalizing" aims to make a set of default styling consistent across browsers
    * I'll choose "normalizing", it reduce the work each time you building a new website. With "resetting" you need to style more things each time. Also "normalizing" provide better look and feel before you styling anything.
* Describe Floats and how they work.
  * Ans (Short): Float take element out of flow then push it to left/right side according the value you specified, all siblings that are not floated will around floated elements, usually used with clear property to prevent weird layout.
  * Ans:
    * Float attribute "push" element to left side or right side with value "left" and "right", then other elements (if any) take ramaining spaces around the floated elements. Note that if an element only contains floated elements then need add one more clear element (with style `clear: both;`, e.g., `<div style="clear: both;"></div>`) or it will have no height.
    * You can also apply clear fix by setting the overflow CSS property on a parent element. If this property is set to auto or hidden on the parent element, the parent will expand to contain the floats, effectively clearing it for succeeding elements.
    * Or uses a clever CSS pseudo selector (:after) to clear floats. Rather than setting the overflow on the parent, you apply an additional class like "clearfix" to it. Then apply this CSS:
    ```css
    .clearfix:after { 
       content: "."; 
       visibility: hidden; 
       display: block; 
       height: 0; 
       clear: both;
    }
    ```
  * Ref: [All About Floats](https://css-tricks.com/all-about-floats/)
* Describe z-index and how stacking context is formed.
  * Ans:
    * z-index
      * The z-index property specifies the stack order of an element.
      * Note: z-index only works on positioned elements (position:absolute, position:relative, or position:fixed).
      * It also has no effect between parent and child
    * how stacking context is formed
      * stack order of an element is higher than its parent, z-index cannot change this.
      * stack order of an element is higher than its previous sibling, z-index can affect this case.
      * stack order of an element is higher with larger z-index value.
  * Some cases for investigating: [Sample](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/CSS%20Questions/stacking_context_test.html), [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/CSS%20Questions/stacking_context_test.html), [CSS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/CSS%20Questions/css/stacking_context_test.css)
* Describe BFC(Block Formatting Context) and how it works.
  * Ans:
    * boxes are laid out one after the other, vertically, beginning at the top of a containing block.
    * The vertical distance between two sibling boxes is determined by the 'margin' properties. Vertical margins between adjacent block-level boxes in a block formatting context collapse (if the boxes not establish new BFC).
    * In a block formatting context, each box's left outer edge touches the left edge of the containing block (for right-to-left formatting, right edges touch). This is true even in the presence of floats (although a box's line boxes may shrink due to the floats), unless the box establishes a new block formatting context (in which case the box itself may become narrower due to the floats).
    * BFC works well with floated elements even no clear fix.
    * Text within BFC will not wrap under floated element.
    * BFC can help to prevent unexpected line break.
  * Sample: [Testing page](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/CSS%20Questions/block_formatting_contexts.html), [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/CSS%20Questions/block_formatting_contexts.html), [CSS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/CSS%20Questions/css/block_formatting_contexts.css)
  * Ref: [Block formatting contexts](http://www.w3.org/TR/CSS21/visuren.html#block-formatting), [Understanding Block Formatting Contexts in CSS](http://www.sitepoint.com/understanding-block-formatting-contexts-in-css/)
* What are the various clearing techniques and which is appropriate for what context?
  * Ans:
    * An extra element with css `clear: both;` (or left/right) always works well.
    * Use BFC (overflow: hidden; is a good choice) also works well in most scenario.
    * Use pseudo element (`::after`) with `clear: both:` is a good way in modern browsers if you do not need to support IE8-.
* Explain CSS sprites, and how you would implement them on a page or site.
  * Ans:
    * Explain: CSS sprites denotes combine several small image into large one then use CSS to control which part you want to display for an element, the purpose is to reduce the amount of http requests.
    * how you would implement them on a page or site
      * Basically, I'll put all icons together and use CDN to serve it. For some "component-based" icons (e.g., a search icon has different images for normal/hover/pressed states) I probably will make different css sprite for different component depends on actual use case.
      * For page based site (i.e., really change page often) I'll prefer making page/component based sprite, for SPA (Single Page App) I'll make a big sprite for whole site.

* What are your favourite image replacement techniques and which do you use when?
  * Ans:
    * My favourite one is simply use an absolute positioned element that on top of the text to display image.
    * I'll always use this one and never bother others (such like hide text or move text out of screen).
  * Ref: [Nine Techniques for CSS Image Replacement](https://css-tricks.com/css-image-replacement/)
* How would you approach fixing browser-specific styling issues?
  * Ans:
    * I'll try to find a cross-browser solution and evaluate it.
    * If I cannot find (and accept) a cross-browser solution but it is possible that fix it by css hack without affecting other browsers, I'll use css hack.
    * If the specific browser need different styles that will affect other browsers, I'll write another stylesheet that only load for that browser.
* How do you serve your pages for feature-constrained browsers?
  * Ans: Try to find workaround or implement it myself (i.e., polyfill, something like [CSS3 PIE](http://css3pie.com/) or [excanvas](https://github.com/arv/explorercanvas) help this), or implement the feature to fit that browser first then progressive enhancement.
  * What techniques/processes do you use?
    * Ans: 
      * Currently I use ZK which has it's own system to manage Component and resources.
      * Regarding the parent question, the process I use is progressive enhancement, the techniques I use depending on the target issue (talk more example to describe if needed).
* What are the different ways to visually hide content (and make it available only for screen readers)?
  * Ans: Control positio of text by `text-indent: -10000px;` or `position: fixed; left: -10000px;`, or probably use opacity/control its color to make it invisible, need to test which property will also skipped by screen readers actually.
* Have you ever used a grid system, and if so, what do you prefer?
  * Ans: I tried two, ZK's grid component and bootstrap's grid system (I am not sure whether flex box model and table are grid systems, looks similarly). I prefer bootstrap since its pretty straight forward, just need to know little class names/rules and easy to control. It mix style into HTML a bit but its acceptable for me.
  * Update: One day later I found Grid System also related to Graphic Design, bootstrap even better when consider this, it makes a bridge between Designer and Developer.
  * Ref: [Grid (graphic design)](https://en.wikipedia.org/wiki/Grid_%28graphic_design%29)
* Have you used or implemented media queries or mobile specific layouts/CSS?
  * Ans: Yes. (Is this the answer?), usually need to adjust image/text/button size for smaller screen, touch is not as easy as mouse cllick.
* Are you familiar with styling SVG?
  * Ans: No, haven't tried it too much. Just used Morrischart and try to adjust its text size by cotrol some attributes.
* How do you optimize your webpages for print?
  * Ans:
    * For Website Structure
      * Create A Stylesheet For Print: Add the print style sheet, with the media attribute set to "print", you probably want to hide side-bar menu or adjust content size for print.
      * Avoid Unnecessary HTML Tables: Controlling the content area of your website can be extremely challenging when the page structure is trapped in a table.
      * Know Which Portions Of The Page Don't Have Any Print Value: e.g., banners or ads should be removed when print. Create a class called no-print and add that class declaration to DIVS, images, and other elements that have no print value
      ```css
      .no-print   { display:none; }
      ```
      ```html
      <!-- Example -->
      <div id="navigation" class="no-print">
        .... <!-- who needs navigation when you're looking at a printed piece? -->
      </div>
      ```
      * Use Page Breaks: page-break-before / page-break-after properties prove to be useful. Page breaks are much more reliable when used with DIV elements instead of table cells.
      ```css
      .page-break { page-break-before: always; } /* put this class into your main.css file with "display:none;" */
      ```
      ```html
      Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Fusce eu felis. Curabitur sit amet magna. Nullam aliquet. Aliquam ut diam...
      <div class="page-break"></div>
      Lorem ipsum dolor sit amet, consectetuer adipiscing elit....
      ```
      * Size Your Page For Print: Setting the content area width to 600px. This ensures that words wont bleed outside the print area.
      * Test: Note that if you have a website that serves dynamic data, you wont be able to win all the time but you may be able to figure a scheme to format content well most of the time. Be sure to test in multiple browsers
    * For Website Content
      * Adjust Your Heading Tag Properties: Make sure your `<h1>` tag is large enough to make it known that it is the subject of the printed piece. For the `<h2>` tag, using a bottom-border property to establish the tag as the second heading. The `<h3>` will be the same size or slightly larger than the paragraph text. All tags will be in bold. To make things easier, using margin and setting the padding to 0; the two properties control the same function for printing and it will be easier to just use one. Use very little margin below the heading with exception to the `<h1>` tag.
      ```css
      h1,h2,h3  { font-weight:bold; }
      h1    { font-size:24px; }
      h2    { font-size:16px; border-bottom:1px solid #ccc; padding:0 0 2px 0; margin:0 0 5px 0; }
      h3    { font-size:13px; margin:0 0 2px 0; }
      ```
      * Adjust Your Paragraph Tag Properties: There are some properties you can optimize. The line-height, padding, and margin will be the most important properties to address. Setting the bottom margin of the paragraph to a value more than the line-height. Lastly, adjust the font-size to a reasonable ratio to your headers and use a standard font-family if you aren't already.
      ```css
      p   { font-size:11px; font-family:times new roman, serif; margin:0 0 18px 0; }
      ```
      * Format Your Links: simply adding the necessary text-decoration declaration:
      ```css
      a   { text-decoration:underline; }
      ```
      Showing the user a link may not be good enough. What if you'd like them to know the URL of the link? Simply add the following to your print stylesheet (not supported by some browsers such like IE6):
      ```css
      a:link:after, a:visited:after { content: " (" attr(href) ") "; font-size: 90%; }
      ```
      * Adjust Any Custom Classes: Look through your custom CSS classes to make sure none of your other settings will cause your page to look unorganized.
  * Ref: [Optimizing Your Website Structure For Print Using CSS](https://davidwalsh.name/optimizing-structure-print-css), [Optimizing Your Website Content For Print Using CSS](https://davidwalsh.name/optimizing-content-print-css)
* What are some of the "gotchas" for writing efficient CSS?
  * Ans: Its a bit personal, several points
    * Avoid expensive property: Try to replace expensive property by faster one, e.g.,
    replace
    ```css
    /* The slow way */
    .make-it-slow {
      box-shadow: 0 1px 2px rgba(0,0,0,0.15);
      transition: box-shadow 0.3s ease-in-out:
    }

    /* Transition to a bigger shadow on hover */
    .make-it-slow:hover {
      box-shadow: 0 5px 15px rgba(0,0,0,0.3);
    }
    ```
    with
    ```css
    /* The fast way */
    .make-it-fast {
      box-shadow: 0 1px 2px rgba(0,0,0,0.15);
    }

    /* Pre-render the bigger shadow, but hide it */
    .make-it-fast:after {
      box-shadow: 0 5px 15px rgba(0,0,0,0.3);
      opacity: 0;
      transition: opacity 0.3s ease-in-out:
    }
    ```
    * Minimizing browser reflow: 
      * Reduce unnecessary DOM depth.
      * Minimize CSS rules, and remove unused CSS rules.
      * If you make complex rendering changes such as animations, do so out of the flow. Use position-absolute or position-fixed to accomplish this.
      * Avoid unnecessary complex CSS selectors - descendant selectors in particular - which require more CPU power to do selector matching.
    * Use preprocessor (e.g., LESS, SASS) for better structure.
    * I prefer use only class, avoid ID, use some major tag (`body, h1/h2/h3, p, ...` for normalize only)
    * For a page I'll declare selector by block role, `.header .slogan`, `.sidebar .menu`, etc.
    * For some component used everywhere I'll declare "component based" CSS, this helps to improve maintainability and reusability, also reduce the file size since you do not need to write styles for different page, prevent too much descendant selectors and reduce the amount of unused rules. e.g.,
    ```css
    .chosenbox {...}
    .chosenbox-sel-item {...}
    .chosenbox-del-btn {...}
    ```
    * For some utilities I probably will write specific class and apply it anywhere in HTML if needed, this mix style into HTML but acceptable for me. e.g.,
    ```css
    .inline-block {
      display: inline-block;
      /* IE hack */
      *display: inline;
      zoom: `;
    }
    ```
    * Ref: [CSS performance revisited: selectors, bloat and expensive styles](http://benfrain.com/css-performance-revisited-selectors-bloat-expensive-styles/), [Profiling CSS for fun and profit](http://perfectionkills.com/profiling-css-for-fun-and-profit-optimization-notes/), [Minimizing browser reflow](https://developers.google.com/speed/articles/reflow?hl=en), [How to animate "box-shadow" with silky smooth performance](http://tobiasahlin.com/blog/how-to-animate-box-shadow/)
* What are the advantages/disadvantages of using CSS preprocessors?
  * Ans:
    * advantages
      * selector nesting let you writing CSS in more scructure way, instead of repeating the selectors many times you simply nest the rules that should be applied to child elements.
      * extend (in SASS) let you defining a set of styles and reuse it everywhere, and this will be compilied to comma-delimited CSS selectors.
      * mixin let you defining a set of styles and reuse it everywhere, probably including some variable and calculation.
      * variables let you changing values used in lots of area easily.
    * disadvantages
      * selector nesting is very easy to be over specific and specificity is hard to handle and a bad thing for maintainability
      * selector nesting also makes you harder to locate something, e.g., probably a `.text` class under several different block, you will need to check which block you are at after simple text-search. It is even harder since the selectors are mixed with properties.
      * selector nesting guide you to write rules nested, probably cause long chain of selectors that often just as bad.
      * extend probably cause massive selectors in CSS.
      * variables, extend and mixin reduce maintainability. Since the place where you are using the properties is far away from where the properties are being defined there is a bigger chance that you will change properties without noticing you are affecting multiple objects at once, or not realizing which elements are being affected by the changes.
      * In short, if the rules are changed often (and not logically/predictable) then probably better not to use variables, extend and mixin.
    * Ref: [The problem with CSS pre-processors](http://blog.millermedeiros.com/the-problem-with-css-pre-processors/), [@extend performance #12](https://github.com/sass/sass/issues/12), [LESS/SASS: The Advantages of CSS Preprocessing Explained](http://blog.blakesimpson.co.uk/read/37-less-sass-the-advantages-of-css-preprocessing-explained)
  * Describe what you like and dislike about the CSS preprocessors you have used.
    * Ans: I pretty like the way LESS works, you can build it to CSS file directly or build it by JavaScript at client side dynamically, this gives it more flexibility. Nothing to dislike, if you do not like some features then just simply do not use them and write plain CSS.
* How would you implement a web design comp that uses non-standard fonts?
  * Ans:
    * I'll try to find a cloud font service like [Google Fonts](https://www.google.com/fonts) to use first.
    * If there is no cloud font I can use then I'll try to find the appropriate font source file (.ttf, .woff, etc, probably need to convert file manually) and use `@font-face` property in CSS:
    ```css
      @font-face {
        font-family: 'MyWebFont';
        src: url('webfont.eot'); /* IE9 Compat Modes */
        src: url('webfont.eot?#iefix') format('embedded-opentype'), /* IE6-IE8 */
             url('webfont.woff2') format('woff2'), /* Super Modern Browsers */
             url('webfont.woff') format('woff'), /* Pretty Modern Browsers */
             url('webfont.ttf')  format('truetype'), /* Safari, Android, iOS */
             url('webfont.svg#svgFontName') format('svg'); /* Legacy iOS */
      }
    ```
  * Ref: [Using @font-face](https://css-tricks.com/snippets/css/using-font-face/)
* Explain how a browser determines what elements match a CSS selector.
  * Ans: CSS Selectors are matched by browser engines from right to left. So they first find the children and then check their parents to see if they match the rest of the parts of the rule.
  * Ans (longer): When a browser is doing selector matching it has one element (the one it's trying to determine style for) and all your rules and their selectors and it needs to find which rules match the element. If you start by just matching the rightmost part of the selector against your element, then chances are it won't match and you're done. If it does match, you have to do more work, but only proportional to your tree depth, which is not that big in most cases. On the other hand, if you start by matching the leftmost part of the selector... what do you match it against? You have to start walking the DOM, looking for nodes that might match it. Just discovering that there's nothing matching that leftmost part might take a while.
  * Ref: [Why do browsers match CSS selectors from right to left?](http://stackoverflow.com/questions/5797014/why-do-browsers-match-css-selectors-from-right-to-left)
* Describe pseudo-elements and discuss what they are used for. 
  * Ans: A CSS pseudo-element is used to style specified parts of an element. For example, it can be used to:
    * Style the first letter, or line, of an element
    * Insert content before, or after, the content of an element
  * Ref: [CSS Pseudo-elements](http://www.w3schools.com/css/css_pseudo_elements.asp)
* Explain your understanding of the box model and how you would tell the browser in CSS to render your layout in different box models.
  * Ans:
    * All HTML elements can be considered as boxes. In CSS, the term "box model" is used when talking about design and layout. The CSS box model is essentially a box that wraps around every HTML element. It consists of: margins, borders, padding, and the actual content.
    * There is a `box-sizing` property in CSS3 used to tell the browser what the sizing properties (width and height) should include.
      * `content-box`: Default. The width and height properties (and min/max properties) includes only the content. Border, padding, or margin are not included, which means you need to do some calculation to adjust the width/height with border/padding specified.
      * `border-box`: The width and height properties (and min/max properties) includes content, padding and border, but not the margin. So you can specify the width/height without headache of different border or padding.
      * Seems there was a `padding-box` that is removed now.
  * Ref: [CSS Box Model](http://www.w3schools.com/css/css_boxmodel.asp), [CSS3 box-sizing Property](http://www.w3schools.com/cssref/css3_pr_box-sizing.asp), [Box Sizing](https://css-tricks.com/box-sizing/)
* What does ```* { box-sizing: border-box; }``` do? What are its advantages?
  * Ans:
    * It tells the browser that the width and height properties (and min/max properties) includes content, padding and border, but not the margin.
    * It allows you specify border/padding freely without affect actual visible width, this is useful for RWD (Responsive Web Design).
    * Just need to take care of browser compatibility btw.
* List as many values for the display property that you can remember.
  * Ans: (Not cheeting) `none`, `inline`, `inline-block`, `block`, `table`, `table-cell`, `table-caption`, `flex`
* What's the difference between inline and inline-block?
  * Ans:
    * `inline`: Both the inside of this block and the element itself are formatted as an inline-level box.
    * `inline-block`: The inside of this block is formatted as block-level box, and the element itself is formatted as an inline-level box
    * Inline elements:
      * respect left & right margins and padding, but not top & bottom
      * cannot have a width and height set
      * allow other elements to sit to their left and right
    * Inline-block elements
      * allow other elements to sit to their left and right
      * respect all margin and padding
      * respect height and width
  * Ref: [CSS display Property](http://www.w3schools.com/cssref/pr_class_display.asp), [What is the difference between display: inline and display: inline-block?](http://stackoverflow.com/questions/8969381/what-is-the-difference-between-display-inline-and-display-inline-block)
* What's the difference between a relative, fixed, absolute and statically positioned element?
  * Ans:
    * `position: static;`: HTML elements are positioned static by default. Static positioned elements are not affected by the top, bottom, left, and right properties. An element with `position: static;` is not positioned in any special way; it is always positioned according to the normal flow of the page
    * `position: relative;`: An element with `position: relative;` is positioned relative to its normal position. Setting the top, right, bottom, and left properties of a relatively-positioned element will cause it to be adjusted away from its normal position. Other content will not be adjusted to fit into any gap left by the element. Usually the element with `position: relative;` without any special offset specified is used to be the `anchor` of element with `position: absolute;`.
    * `position: fixed;`: An element with `position: fixed;` is positioned relative to the viewport, which means it always stays in the same place even if the page is scrolled. The top, right, bottom, and left properties are used to position the element. A fixed element does not leave a gap in the page where it would normally have been located.
    * `position: absolute;`: An element with `position: absolute;` is positioned relative to the nearest positioned ancestor (instead of positioned relative to the viewport, like fixed). However; if an absolute positioned element has no positioned ancestors, it uses the document body, and moves along with page scrolling.
      * Note: A "positioned" element is one whose position is anything except static.
  * Ref: [CSS Layout - The position Property](http://www.w3schools.com/css/css_positioning.asp)
* The 'C' in CSS stands for Cascading.  How is priority determined in assigning styles (a few examples)?  How can you use this system to your advantage?
  * Ans:
    * How is priority determined in assigning styles
      * The ID selector is stronger than Class.
      * Class selector is stronger than Tag.
      * The rule "further down in the CSS" has higher priority
      * The selector which is "closer to the actual text" has higher priority
      * More specific (by specifing parent selector or chaining multiple selectors) is stronger.
      * Inline style is strongest (except `!important`)
      * `!important` raises priority as a charm.
      * There are also some default styles, e.g. `display: block;` for `div`.
      * Some examples: [Sample](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/CSS%20Questions/css_priority.html), [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/CSS%20Questions/css_priority.html), [CSS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/CSS%20Questions/css/css_priority.css)
    * How can you use this system to your advantage?
      * Do reset/normalize with the weaker selector (tags) at the top, this makes them easier to be overridden.
      * Create util class (e.g. `.clear {clear: both !important;}`) stronger at the bottom so it will always works as expected.
      * Declare multiple-status part normally - not too strong/weak (e.g., normal/hover/pressed/focused of a button) so can switch between them easily.
      * Well, I cannot answer this part well.
  * Ref: [Specifics on CSS Specificity](https://css-tricks.com/specifics-on-css-specificity/)
* What existing CSS frameworks have you used locally, or in production? How would you change/improve them?
  * Ans:
    * I just tried Bootstrap
    * There are something I want to change
      * The menu works with mouse click be default, I would change it to work with mouseover.
      * The cols is floated, this will cause some weird layout if different column has different height. I decided to change them to `display: inline-block` so I can use them easier (I guess).
    * I have read [5 reasons NOT to use Twitter Bootstrap](http://www.zingdesign.com/5-reasons-not-to-use-twitter-bootstrap/) so probably will try some other Framework (e.g., [Foundation](http://foundation.zurb.com/) )
* Have you played around with the new CSS Flexbox or Grid specs?
  * Ans: Yes. I have tried to make [a component](https://github.com/benbai123/flexlayout) for [ZK](https://www.zkoss.org/) Framework with Flexbox, but it is outdated, I made it at 2012 and the Flexbox spec has changed after that.
* How is responsive design different from adaptive design?
  * Ans: responsive design using a set of styles (probably with some media query) to fit multiple screen size, adaptive design make several sets of styles and apply it to different screen size based on breakpoint.
  * Ref: [Adaptive web design (wiki)](https://en.wikipedia.org/wiki/Adaptive_web_design), [What is the difference between responsive vs. adaptive web design?](http://www.techrepublic.com/blog/web-designer/what-is-the-difference-between-responsive-vs-adaptive-web-design/), [Responsive vs. Adaptive Design: What’s the Best Choice for Designers?](https://studio.uxpin.com/blog/responsive-vs-adaptive-design-whats-best-choice-designers/)
* Have you ever worked with retina graphics? If so, when and what techniques did you use?
  * Ans:
    * No I haven't, but I know its related to high resolution.
    * I'll try 2 ways, use images with higher dimantion for retina, or use SVG to scale images.
  * Ref: [I cheated before answer this (and lots of others, actually) question :p](http://www.pro-tekconsulting.com/blog/have-you-ever-worked-with-retina-graphics-if-so-when-and-what-techniques-did-you-use/)
* Is there any reason you'd want to use `translate()` instead of *absolute positioning*, or vice-versa? And why?
  * Ans:
    * Is there any reason you'd want to use `translate()` instead of *absolute positioning*
      * translateX/Y have better performance
      * The property works well with the element itself, do not need another parent with `position: relative`
    * vice-versa
      * translateX/Y affected by padding, so prbably need some calculation (by yourself) to get real position.
      * *absolute positioning* support older browser
    * Simple sample: [Demo](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/CSS%20Questions/absolute_and_translate.html), [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/CSS%20Questions/absolute_and_translate.html), [CSS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/CSS%20Questions/css/absolute_and_translate.css)
  * Ref: [Why Moving Elements With Translate() Is Better Than Pos:abs Top/left](http://www.paulirish.com/2012/why-moving-elements-with-translate-is-better-than-posabs-topleft/), [CSS transform vs position](http://stackoverflow.com/questions/7108941/css-transform-vs-position)
