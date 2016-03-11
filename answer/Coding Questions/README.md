
#### Coding Questions:

*Question: What is the value of `foo`?*
```javascript
var foo = 10 + '20';
```

  * Ans: 1020

*Question: How would you make this work?*
```javascript
add(2, 5); // 7
add(2)(5); // 7
```

  * Ans:

  ```javascript

  function add (a, b) {
    if (typeof a == "undefined") return null;
    if (typeof b == "undefined") {
      return function (c) {
        return add(a, c);
      }
    }
    return a+b;
  }
  ```

*Question: What value is returned from the following statement?*
```javascript
"i'm a lasagna hog".split("").reverse().join("");
```

  * Ans: `"goh angasal a m'i"`

*Question: What is the value of `window.foo`?*
```javascript
( window.foo || ( window.foo = "bar" ) );
```

  * Ans: `"bar"`

*Question: What is the outcome of the two alerts below?*
```javascript
var foo = "Hello";
(function() {
  var bar = " World";
  alert(foo + bar);
})();
alert(foo + bar);
```

  * Ans: The inner one (within IIFE) alert `Hello World`, outer one cause ReferenceError since global bar is not declared

*Question: What is the value of `foo.length`?*
```javascript
var foo = [];
foo.push(1);
foo.push(2);
```

  * Ans: 2

*Question: What is the value of `foo.x`?*
```javascript
var foo = {n: 1};
var bar = foo;
foo.x = foo = {n: 2};
```

  * Ans: `undefined`

*Question: What does the following code print?*
```javascript
console.log('one');
setTimeout(function() {
  console.log('two');
}, 0);
console.log('three');
```

  * Ans: 

  ```javascript
  
  one
  three
  two
  ```