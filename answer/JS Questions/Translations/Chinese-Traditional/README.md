#### JS 規格問題集：

* 描述 event delegation
  * 答 (簡短版): 將 event handler 綁定在某個父元素 (如 table, ul, 較外層的 div 或 document) 來處理子元素 (如 td, li 或較內層的 div) 透過 event bubbling 傳上來的 event
  * 答 (完整版): 
    * Dom event delegation 是一個透過 event bubbling (也叫 event propagation), 由某個共通父元素而非每一個子元素 (如由 ul 而非每一個 li) 來處理 UI 事件的機制
    * 透過 event delegation 可以大幅減少綁定的 event handler 的數量, 而且在動態建立子元素時不需要處理 event 的綁定
    * 因此它可以減低記憶體用量, 這對極少換頁的 SPA (Single Page Application) 來說是很有幫助的
  * 參考: [What is DOM Event delegation?](http://stackoverflow.com/questions/1687296/what-is-dom-event-delegation), 順帶一提, 最高分的那個答案很棒, 應該被 accept
* 描述 `this` 在 JavaScript 中如何運作
  * 答 (老實版): 我會用它可是不知道怎麼解釋耶科科
  * 答 (作弊版): 
    * ECMAScript 標準將 `this` 定義為 "目前 execution context (執行上下文) 中 ThisBinding (可以理解為執行上下文的本體, 更白話的說是 誰在跑這段 code) 的值"
    * 當在 global execution context 中執行 code 時 (也就是 global scope), ThisBinding 即為 global object (瀏覽器中為 window)
    * 如果是 call 某個 object 上的方法如 `obj.myMethod()` 或 `obj["myMethod"]()`, ThisBinding 即為該 object `obj`
    * 當直接 call eval 方法時, ThisBinding 不變, 誰 call 的就是誰
    * 當間接 call eval 方法時, ThisBinding 為 global object
    * 直接 call 某個方法時如 `foo();`, ThisBinding 為 global object
    * 有一些方法讓你可以直接指定 ThisBinding
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
    * 當在 Constructor 中呼叫方法時, `this` 即為建立中的物件
  * 一些範例: [Demo](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/JS%20Questions/the_this_keyword.html), [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/the_this_keyword.html), [JS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/js/the_this_keyword.js)
  * 參考: [How does the “this” keyword work?](http://stackoverflow.com/questions/3127429/how-does-the-this-keyword-work), [Scope in JavaScript](http://web.archive.org/web/20110725013125/http://www.digital-web.com/articles/scope_in_javascript/)
* 描述 prototypal inheritance 如何運作？
  * 答:
    * 首先解釋什麼是 prototype:
      * JavaScript 中所有的東西都是 object (物件)
      * 每一個物件內部都有一個連結連向它的 prototype (另一個物件)
      * 就這樣一個連一個直到最後連向 null, 這就形成了 prototype chain
    * prototypal inheritance 如何運作？
      * 可以把 JavaScript 的 object 想成用來裝屬性的動態的 "包包"
      * 當試著存取一個屬性時, 不只會在當前 object 這個包包中找, 還會延著 prototype chain 往各個 prototype 包包中找進去, 直到找到該屬性或 prototype chain 都找完了為止
  * 參考: [Inheritance and the prototype chain](https://developer.mozilla.org/en/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)
* 你如何測試你的 JavaScript？
  * 答 (英文版沒這題 囧):
    * 我不會針對 JavaScript 做測試, 而是以 Selenium 做整個頁面的測試, 遇到問題再去找
* AMD vs. CommonJS?
  * 答 (真實版): 哩供蝦? (傻笑).
  * 答 (作弊版): 
    * 它們的不同點在:
      * AMD 以瀏覽器為優先, CommonJS 則主要是為了給 server 用.
      * AMD 是非同步的, CommonJS 則以同步的方式.
    * 總的來說, AMD 因為非同步所以會有較多的 round trip, 使載入/render 時間延長, 也較不易讀, CJS 則是會比較 block 住 render process (搬來瀏覽器用的話) 看起來彈性也較低 (對動態/延遲載入來說)
  * 參考: [AMD vs Common JS & UMD](https://www.linkedin.com/pulse/amd-vs-common-js-umd-damodaran-sathyakumar).
* 解釋下列程式碼為什麼不是IIFE: `function foo(){ }();`.  (Immediately Invoked Function Expression,立即函式)
  * 答 (簡短版): `function foo(){ }` 沒有用小括號 `()` 括起來
  * 答 (完整版): 當 parser 邏到 function 關鍵字時它會當你要定義一個方法, 然後它就會把你的程式看成
  ```javascript
  // 定義方法
  function foo(){ }
  // ??? 不知道這兩個小括號是?
  ();
  ```
  因為 JS 執行分兩個階段, 編譯與執行, function declaration (定義方法) 會在編譯階段處理, 然後執行階段就只有 `();` 能執行, 然後造成 SyntaxError
  * 需要修改那裡使它成為IIFE?
    * 答: 要做的是將 function declaration (定義方法) 改成 function expression (函數表達), 例如將 `function foo` 改成 `var foo = function` 或以小括號把 `function foo () {}` 括起來 (因為小括號內只能有表達式, 這樣做會讓 `function foo () {}` 被當成一個參數), 如下
      * 改為 function expression
      ```javascript
      var foo = function(){
        // code to run
      }();
      ```
      * 用括號括起來
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
  * 範例: [Demo](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/JS%20Questions/IIFE_test.html), [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/IIFE_test.html), [JS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/js/IIFE_test.js)
  * 參考: [Immediately-Invoked Function Expression (IIFE)](http://benalman.com/news/2010/11/immediately-invoked-function-expression/), [JavaScript function declaration and evaluation order](http://stackoverflow.com/questions/3887408/javascript-function-declaration-and-evaluation-order)
* `null`、`undefined`和 `undeclared`變數之間有什麼差異？
  * 答: 
    * `undeclared` 變數沒有被宣告, 不存在
    * `undefined` 宣告變數了但還沒有附值
    * `null` 宣告變數並指定其值為 `null`
  * 你如何檢查？
    * 答: 如以下程式
    ```javascript
    function testStatus () {
        try { // 未被宣告會拋出 exception
            if (val) {};
        } catch (e) { // 回傳例外或訊息
            return e;
        }
        if ((typeof val) === 'undefined') // 判斷是否為 undefined
            return 'val is undefined';
        if (val == null) // 判斷是否為 null
            return 'val is null';
    }
    ```
  * 範例: [Demo](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/JS%20Questions/null_undefine_undeclare.html), [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/null_undefine_undeclare.html), [JS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/js/null_undefine_undeclare.js)
  * 參考: [JS: null, undefined, and undeclared](http://lucybain.com/blog/2014/null-undefined-undeclared/), [What is the difference in Javascript between 'undefined' and 'not defined'?](http://stackoverflow.com/questions/833661/what-is-the-difference-in-javascript-between-undefined-and-not-defined)
* 什麼是 closure, 如何/為什麼使用?
  * 答:
    * Closures 是參考一組獨立變數的 functions. 也就是在 closure 中定義的 function 會記住它被創建當下的環境.
    * 如何/為什麼使用: 可以用來將某些資料私有化 (無法由外部修改), 保時 global scope 乾淨, 避免因錯誤操作造成的問題等 (因為私有不可能被操作), 例如想像你寫了一個 JS Game, 你會想將整個 Game 的環境私有化以確保不會被修改
  * 範例: [Demo](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/JS%20Questions/closure.html), [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/closure.html), [JS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/js/closure.js)
  * 參考: [Closures - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)
* anonymous functions 典型的使用時機？
  * 答: jQuery 等等的 event handler
  ```javascript
  $('.selector').on('click', function (e) {
    // this is anonymous functions
  });
  ```
  或 IIFE
  ```javascript
  (function () {
    //  IIFE with anonymous functions
  })();
  ```
* 你如何架構你的程式碼？ (module pattern, classical inheritance?)
  * 答:
    * 我使用 object literal (物件實字) 把我的程式模組化/元件化, 操作 prototype chain 來模擬類別及繼承
    * 我會寫一些 API 將定義類別、繼承類別、生成實體及呼叫 super 方法等等的實做包裝起來, 如此我就能透過單純的物件實字來作業
  * 範例: [Demo](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/JS%20Questions/code_organization.html), [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/code_organization.html), [JS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/js/code_organization.js)
  * 注意:
    * 以上範例只是個簡單的 demo case, 實際使用上可能會將 code 分散在多個 JS 檔的不同 IIFE 中, 因此會在不同的 Scope
    * 這有可能是跟不上時代的老套做法, 今日可能會使用一些現成 libraries (如 AMD/CommonJS) 來管理/組織你的 code.
  * 參考: 這招從 [ZK](https://www.zkoss.org/) 學的, 見 [zk.js](https://github.com/zkoss/zk/blob/v7.0.3/zk/src/archive/web/js/zk/zk.js#L211)
* host objects 和 native objects 有何不同？
  * 答:
    * host objects: 執行環境本身提供以完成 ECMAScript 執行環境的物件, 如 (假設為瀏覽器環境) `window`, `document`, `location`, `history`, `XMLHttpRequest`, `setTimeout`, ..., 簡言之, "環境相關" 的物件
    * native objects: ECMAScript 本身即有清楚定義的物件, 如 `Object` (constructor), `Date`, `Math`, `parseInt`, `eval`, string 方法如 `indexOf` 和 `replace`, ..., 簡言之, "規格相關 (環境無關)" 的物件
  * 參考: [What is the difference between native objects and host objects?](http://stackoverflow.com/questions/7614317/what-is-the-difference-between-native-objects-and-host-objects)
* `function Person(){}`、`var person = Person()`和`var person = new Person()`之間有何不同？
  * 答:
    * `function Person(){}` 宣告一個 Person 方法.
    * `var person = Person()` 宣告一個叫 person 的變數, 它的值是呼叫 Person 方法的回傳值
    * `var person = new Person()` 宣告一個叫 person 的變數, 它的值是以 Person 方法當建構子建立的物件
    * (疑問) 不確定這題是要問啥
* `.call` 和 `.apply`有何不同？
  * 答 (老實版): 嗯...不是很清楚耶科科, 反正我都用 apply 沒在用 call
  * 答 (作弊版):
    * `.apply` 讓以將參數們透過 array 傳遞; `.call` 則需要確實的一個一個塞參數
    * (當明確知道參數能一個一個塞時) `.call` 的效能比較好, [call vs apply - jsperf](http://jsperf.com/test-call-vs-apply/3)
  * 參考: [What is the difference between call and apply?](http://stackoverflow.com/questions/1986896/what-is-the-difference-between-call-and-apply)
* 描述 `Function.prototype.bind`?
  * 答: bind() 方法會基於呼叫它的 function 來建立一個新 function, 呼叫 bind() 方法時可以傳入一串參數, 其中的第一個會被當成呼叫新建立的 function 時的 ThisBinding, 之後的會依續被塞到呼叫新 function 時所傳入的參數之前
  * 簡單示例:
  ```javascript
  function foo (a, b, c) {}
  var obj = {},
    bar = foo.bind(obj, 1, 2);
  // 相當於 foo(1, 2, 3) 且其中 this 會是 obj
  bar(3);
  ```
  * 範例: [Demo](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/JS%20Questions/function_bind.html), [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/function_bind.html), [JS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/JS%20Questions/js/function_bind.js)
  * 參考: [Function.prototype.bind() - JavaScript | MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind)
* 你什麼時候優化你的程式？
  答 (英文版沒有): 想到的時候, 我並不會先快速寫一個較差的版本再優化, 所有寫下的東西就是當下我能想到最好的寫法, 而在寫下之後對於較複雜或常用的部份我會時常放在腦中想, 有時會想到更好的做法
* 你什麼情況會使用 `document.write()`？ (多數的廣告產生仍然使用 `document.write()` 雖然這樣用會令人皺眉)
  * Ans: 過去不曾使用, 以下為找到的
    * 用來補缺的東西 (就是比 create script 元素設 src 寫起來方便一點這樣),
    ```html
    <!-- Grab Google CDN's jQuery, with a protocol relative URL; fall back to local if offline -->
    <script src="//ajax.googleapis.com/ajax/libs/jquery/1.6.3/jquery.min.js"></script>
    <script>window.jQuery || document.write('<script src="js/libs/jquery-1.6.3.min.js"><\/script>')</script>
    ```
    * 對某些 lib 來說這很好用
      * 這讓 script 比較小 (如上所示寫起來方便, code 也少)
      * 某些 case 下不用操心蓋到 onload event (還是寫起來方便)
      * 相容性很好
  * 參考: [JS: When would you use document.write()?](http://lucybain.com/blog/2015/js-document-write/), [Why is document.write considered a “bad practice”?](http://stackoverflow.com/questions/802854/why-is-document-write-considered-a-bad-practice)
* feature detection, feature inference, 和使用 UA string 有什麼不同？
  * 答:
    * Feature detection 直接檢查某個功能存不存在, e.g.:
    ```javascript
    // 檢查有沒有 XMLHttpRequest
    if (window.XMLHttpRequest) {
      new XMLHttpRequest();
    }
    ```
    * Feature inference 透過檢查別的功能來推斷某個功能存不存在, e.g.:
    ```javascript
    // 檢查有沒有 document.getElementsByTagName
    if (document.getElementsByTagName) {
      // 若有就推論有 document.getElementById
      element = document.getElementById(id);
    }
    ```
    * 使用 UA string 是個不應該再用的老做法 (個人七年前看人用過). 因為那要檢查一堆版本很不方便, 還是直接用前兩種吧, e.g.:
    ```javascript
    // 請問你是 IE7 嗎?
    if (navigator.userAgent.indexOf("MSIE 7") > -1){
      //do something
      // 建議輸出訊息請他裝 Chrome/FF 然後結束這個回合
    }
    ```
* 盡可能的詳述描述 AJAX。
  * 答 (誠實版): 就是一個可以像鴨子偷偷在水底滑水那樣, 讓你利用 JavaScript 在背後偷偷和 Server 交換資料, 讓你不用 reload 就能改變頁面狀態
  * 答 (狀態顯示為 "囧rz"):
    * Ajax (asynchronous JavaScript and XML 的縮寫) 是一組 web 開發方法, 它使用許多 client 端的 web 技術來建立異步 web 應用, 使用 Ajax, web 應用可以在不影響當前頁面的情況下異步地發送資料到 Server 或從 Server 端取得資料, 藉由這樣將 資料交換 與 頁面顯示 之間的耦合去除, Ajax 讓網頁/web 應用能動態的改變頁面內容而不必整頁 reload, 資料可以透過 XMLHttpRequest 取得, 另外雖然名稱中有 XML 不過並不一定要使用 XML, JSON 是更常用的, 也有人將用 JSON 的做法稱為 AJAJ (是說反正都回傳 String 怎麼不叫 AJAS?), 又雖然名稱中有 Asynchronous 但其實也可以是同步的
    * Ajax 不是指單一的一種技術, 而是一組技術的集合, HTML 用以標記資訊, CSS 用來調整樣式, JavaScript 用來使用 XMLHttpRequest 跟 Server 交換資料並操作 DOM 以達成動態呈現
  * Ref: [AJAX - wiki](https://en.wikipedia.org/wiki/Ajax_%28programming%29)
* 描述 JSONP 如何運作 (且為何它不是真正的 AJAX)。
  * 答:
    * JSONP 如何運作
      * 利用 JavaScript 動態插入一個 script 元素向 Server 要求某個 JavaScript 檔案 (可能在 src 上有加一些參數), 假設是 `http://www.example.net/sample.aspx?callback=mycallback`, 這告訴 Server 你有個 callback 方法叫 mycallback
      * Server 回傳一個 JS 檔案 (可能是動態生成的), 假設如下
      ```javascript
      // 這是回傳的 JS 檔的內容
      // server 呼叫 mycallback 並傳入一些資料
      mycallback({ foo: 'bar' });
      ```
      * 當 script 載入後它就會被執行, 然後你的方法就會被呼叫了, 這樣可以避開 cross-domain 的限制 (AJAX 則無法避開, 要 Server 允許), 注意這並不表示 JSONP 比較危險, 因為回傳 JS 的動作及內容本身還是 Server 支援並生成的
    * 且為何它不是真正的 AJAX
      * AJAX 有 cross-domain 問題, 除非使用 Cross-origin resource sharing (跨域資源共享), JSONP 是繞過 AJAX 受到 same domain policy 限制的技巧 (假設 Server 不開放跨域資源共享下的替代方案).
      * Ajax calls 可以是異步或同步的, JSONP 只能是異步的 (插入 script 後就只能等它載入後執行, 無法阻擋其它 JS 被執行).
  * 參考: [What is JSONP all about?](http://stackoverflow.com/questions/2067472/what-is-jsonp-all-about), [I don't get how JSONP is ANY different from AJAX](http://stackoverflow.com/questions/10289789/i-dont-get-how-jsonp-is-any-different-from-ajax)
* 你是用過 JavaScript templating (樣板) ？
  * 如果有的話，你有用過哪些 libraries？ (Mustache.js, Handlebars … 等)
    * 答: 有, Angularjs
* 描述 "hoisting"
  * 答:
    * 在 JavaScript 中, 函數及變數定義是被 "提前" 的, 它們相當於會被提前到所屬 scope 的頂端
    * 因此你可以在函數或變數定義之前先使用它 (只是寫起來像那樣)
  * 例:
    * 變數
    ```javascript
    // 你寫的 code
    foo = 2
    var foo;

    // 實際運作的狀況:
    var foo;
    foo = 2;
    ```
    * 函數
    ```javascript
    // 先呼叫
    hoisted(); // logs "foo"

    // 定義在下面
    function hoisted() {
      console.log("foo");
    }
    ```
  * 參考: [Hoisting - MDN](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting)
  * 另外也看看這成因: [JavaScript function declaration and evaluation order](http://stackoverflow.com/questions/3887408/javascript-function-declaration-and-evaluation-order)
* 描述 event bubbling.
* "attribute" 和 "property" 的不同？
* 為什麼擴展 JavaScript 內建的 objects 不是個好方法？
* document load event 和 document ready event 有什麼不同？
* `==` 和 `===` 有什麼不同？
* 描述 JavaScript 的 same-origin policy (同源策略)
* 實作如下程式:

```javascript
duplicate([1,2,3,4,5]); // [1,2,3,4,5,1,2,3,4,5]
```

* Ternary expression 怎麼來的, "Ternary" 的意思是什麼？
* 什麼是 `"use strict";`? 使用他的優點和缺點是什麼？
* Create a for loop that iterates up to `100` while outputting **"fizz"** at multiples of `3`, `"buzz"` at multiples of `5` and **"fizzbuzz"** at multiples of `3` and `5`
