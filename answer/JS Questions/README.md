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
  * Ref: [Immediately-Invoked Function Expression (IIFE)](http://benalman.com/news/2010/11/immediately-invoked-function-expression/), [JavaScript function declaration and evaluation order](http://stackoverflow.com/questions/3887408/javascript-function-declaration-and-evaluation-order)
* What's the difference between a variable that is: `null`, `undefined` or undeclared?
  * How would you go about checking for any of these states?
* What is a closure, and how/why would you use one?
* What's a typical use case for anonymous functions?
* How do you organize your code? (module pattern, classical inheritance?)
* What's the difference between host objects and native objects?
* Difference between: `function Person(){}`, `var person = Person()`, and `var person = new Person()`?
* What's the difference between `.call` and `.apply`?
* Explain `Function.prototype.bind`.
* When would you use `document.write()`?
* What's the difference between feature detection, feature inference, and using the UA string?
* Explain AJAX in as much detail as possible.
* Explain how JSONP works (and how it's not really AJAX).
* Have you ever used JavaScript templating?
  * If so, what libraries have you used?
* Explain "hoisting".
* Describe event bubbling.
* What's the difference between an "attribute" and a "property"?
* Why is extending built-in JavaScript objects not a good idea?
* Difference between document load event and document ready event?
* What is the difference between `==` and `===`?
* Explain the same-origin policy with regards to JavaScript.
* Make this work:
```javascript
duplicate([1,2,3,4,5]); // [1,2,3,4,5,1,2,3,4,5]
```
* Why is it called a Ternary expression, what does the word "Ternary" indicate?
* What is `"use strict";`? what are the advantages and disadvantages to using it?
* Create a for loop that iterates up to `100` while outputting **"fizz"** at multiples of `3`, **"buzz"** at multiples of `5` and **"fizzbuzz"** at multiples of `3` and `5`
* Why is it, in general, a good idea to leave the global scope of a website as-is and never touch it?
* Why would you use something like the `load` event? Does this event have disadvantages? Do you know any alternatives, and why would you use those?
* Explain what a single page app is and how to make one SEO-friendly.
* What is the extent of your experience with Promises and/or their polyfills?
* What are the pros and cons of using Promises instead of callbacks?
* What are some of the advantages/disadvantages of writing JavaScript code in a language that compiles to JavaScript?
* What tools and techniques do you use debugging JavaScript code?
* What language constructions do you use for iterating over object properties and array items?
* Explain the difference between mutable and immutable objects.
  * What is an example of an immutable object in JavaScript?
  * What are the pros and cons of immutability?
  * How can you achieve immutability in your own code?
* Explain the difference between synchronous and asynchronous functions.
* What is event loop?
  * What is the difference between call stack and task queue?
