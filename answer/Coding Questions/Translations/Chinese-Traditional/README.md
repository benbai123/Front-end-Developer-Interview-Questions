
#### <a name='coding-questions'>程式碼問題集:</a>

問題: `foo` 的值是?
```javascript
var foo = 10 + '20';
```

  * 答: 1020

問題：實作符合下面的函式

```javascript
add(2, 5); // 7
add(2)(5); // 7
```

  * 答:

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

問題: 下面的 statement(陳述式) 會回傳什麼？

```javascript
"i'm a lasagna hog".split("").reverse().join("");
```

  * 答: `"goh angasal a m'i"`

問題:  window.foo 的值是什麼？

```javascript
( window.foo || ( window.foo = "bar" ) );
```

  * 答: `"bar"`

問題: 下面的兩個 alerts 的結果會是什麼？

```javascript
var foo = "Hello";
(function() {
  var bar = " World";
  alert(foo + bar);
})();
alert(foo + bar);
```

  * 答: The inner one (within IIFE) alert `Hello World`, outer one cause ReferenceError since global bar is not declared

問題: 下面 foo.length 的值是什麼？

```javascript
var foo = [];
foo.push(1);
foo.push(2);
```

  * 答: 2

問題: `foo.x` 的值是?
```javascript
var foo = {n: 1};
var bar = foo;
foo.x = foo = {n: 2};
```

  * 答: `undefined`

問題: 以下程式會印出?
```javascript
console.log('one');
setTimeout(function() {
  console.log('two');
}, 0);
console.log('three');
```

  * 答: 

  ```javascript
  
  one
  three
  two
  ```