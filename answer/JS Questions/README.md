#### JS Questions:

* Explain event delegation
  * Ans (Short): Bind event handler to a single parent to listen and handle event from children (through event bubbling).
  * Ans (Long):
      * DOM event delegation is a mechanism of responding to ui-events via a single common parent rather than each child, through the magic of event "bubbling" (aka event propagation). 
      * With event delegation the number of event bindings can be drastically decreased by moving them to a common parent element, and code that dynamically creates new elements on the fly can be decoupled from the logic of binding their event handlers.
      * Another benefit to event delegation is that the total memory footprint used by event listeners goes down (since the number of event bindings go down). It may not make much of a difference to small pages that unload often (i.e. user's navigate to different pages often). But for long-lived applications it can be significant.
  * Ref: [What is DOM Event delegation?](http://stackoverflow.com/questions/1687296/what-is-dom-event-delegation), the answer with highest vote up really good and should be accepted, btw.
* Explain how `this` works in JavaScript
  * Ans (Real): Well I know how to use it but Explain...
  * Ans (Cheated):
      * The ECMAScript Standard defines this as a keyword that "evaluates to the value of the ThisBinding of the current execution context"
      * When evaluating code in the initial global execution context (global scope), ThisBinding is set to the global object (if in browser, window)
      * If a function is called on an object, such as in `obj.myMethod()` or the equivalent `obj["myMethod"]()`, then ThisBinding is set to the object `obj`.
      * by a direct call to eval(), ThisBinding is left unchanged; it is the same value as the ThisBinding of the calling execution context
      * if not by a direct call to eval(), ThisBinding is set to the global object as if executing in the initial global execution context 
      * If call a function directly e.g. `myFun()`, then ThisBinding is set to global object.
      * There are some functions that allow you to specify ThisBinding
      ```
      Function.prototype.apply( thisArg, argArray )
      Function.prototype.call( thisArg [ , arg1 [ , arg2, ... ] ] )
      Function.prototype.bind( thisArg [ , arg1 [ , arg2, ... ] ] )
      Array.prototype.every( callbackfn [ , thisArg ] )
      Array.prototype.some( callbackfn [ , thisArg ] )
      Array.prototype.forEach( callbackfn [ , thisArg ] )
      Array.prototype.map( callbackfn [ , thisArg ] )
      Array.prototype.filter( callbackfn [ , thisArg ] )
      ```
      * When a function is called in a constructor context, the value of this is the new object that the interpreter created
  * Sample: [Demo](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/JS%20Questions/the_this_keyword.html), [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/the_this_keyword.html), [JS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/js/the_this_keyword.js)
  * Ref: [How does the “this” keyword work?](http://stackoverflow.com/questions/3127429/how-does-the-this-keyword-work), [Scope in JavaScript](http://web.archive.org/web/20110725013125/http://www.digital-web.com/articles/scope_in_javascript/)
* Explain how prototypal inheritance works
  * Ans:
    * Explain what prototype first:
      * JavaScript only has one construct: objects
      * Each object has an internal link to another object called its prototype
      * That prototype object (of another object) has a prototype of its own, and so on until an object is reached with null as its prototype. null, by definition, has no prototype, and acts as the final link in this prototype chain.
    * how prototypal inheritance works
      * JavaScript objects are dynamic "bags" of properties (referred to as own properties). JavaScript objects have a link to a prototype object.
      * When trying to access a property of an object, the property will not only be sought on the object but on the prototype of the object, the prototype of the prototype, and so on until either a property with a matching name is found or the end of the prototype chain is reached.
  * Ref: [Inheritance and the prototype chain](https://developer.mozilla.org/en/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)
* What do you think of AMD vs CommonJS?
  * Ans (Real): Actually I do not know both of them (:->).
  * Ans (Cheating): 
    * The difference:
      * AMD takes a browser-first approach, CommonJS on the other hand takes a server-first approach.
      * AMD using asynchronous behavior, CommonJS assuming synchronous behavior.
    * In my opinion the asynchronous behavior in AMD will cause more round trip, make longer loading/rendering time and also harder to read. On the other hand, CJS block browser more and seems less flexibility (for dynamic/lazy/defer loading).
  * Ref: [AMD vs Common JS & UMD](https://www.linkedin.com/pulse/amd-vs-common-js-umd-damodaran-sathyakumar).
* Explain why the following doesn't work as an IIFE: `function foo(){ }();`.
  * Ans (short): The `function foo(){ }` is not wrapped by `()`
  * Ans (long): When the parser encounters the function keyword it treats it as a function declaration (statement) by default, so it will parse it as
  ```javascript
  // the function declaration
  function foo(){ }
  // ??? what's this?
  ();
  ```
  Because JS is executed under two phase, compilation and execution, function declaration (`function foo(){ }` here) will be parsed and execued in compilation phase, then only `();` executed in execution phase which causes the SyntaxError.
  * What needs to be changed to properly make it an IIFE?
    * Ans: So what we need to do is turn it into function expression by changing `function foo` to `var foo = function` or wrapping `function foo () {}` in parens.
      * change to function expression
      ```javascript
      var foo = function(){
        // code to run
      }();
      ```
      * wrap with parens
      ```javascript
      (function foo() {
        // code to run
      })();
      ```
      * you can even remove its name
      ```javascript
      (function () {
        // code to run
      })();
      ```
  * Sample: [Demo](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/JS%20Questions/IIFE_test.html), [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/IIFE_test.html), [JS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/js/IIFE_test.js)
  * Ref: [Immediately-Invoked Function Expression (IIFE)](http://benalman.com/news/2010/11/immediately-invoked-function-expression/), [JavaScript function declaration and evaluation order](http://stackoverflow.com/questions/3887408/javascript-function-declaration-and-evaluation-order)
* What's the difference between a variable that is: `null`, `undefined` or undeclared?
  * Ans: 
    * `undeclared` variables don’t even exist
    * `undefined` variables exist, but don’t have anything assigned to them
    * `null` variables exist and have null assigned to them
  * How would you go about checking for any of these states?
    * Ans: As the code below, it can detect the status of a specific variable
    ```javascript
    function testStatus () {
        try { // here it is undeclared so will throw an exception
            if (val) {};
        } catch (e) { // just return the exception or some msg
            return e;
        }
        if ((typeof val) === 'undefined') // here it is undefined
            return 'val is undefined';
        if (val == null) // it is null
            return 'val is null';
    }
    ```
  * Sample: [Demo](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/JS%20Questions/null_undefine_undeclare.html), [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/null_undefine_undeclare.html), [JS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/js/null_undefine_undeclare.js)
  * Ref: [JS: null, undefined, and undeclared](http://lucybain.com/blog/2014/null-undefined-undeclared/), [What is the difference in Javascript between 'undefined' and 'not defined'?](http://stackoverflow.com/questions/833661/what-is-the-difference-in-javascript-between-undefined-and-not-defined)
* What is a closure, and how/why would you use one?
  * Ans:
    * Closures are functions that refer to independent (free) variables. In other words, the function defined in the closure 'remembers' the environment in which it was created.
    * How/Why use it: For me the reason is I want something private, make global scope clean, and prevent any unexpected interaction/side-effect. e.g., assume making a game with JavaScript and you want keep the whole environment of the game private and safe.
  * Sample: [Demo](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/JS%20Questions/closure.html), [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/closure.html), [JS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/js/closure.js)
  * Ref: [Closures - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)
* What's a typical use case for anonymous functions?
  * Ans: The inline function, e.g., jQuery Event Handler
  ```javascript
  $('.selector').on('click', function (e) {
    // this is anonymous functions
  });
  ```
  or IIFE
  ```javascript
  (function () {
    //  IIFE with anonymous functions
  })();
  ```
* How do you organize your code? (module pattern, classical inheritance?)
  * Ans:
    * I use object literal to modulize/componentlize my code and handle prototype chain to simulate classical inheritance if needed.
    * I will write some APIs to wrap the common process of define/extend a `class` create instance and call super method, after that I can work with almost pure object literal.
  * Sample: [Demo](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/JS%20Questions/code_organization.html), [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/code_organization.html), [JS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/js/code_organization.js)
  * Note:
    * The sample above just simple demo case, in real use you may separate js code to several different files and wrap them in different IIFE so they will have different scope.
    * This probably a little old fashion, today probably use some libraries (e.g., AMD/CommonJS) to help you organize your code.
  * Ref: The way I organize my code is learned from [ZK](https://www.zkoss.org/), See [zk.js](https://github.com/zkoss/zk/blob/v7.0.3/zk/src/archive/web/js/zk/zk.js#L211)
* What's the difference between host objects and native objects?
  * Ans:
    * host objects: object supplied by the host environment to complete the execution environment of ECMAScript. e.g., (assuming browser environment): `window`, `document`, `location`, `history`, `XMLHttpRequest`, `setTimeout`, ...
    * native objects: object in an ECMAScript implementation whose semantics are fully defined by this specification rather than by the host environment. e.g., `Object` (constructor), `Date`, `Math`, `parseInt`, `eval`, string methods like `indexOf` and `replace`, ...
  * Ref: [What is the difference between native objects and host objects?](http://stackoverflow.com/questions/7614317/what-is-the-difference-between-native-objects-and-host-objects)
* Difference between: `function Person(){}`, `var person = Person()`, and `var person = new Person()`?
  * Ans:
    * `function Person(){}` declare a function named Person.
    * `var person = Person()` declare a variable named person, call Person function, and assign the value returned by Person function to variable person.
    * `var person = new Person()` declare a variable named person, create an object with the constructor function Person, and assign the created object to person variable.
    * (Doubt) is this the answer? Not sure what the question asking.
* What's the difference between `.call` and `.apply`?
  * Ans (honest): I know they are different but not quite clear...
  * Ans (cheating):
    * `.apply` lets you invoke the function with arguments as an array; `.call` requires the parameters be listed explicitly.
    * (when the args are known) `.call` has better performance than `.apply`, [call vs apply - jsperf](http://jsperf.com/test-call-vs-apply/3)
  * Ref: [What is the difference between call and apply?](http://stackoverflow.com/questions/1986896/what-is-the-difference-between-call-and-apply)
* Explain `Function.prototype.bind`.
  * Ans: The bind() method creates a new function that, when called, has its this keyword set to the provided value, with a given sequence of arguments preceding any provided when the new function is called.
  * Sample: [Demo](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/JS%20Questions/function_bind.html), [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/function_bind.html), [JS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/js/function_bind.js)
  * Ref: [Function.prototype.bind() - JavaScript | MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind)
* When would you use `document.write()`?
  * Ans: I didn't use it before, but I learned some legitimate use of it when answer this question
    * for polyfill or fallback e.g.,
    ```javascript
    <!-- Grab Google CDN's jQuery, with a protocol relative URL; fall back to local if offline -->
    <script src="//ajax.googleapis.com/ajax/libs/jquery/1.6.3/jquery.min.js"></script>
    <script>window.jQuery || document.write('<script src="js/libs/jquery-1.6.3.min.js"><\/script>')</script>
    ```
    * for some 3rd party lib it's actually the easiest way for them to distribute such snippets
      * It keeps the scripts small
      * They don't have to worry about overriding already established onload events or including the necessary abstraction to add onload events safely
      * It's extremely compatible
  * Ref: [JS: When would you use document.write()?](http://lucybain.com/blog/2015/js-document-write/), [Why is document.write considered a “bad practice”?](http://stackoverflow.com/questions/802854/why-is-document-write-considered-a-bad-practice)
* What's the difference between feature detection, feature inference, and using the UA string?
  * Ans:
    * Feature detection checks a feature for existence, e.g.:
    ```javascript
    if (window.XMLHttpRequest) {
      new XMLHttpRequest();
    }
    ```
    * Feature inference checks for a feature just like feature detection, but uses another function because it assumes it will also exist, e.g.:
    ```javascript
    if (document.getElementsByTagName) {
      element = document.getElementById(id);
    }
    ```
    * Checking the UA string is an old practice and should not be used anymore. You keep changing the UA checks and never benefit from newly implemented features, e.g.:
    ```javascript
    if (navigator.userAgent.indexOf("MSIE 7") > -1){
      //do something
    }
    ```
* Explain AJAX in as much detail as possible.
  * Ans (honestly): A way to send/retrieve data to/from server asynchronously (in the background) without interfering with the display and behavior of the existing page so you can exchanging data asynchronously between browser and server to avoid full page reloads
  * Ans (uh...oh...):
    * Ajax (short for asynchronous JavaScript and XML) is a set of web development techniques utilizing many web technologies used on the client-side to create asynchronous Web applications. With Ajax, web applications can send data to and retrieve from a server asynchronously (in the background) without interfering with the display and behavior of the existing page. By decoupling the data interchange layer from the presentation layer, Ajax allows for web pages, and by extension web applications, to change content dynamically without the need to reload the entire page. Data can be retrieved using the XMLHttpRequest object. Despite the name, the use of XML is not required (JSON is often used in the AJAJ variant), and the requests do not need to be asynchronous.
    * Ajax is not a technology, but a group of technologies. HTML and CSS can be used in combination to mark up and style information. The DOM is accessed with JavaScript to dynamically display – and allow the user to interact with – the information presented. JavaScript and the XMLHttpRequest object provide a method for exchanging data asynchronously between browser and server to avoid full page reloads.
  * Ref: [AJAX - wiki](https://en.wikipedia.org/wiki/Ajax_%28programming%29)
* Explain how JSONP works (and how it's not really AJAX).
  * Ans:
    * how JSONP works
      * Client use `<script>` tag to request a JS File, assume your request is ```http://www.example.net/sample.aspx?callback=mycallback```, you tell server the callback functio is mycallback with the callback parameter.
      * Server (probably dynamic generate and) response a JS File, assume the response like
      ```javascript
      // server wrap the data with mycallback as you specified
      mycallback({ foo: 'bar' });
      ```
      * when the script is loaded, it'll be evaluated, and your function will be executed, no cross-domain issue (AJAX usually cannot make cross-domain request)
    * how it's not really AJAX
      * AJAX has cross-domain issue except use Cross-origin resource sharing, JSONP is really a simple trick to overcome the XMLHttpRequest same domain policy.
      * Ajax calls can be synchronous (blocking until they complete) or asynchronous, JSONP can only be asynchronous.
  * Ref: [What is JSONP all about?](http://stackoverflow.com/questions/2067472/what-is-jsonp-all-about), [I don't get how JSONP is ANY different from AJAX](http://stackoverflow.com/questions/10289789/i-dont-get-how-jsonp-is-any-different-from-ajax)
* Have you ever used JavaScript templating?
  * If so, what libraries have you used?
  * Ans: Yes, Angularjs
* Explain "hoisting".
  * Ans:
    * In JavaScript, functions and variables are hoisted. Hoisting is JavaScript's behavior of moving declarations to the top of a scope (the global scope or the current function scope).
    * That means that you are able to use a function or a variable before it has been declared, or in other words: a function or variable can be declared after it has been used already.
  * Examples:
    * Variables
    ```javascript
    // the real code
    foo = 2
    var foo;

    // is implicitly understood as:

    var foo;
    foo = 2;
    ```
    * Functions
    ```javascript
    hoisted(); // logs "foo"

    function hoisted() {
      console.log("foo");
    }
    ```
  * Ref: [Hoisting - MDN](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting)
  * See also: [JavaScript function declaration and evaluation order](http://stackoverflow.com/questions/3887408/javascript-function-declaration-and-evaluation-order)
* Describe event bubbling.
  * Ans: It is a way of event propagation in the HTML DOM. Event propagation is a way of defining the element order when an event occurs. If you have a `<p>` element inside a `<div>` element, and the user clicks on the `<p>` element, In bubbling the inner most element's event is handled first and then the outer: the `<p>` element's click event is handled first, then the `<div>` element's click event.
  * Ref: [JavaScript HTML DOM EventListener](http://www.w3schools.com/js/js_htmldom_eventlistener.asp)
* What's the difference between an "attribute" and a "property"?
  * Ans: 
    * When writing HTML source code, you can define attributes on your HTML elements. Then, once the browser parses your code, a corresponding DOM node will be created. This node is an object, and therefore it has properties.
    * Some HTML attributes have 1:1 mapping onto properties. id is one example of such.
    * Some do not (e.g. the value attribute specifies the initial value of an input, but the value property specifies the current value).
  * Ref: [HTML - attributes vs properties [duplicate]](http://stackoverflow.com/questions/19246714/html-attributes-vs-properties), [Properties and Attributes in HTML](http://stackoverflow.com/questions/6003819/properties-and-attributes-in-html)
* Why is extending built-in JavaScript objects not a good idea?
  * Ans:
    * When you extend an object, you change its behaviour. Changing the behaviour of an object that will only be used by your own code is fine. But when you change the behaviour of something that is also used by other code there is a risk you will break that other code.
    * Some cases:
      * Assume you added some API that pretty makes sense (e.g., Array.duplicate), but later, a native object is changed to include a "duplicate" function with different semantics to your own
      * Assume you did something that pretty makes sense, but later, one of 3rd party lib did same thing with different semantics and you upgraded it.
  * Ref: [Why is extending native objects a bad practice?](http://stackoverflow.com/questions/14034180/why-is-extending-native-objects-a-bad-practice)
* Difference between document load event and document ready event?
  * Ans:
    * `$(document).ready()`: Executes when HTML-Document is loaded and DOM is ready, In most cases, the script can be run as soon as the DOM hierarchy has been fully constructed.
    * `window.onload = funcRef;`: Occurs when window has been loaded. In cases where code relies on loaded assets (for example, if the dimensions of an image are required), the code should be placed in a handler for the load event instead.
  * Ref:
    * [jQuery - What are differences between $(document).ready and $(window).load?](http://stackoverflow.com/questions/8396407/jquery-what-are-differences-between-document-ready-and-window-load)
    * [.ready() | jQuery API Documentation](https://api.jquery.com/ready/)
    * [onload Event | w3school](http://www.w3schools.com/jsref/event_onload.asp)
    * [GlobalEventHandlers.onload](https://developer.mozilla.org/en/docs/Web/API/GlobalEventHandlers/onload)
* What is the difference between `==` and `===`?
  * Ans: 
    * `==`: Values are considered equal if they are identical strings, numerically equivalent numbers, the same object, identical Boolean values, or (if different types) they can be coerced into one of these situations
    * `===`: The operators behave identically to the equality operators except no type conversion is done, and the types must be the same to be considered equal.
    * some examples: 
      ```javascript
      1 == '1'; // true
      1 == true; // true
      1 === '1'; // false
      1 === true; // false
      new Date() == new Date().toString(); // true
      new Date() === new Date().toString(); // false
      ```
  * Ref:
    * [Does it matter which equals operator (== vs ===) I use in JavaScript comparisons?](http://stackoverflow.com/questions/359494/does-it-matter-which-equals-operator-vs-i-use-in-javascript-comparisons)
    * [JavaScript tutorial: Comparison operators](http://www.c-point.com/javascript_tutorial/jsgrpComparison.htm)
* Explain the same-origin policy with regards to JavaScript.
  * Ans:
    * The same-origin policy restricts how a document or script loaded from one origin can interact with a resource from another origin. It is a critical security mechanism for isolating potentially malicious documents.
    * Two pages have the same origin if the protocol, port (if one is specified), and host are the same for both pages.
  * Ref: [Same-origin policy - Web security | MDN](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)
* Make this work:
```javascript
duplicate([1,2,3,4,5]); // [1,2,3,4,5,1,2,3,4,5]
```
  * Ans:
  ```javascript
  function duplicate (a) {
    return a.slice().concat(a);
  }
  duplicate([1,2,3,4,5]); // [1,2,3,4,5,1,2,3,4,5]
  ```
  * Ref:
    * [Javascript fastest way to duplicate an Array - slice vs for loop](http://stackoverflow.com/questions/3978492/javascript-fastest-way-to-duplicate-an-array-slice-vs-for-loop)
* Why is it called a Ternary expression, what does the word "Ternary" indicate?
  * Ans:
    * Because it takes three elements, `a? b : c`, where a is the condition, b/c are the returned value when a is true/false
    * Ternary denotes the number of elements (3) it takes
  * Ref:
    * [Ternary operation](https://en.wikipedia.org/wiki/Ternary_operation)
    * [Arity](https://en.wikipedia.org/wiki/Arity)
* What is `"use strict";`? what are the advantages and disadvantages to using it?
  * Ans:
    * What is `"use strict";`?
      * ECMAScript 5's strict mode is a way to opt in to a restricted variant of JavaScript. Strict mode isn't just a subset: it intentionally has different semantics from normal code. Browsers not supporting strict mode will run strict mode code with different behavior from browsers that do, so don't rely on strict mode without feature-testing for support for the relevant aspects of strict mode. Strict mode code and non-strict mode code can coexist, so scripts can opt into strict mode incrementally.
      * Strict mode makes several changes to normal JavaScript semantics. First, strict mode eliminates some JavaScript silent errors by changing them to throw errors. Second, strict mode fixes mistakes that make it difficult for JavaScript engines to perform optimizations: strict mode code can sometimes be made to run faster than identical code that's not strict mode. Third, strict mode prohibits some syntax likely to be defined in future versions of ECMAScript.
    * what are the advantages and disadvantages to using it?
      * advantages
        * It catches some common coding bloopers, throwing exceptions.
        * It prevents, or throws errors, when relatively "unsafe" actions are taken (such as gaining access to the global object).
        * It disables features that are confusing or poorly thought out.
      * disadvantages
        * Strict mode changes semantics, make sure to test your code in browsers that do and don't support strict mode.
          * i.e., probably need to test more browsers
        * If you add `"use strict";` globally probably will break some old code
  * Ref:
    * [Strict mode - JavaScript | MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Strict_mode)
    * [What does “use strict” do in JavaScript, and what is the reasoning behind it?](http://stackoverflow.com/questions/1335851/what-does-use-strict-do-in-javascript-and-what-is-the-reasoning-behind-it)
    * [JavaScript: Why the hatred for strict mode?](http://www.2ality.com/2011/10/strict-mode-hatred.html)
* Create a for loop that iterates up to `100` while outputting **"fizz"** at multiples of `3`, **"buzz"** at multiples of `5` and **"fizzbuzz"** at multiples of `3` and `5`
  * Ans:
    * Something in the test
      * Use array.push to concat string for (probably) lots of small string
      * Try to find a way (lcm here) to reduce loop times
      * Manipulate dom at once rather than update dom several (probably lots of) times
      * Declare variables with var keyward, and outside the for loop
    * Code:
    ```javascript
    var idx = 3, // start from 3
      str,
      arr = [],
      tmp,
      lcm;
    for ( ; idx <= 100; idx++) {
        str = '';
        if (idx%3 == 0) str+='fizz';
        if (idx%5 == 0) str+='buzz';
        if (str) arr.push(str);
        // found the Least common multiple
        if (str == 'fizzbuzz') {
            // copy the lcm and current array
            lcm = idx;
            tmp = arr.slice();
            // step forward lcm
            // loop until limitation
            while ((idx + lcm) < 100) {
                idx += lcm;
                // append the tmp to arr
                arr = arr.concat(tmp);
            }
        }
    }
    output(arr.join(', '));
    function output (str) {
        $('.output')[0].innerHTML += str;
    }
    ```
* Why is it, in general, a good idea to leave the global scope of a website as-is and never touch it?
  * Ans:
    * Every JavaScript file included in the page runs in the same scope. If you have global variables or functions in your code, scripts included after yours that contain the same variable and function names will overwrite your variables/functions.
    * Global variables have slow performance, some reasons:
      * Browser need to walkthrough scope chain to search it.
      * Generally global scope contains much more stuffs than local function scope, need more time to find a variable from it.
      * Probably more reasons, but just remember the conclution: Global is Slow.
  * Ref: 
    * [JavaScript best practices | w3](https://www.w3.org/wiki/JavaScript_best_practices)
    * [Global Variables vs Local Variables | jsPerf](https://jsperf.com/global/4)
    * [30 Tips To Improve Javascript Performance](https://www.monitis.com/blog/2011/05/15/30-tips-to-improve-javascript-performance/)
    * [JavaScript variable performance](https://www.nczonline.net/blog/2009/02/10/javascript-variable-performance/)
* Why would you use something like the `load` event? Does this event have disadvantages? Do you know any alternatives, and why would you use those?
  * Ans:
    * Why would you use something like the `load` event?
      * When I want to do something that really need some other things to be loaded, e.g.,
      * Assume I need to adjust some size based on 'real size' of a block, I need to wait until all contents including image/css are loaded (probably also need to wait some other js are executed)
      * Assume I wrote a piece of JS rely on some others, then I need to run my JS after the others are loaded. (e.g., load some 3rd party API then use it)
    * Does this event have disadvantages?
      * Yes, there are some cases that will cause slow speed, bad structure or broken result.
      * For `window.onload`, it will make things slow since need to wait *Everything* loaded.
      * For some independent content (say dynamically insert an image tag and a script, and in the script there is a foo for image.onload), cannot tell which is loaded first, foo probably will not be called.
        * Note: dynamically added script will not be loaded in sync way.
      * For some 3rd party lib, this simply not work since they need to load more things after first script loaded (so you need to wait more)
      * Assume several scripts has looooong dependency chain and listen to onload event one after another, also pretty slow.
      * You need to really get Dom to bind load event, make your code less flexibility
    * Do you know any alternatives, and why would you use those?
      * Yes, there are some alternatives:
      * Make your own event to arrange your code properly
      ```javascript
      // in the dependency JS file
      // trigger event to notify the
      // scripts depends on this one
      window.myNameSpace.loaded['hey-I-am-loaded'] = true;
      $(document).trigger('hey-I-am-loaded');

      // in the JS file that depends on dependency
      var executed; // flag to keep whether executed
      // run when receive event
      $(document).on('hey-I-am-loaded', run);
      function run () {
        if (!executed && window.myNameSpace.loaded['hey-I-am-loaded']) {
          executed = true;
          // do something
          // ...
          // and trigger another event to notify the
          // scripts depends on this one
          window.myNameSpace.loaded['hey-I-am-also-loaded'] = true;
          $(document).trigger('hey-I-am-also-loaded');
        }
      }
      // also run itself if missing the event since dependency loaded first
      run();
      ```
      * Or generate content by script instead of make them separate
      ```html
      <div class="the-img-area"></div>
      ```
      ```javascript
      var img = new Image();
      // so you will never miss the load event
      img.onload = foo;
      img.src = src;
      $('.the-img-area').append(img);
      ```
      * For some 3rd lib that load more things after it is loaded, what you can do is using a timer to check
      ```javascript
      var cnt = 0,
        timer = setInterval(function () {
        if (cnt > 10) {
          // not loaded after 10 seconds
          outputErrorMessage();
          clearInterval(timer);
        } else if (window.theLibThatDoesntProvideEventOrCallback
          && window.someMoreThingsLoadedByThatLib) {
          foo();
          clearInterval(timer);
        }
        cnt++;
        // check every 100 ms
      }, 1000);
      ```
* Explain what a single page app is and how to make one SEO-friendly.
  * Ans:
    * Explain what a single page app is: A single-page application (SPA) is a web application or web site that fits on a single web page with the goal of providing a more fluent user experience similar to a desktop application. In a SPA, either all necessary code – HTML, JavaScript, and CSS – is retrieved with a single page load, or the appropriate resources are dynamically loaded and added to the page as necessary, usually in response to user actions. The page does not reload at any point in the process, nor does control transfer to another page, although the location hash can be used to provide the perception and navigability of separate logical pages in the application, as can the HTML5 pushState() API . Interaction with the single page application often involves dynamic communication with the web server behind the scenes.
    * how to make one SEO-friendly
      * You can always output some basic HTML with respect to app status (heading, list, urls)
      * You can try to detect "robot" and "human with real browser" and provide robot friendly content to robot.
      * Or use a headless browser that outputs an HTML snapshot on your web server
  * Ref: 
    * [Single-page application](https://en.wikipedia.org/wiki/Single-page_application)
    * [HOW TO OPTIMIZE SINGLE PAGE SITES FOR SEARCH ENGINES](http://www.webdesignerdepot.com/2013/10/how-to-optimize-single-page-sites-for-search-engines/)
    * [A proposal for making AJAX crawlable](https://googlewebmastercentral.blogspot.tw/2009/10/proposal-for-making-ajax-crawlable.html)
* What is the extent of your experience with Promises and/or their polyfills?
  * Ans:
    * I used it in nodeJS (well, not Front-End, and probably not native one) to do some task with MongoDB, chaining tasks or something like that. It makes the code more readable (compared with nested callback) and the order of code sequence is less important (compared with event)
    * No, I didn't use other polyfills
    * BTW, some good articles regarding Promise: [JavaScript Promises](http://www.html5rocks.com/en/tutorials/es6/promises/?redirect_from_locale=tw), [JavaScript Promises With Node.js](http://zpalexander.com/blog/javascript-promises-node-js/)
* What are the pros and cons of using Promises instead of callbacks?
  * Ans:
    * Pros:
      * Avoid [callback hell](http://stackoverflow.com/questions/25098066/what-is-callback-hell-and-how-and-why-rx-solves-it)
    * Cons:
      * Currently there are lots of different versions of Promise. (Native, and [others in this list](http://complexitymaze.com/2014/03/03/javascript-promises-a-comparison-of-libraries/))
      * In some version of Promise, it is much more harder to debug.
      * It increases the code complexity.
  * Ref:
    * [How to debug javascript promises?](http://stackoverflow.com/questions/25827234/how-to-debug-javascript-promises)
    * [Promises/A+ Considered Harmful](http://robotlolita.me/2013/06/28/promises-considered-harmful.html)
    * [Promises vs Callbacks – Code comparison](http://lkrnac.net/blog/2014/10/promises-vs-callbacks-comparison/)
* What are some of the advantages/disadvantages of writing JavaScript code in a language that compiles to JavaScript?
  * Ans (Assume CoffeeScript, basically from Ref):
    * advantages
      * Encourages the use of good JavaScript patterns
      * Discourages JavaScript anti-patterns
      * Makes even good JavaScript code shorter and more readable
    * disadvantages
      * Compilation can be a pain. (Agree!)
      * Relatedly, debugging can be a pain.
      * It's prone to change.
      * It's not as well-known.
    * disadvantages (some more points IMHO)
      * Some problem probably caused by the advantages, e.g.,
        * variables are auto-scoped -> A guy wrote CoffeeScript only probably has less knowledge of scope.
        * Makes even good JavaScript code shorter and more readable -> but probably let you forget real syntax and you will have pain when switch to native JavaScript project.
      * The indent matters. Compilation can be much more pain with this.
      * Line break matters. Sometimes this will force you to write code in weird structure.
      * The 2 points above make write inline object literal a pain until you familiar with them.
  * Ref:
    [What are the pros and cons of Coffeescript?](http://programmers.stackexchange.com/questions/72569/what-are-the-pros-and-cons-of-coffeescript)
* What tools and techniques do you use debugging JavaScript code?
  * Ans:
    * I use Chrome Developer tools
    * I will use break point, `console.log console.trace console.table console.error` methods, `debugger` keyword
    * There is a great article list some tips [Useful Javascript debugging tips you might not know](https://raygun.io/blog/2015/06/useful-javascript-debugging-tips-you-didnt-know/)
* What language constructions do you use for iterating over object properties and array items?
  * Ans
    * for iterating over object properties I will use
    ```javascript
    var obj = {'k1' : 'v1', 'k2' : 'v2'}, // test obj
      key; // key for loop through obj
    for (key in obj) {
      console.log(key); // property key
      console.log(obj[key]); // property value
    }
    ```
    * for iterating over array items I will use
    ```javascript
    var arr = ['item 1', 'item 2', 'item 3'], // test array
      idx = 0, // start index
      len = arr.length; // stop
    for ( ; idx < len; idx++) {
      // output item
      console.log(arr[idx]);
    }
    ```
* Explain the difference between mutable and immutable objects.
  * What is an example of an immutable object in JavaScript?
  * What are the pros and cons of immutability?
  * How can you achieve immutability in your own code?
    * Ans (Will focus on native immutable feature):
      * Explain the difference between mutable and immutable objects
        * mutable: something you can change easily
        * immutable: something you cannot change
      * What is an example of an immutable object in JavaScript
      ```javascript
      // string is immutable
      // you cannot change it with something like str[1]='b'
      var s = `foo`;
      ```
      or
      ```javascript
      var obj = {'a': 'aaa', 'b': function () {};}
      // obj becomes immutable
      Object.freeze(obj);
      ```
      * What are the pros and cons of immutability?
        * pros: 
          * Makes an object reliable, you can make sure anything in it will not be changed
          * Prevent some data accidently modified by other function when you pass it in
          * Prevent something like "bug caused by shared data is modified accidently"
        * cons:
          * Probably need more memory since you need clone the object when you want some different property (speed probably also slower)
    * Ref:
      * [Are JavaScript strings immutable? Do I need a “string builder” in JavaScript?](http://stackoverflow.com/questions/51185/are-javascript-strings-immutable-do-i-need-a-string-builder-in-javascript)
      * [Object.freeze()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)
      * [Any performance benefit to “locking down” JavaScript objects?](http://stackoverflow.com/questions/8435080/any-performance-benefit-to-locking-down-javascript-objects)
* Explain the difference between synchronous and asynchronous functions.
  * Ans:
    * synchronous function: all tasks below it should wait until it is done.
    ```javascript
    // call the synchronous function 
    syncFoo ();
    // nextTask should wait syncFoo finish
    // in other word, you can sure everything in
    // syncFoo are finished when run nextTask
    nextTask();
    ```
    * asynchronous functions: all tasks below it can be run while it is being processed.
    ```javascript
    // call the asynchronous functions
    // and you pass nextTask in as a callback
    // that will be called by asyncFoo when it is finished
    asyncFoo(nextTask);
    // you can not sure everything in
    // asyncFoo are finished when run otherTask
    otherTask();
    ```
    * For more information, please refer to Ref
  * Ref:
    * [Asynchronous vs synchronous execution, what does it really mean?](http://stackoverflow.com/questions/748175/asynchronous-vs-synchronous-execution-what-does-it-really-mean)
    * [What is the difference between synchronous and asynchronous programming (in node.js)](http://stackoverflow.com/questions/16336367/what-is-the-difference-between-synchronous-and-asynchronous-programming-in-node)
* What is event loop?
  * What is the difference between call stack and task queue?
    * Ans:
      * What is event loop
        * JavaScript has a concurrency model based on an "event loop". This model is quite different than the model in other languages like C or Java. 
        * The event loop got its name because of how it's usually implemented, which usually resembles
        ```javascript
        while(queue.waitForMessage()){
          queue.processNextMessage();
        }
        ```
        * A very interesting property of the event loop model is that JavaScript, unlike a lot of other languages, never blocks. Handling I/O is typically performed via events and callbacks, so when the application is waiting for an IndexedDB query to return or an XHR request to return, it can still process other things like user input.
      * What is the difference between call stack and task queue
        * call stack: Function calls form a stack of frames.
        ```javascript
        // stack: empty
        foo();
        // stack: has a frame foo
        function foo () {
          bar();
          // now there are 2 frames in stack, foo and the bar above foo
        } // foo finished, stack becomes empty again
        function bar () {

        } // bar finished, stack has foo in it
        ```
        * task queue: A JavaScript runtime contains a message queue, which is a list of messages to be processed. To each message is associated a function. When the stack is empty, a message is taken out of the queue and processed. The processing consists of calling the associated function (and thus creating an initial stack frame). The message processing ends when the stack becomes empty again.
        ```javascript
        // queue is empty here
        setTimeout(foo, 0);
        // now queue contains foo
        setTimeout(bar, 0);
        // now queue contains foo bollowed by bar
        function foo () {}
        function bar () {}
        ```
  * Ref:
    * [Concurrency model and Event Loop - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)
    * [The JavaScript Event Loop: Explained - Blog by Carbon Five](http://blog.carbonfive.com/2013/10/27/the-javascript-event-loop-explained/)


