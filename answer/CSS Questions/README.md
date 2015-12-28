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
* Explain CSS sprites, and how you would implement them on a page or site.
* What are your favourite image replacement techniques and which do you use when?
* How would you approach fixing browser-specific styling issues?
* How do you serve your pages for feature-constrained browsers?
  * What techniques/processes do you use?
* What are the different ways to visually hide content (and make it available only for screen readers)?
* Have you ever used a grid system, and if so, what do you prefer?
* Have you used or implemented media queries or mobile specific layouts/CSS?
* Are you familiar with styling SVG?
* How do you optimize your webpages for print?
* What are some of the "gotchas" for writing efficient CSS?
* What are the advantages/disadvantages of using CSS preprocessors?
  * Describe what you like and dislike about the CSS preprocessors you have used.
* How would you implement a web design comp that uses non-standard fonts?
* Explain how a browser determines what elements match a CSS selector.
* Describe pseudo-elements and discuss what they are used for. 
* Explain your understanding of the box model and how you would tell the browser in CSS to render your layout in different box models.
* What does ```* { box-sizing: border-box; }``` do? What are its advantages?
* List as many values for the display property that you can remember.
* What's the difference between inline and inline-block?
* What's the difference between a relative, fixed, absolute and statically positioned element?
* The 'C' in CSS stands for Cascading.  How is priority determined in assigning styles (a few examples)?  How can you use this system to your advantage?
* What existing CSS frameworks have you used locally, or in production? How would you change/improve them?
* Have you played around with the new CSS Flexbox or Grid specs?
* How is responsive design different from adaptive design?
* Have you ever worked with retina graphics? If so, when and what techniques did you use?
* Is there any reason you'd want to use `translate()` instead of *absolute positioning*, or vice-versa? And why?
