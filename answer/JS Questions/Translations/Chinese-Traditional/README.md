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
    * 答: 要做的是將 function declaration (定義方法) 改成 function expression (函式表達), 例如將 `function foo` 改成 `var foo = function` 或以小括號把 `function foo () {}` 括起來 (因為小括號內只能有表達式, 這樣做會讓 `function foo () {}` 被當成一個參數), 如下
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
      * 還可以改成暱名函式
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
    * `.apply` 可以將參數們透過 array 傳遞 (使用內建 arguments 參數你甚至不用知道有什麼參數傳入); `.call` 則需要確實的一個一個塞參數
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
    * 在 JavaScript 中, 函式及變數定義是被 "提前" 的, 它們相當於會被提前到所屬 scope 的頂端
    * 因此你可以在函式或變數定義之前先使用它 (只是寫起來像那樣)
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
    * 函式
    ```javascript
    // 先呼叫
    hoisted(); // logs "foo"

    // 定義在下面
    function hoisted() {
      console.log("foo");
    }
    ```
  * 參考: [Hoisting - MDN](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting)
  * 另外也看看這個, 應該是成因: [JavaScript function declaration and evaluation order](http://stackoverflow.com/questions/3887408/javascript-function-declaration-and-evaluation-order)
* 描述 event bubbling.
  * 答: 它是一種 HTML DOM 傳播 event 的方式, 也就是當你觸發一個 event 時 (假設是 click), 第一個接收到該 event 的是最內層的元素 (假設是某個 li), 然後才一路往外層元素傳遞 (ul, 然後或許更外層的 div, 一直到 body document)
  * 參考: [JavaScript HTML DOM EventListener](http://www.w3schools.com/js/js_htmldom_eventlistener.asp)
* "attribute" 和 "property" 的不同？
  * 答: 
    * attribute 就是 HTML code 中那些 tag 的屬性, 如 `<input type="text" value="val" />` 這裡的 type 跟 value 就是 attribute, property 則是瀏覽器 parse 了該 HTML code 建立一個 DOM node 後, 這個 node 是一個 object, 然後它會有 type 及 value 這些屬性
    * 某些 attribute 會直接對應到 property, 如 ID
    * 某些則否, 如 value attribute 指的是初始值, 但 value property 則是目前值
  * 參考: [HTML - attributes vs properties [duplicate]](http://stackoverflow.com/questions/19246714/html-attributes-vs-properties), [Properties and Attributes in HTML](http://stackoverflow.com/questions/6003819/properties-and-attributes-in-html)
* 為什麼擴展 JavaScript 內建的 objects 不是個好方法？
  * 答:
    * 因為很可能之後產生衝突或預期外的結果
    * 例如:
      * 假設你加了一個 Array.duplicate 方法, 之後原生的 JS 也加了該方法, 但意義不同, 那別人使用該方法 (他認為在用原生的) 時就會無法達到他想要的結果
      * 假設你加了一個方法, 某個你使用的 lib 後來也加了同樣的方法, 則可能會讓你的應用產生錯誤.
  * 答: [Why is extending native objects a bad practice?](http://stackoverflow.com/questions/14034180/why-is-extending-native-objects-a-bad-practice)
* document load event 和 document ready event 有什麼不同？
  * 答:
    * `$(document).ready()`: HTML Document 載入完成, DOM 準備好了, 大部份的情形下可以在這時候執行 Script
    * `window.onload = funcRef;`: assets 都載入了, 假設一段 Script 需要取得圖片寬高等載入完成才會知道的資訊就寫在這裡
  * 參考:
    * [jQuery - What are differences between $(document).ready and $(window).load?](http://stackoverflow.com/questions/8396407/jquery-what-are-differences-between-document-ready-and-window-load)
    * [.ready() | jQuery API Documentation](https://api.jquery.com/ready/)
    * [onload Event | w3school](http://www.w3schools.com/jsref/event_onload.asp)
    * [GlobalEventHandlers.onload](https://developer.mozilla.org/en/docs/Web/API/GlobalEventHandlers/onload)
* `==` 和 `===` 有什麼不同？
  * Ans: 
    * `==`: 假如兩個值是 相同的 String, 相同的數字, 相同的布林值或相同的物件即視為相等, 若它們是不同的類型但可透過自動轉型為前述狀況也行
    * `===`: 類似於 `==`, 不同點在不會做自動轉型, 也就是連型態都要一致才會被視為相等.
    * some examples: 
      ```javascript
      1 == '1'; // true
      1 == true; // true
      1 === '1'; // false
      1 === true; // false
      new Date() == new Date().toString(); // true
      new Date() === new Date().toString(); // false
      ```
  * 參考:
    * [Does it matter which equals operator (== vs ===) I use in JavaScript comparisons?](http://stackoverflow.com/questions/359494/does-it-matter-which-equals-operator-vs-i-use-in-javascript-comparisons)
    * [JavaScript tutorial: Comparison operators](http://www.c-point.com/javascript_tutorial/jsgrpComparison.htm)
* 描述 JavaScript 的 same-origin policy (同源策略)
  * 答:
    * same-origin policy 限制一個 origin 的 document/script 能如何與其它 Origin 的資源進行互動, 它是防止惡意文件的關鍵安全機制
    * 兩個頁面若 protocol (http/https), port 及 host 都相同則被視為同一個 origin
  * 參考: [Same-origin policy - Web security | MDN](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)
* 實作如下程式:

```javascript
duplicate([1,2,3,4,5]); // [1,2,3,4,5,1,2,3,4,5]
```

  * 答:
  ```javascript
  
  function duplicate (a) {
    return a.concat(a);
  }
  duplicate([1,2,3,4,5]); // [1,2,3,4,5,1,2,3,4,5]
  ```

  * 參考:
    * [Javascript fastest way to duplicate an Array - slice vs for loop](http://stackoverflow.com/questions/3978492/javascript-fastest-way-to-duplicate-an-array-slice-vs-for-loop)

* Ternary expression 怎麼來的, "Ternary" 的意思是什麼？
  * 答:
    * 它使用三個元素, `a? b : c;`, 其中 a 是條年, b/c 是當 a 為 true/false 時的回傳值
    * Ternary 意指它使用的元素數目 (3)
  * 參考:
    * [Ternary operation](https://en.wikipedia.org/wiki/Ternary_operation)
    * [Arity](https://en.wikipedia.org/wiki/Arity)
* 什麼是 `"use strict";`? 使用他的優點和缺點是什麼？
  * 答:
    * 它會使用 strict mode 來執行你的 code, 這會改變 JavaScript 執行起來的行為, 大部份的改變是會讓不好的寫法 (如使用未宣告的變數) 報錯 (在非 strict mode 時則是自動變成全域變數),
    * 優點很明顯的是可以協助 (逼迫? XD) 你避免使用某些明顯有問題的寫法
    * 缺點則是某些在非 strict mode 下開發的東西在 strict mode 下會無法運作, 而某些在 strict mode 下開發的東西在非 strict mode 下無法運作, 因為 strict mode 不是一個子集而是行為會不同, 又並非所有的瀏覽器都支援 strict mode, 因此不論支不支援的瀏覽器都要測
  * 參考:
    * [Strict mode - JavaScript | MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Strict_mode)
    * [What does “use strict” do in JavaScript, and what is the reasoning behind it?](http://stackoverflow.com/questions/1335851/what-does-use-strict-do-in-javascript-and-what-is-the-reasoning-behind-it)
    * [JavaScript: Why the hatred for strict mode?](http://www.2ality.com/2011/10/strict-mode-hatred.html)
* Create a for loop that iterates up to `100` while outputting **"fizz"** at multiples of `3`, `"buzz"` at multiples of `5` and **"fizzbuzz"** at multiples of `3` and `5`
  * 答:
    * 這題可能測這些東西
      * 使用 array.push 串接 (可能量很大的) 字串
      * 試著找出規則來減少 loop 數 (這裡是用上公倍數)
      * 盡量減少操作 DOM 的次數, 不要每一次輸出都直接操作 DOM
      * 以 var 定義變數, 且是在 loop 之外
    * 程式:
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
* 為何一般來說最好不要使用 global scope 呢?
  * 答:
    * 任何地方的任何 code 都能夠存取 global scope, 如果大家都用 global scope 可能有變數/函式撞名, 先載入的會被蓋掉
    * 全域變數因為一些原因效能較差:
      * 假設你沒有明確指定 (使用 `gvar` 而非 `window.gvar`) 那 browser 要從當前 scope 開始, 一層一層延著 scope chain 找到 global scope 為止.
      * global scope 通常比 local function scope 有更多東西, browser 找某個變數要找比較久
      * 反正 Global 就是慢 (叭~~~).
  * 參考: 
    * [JavaScript best practices | w3](https://www.w3.org/wiki/JavaScript_best_practices)
    * [Global Variables vs Local Variables | jsPerf](https://jsperf.com/global/4)
    * [30 Tips To Improve Javascript Performance](https://www.monitis.com/blog/2011/05/15/30-tips-to-improve-javascript-performance/)
    * [JavaScript variable performance](https://www.nczonline.net/blog/2009/02/10/javascript-variable-performance/)
* 你何時會用 `load` event? 它有什麼缺點嗎? 有什麼替代方案? 為何你會用那些替代方案?
  * 答:
    * 你何時會用 `load` event?
      * 當我需要等某個東西載入完後才能做事時, 如:
      * 假設我要算某個 block 的 size, 那就得等它所有內容含圖片載完, CSS 套完, 會改動它的 JS 跑完最後才能算
      * 假設有一段 JS 要等其它 JS (可能是第三方 lib) 載完才能跑
    * 它有什麼缺點嗎?
      * 是的, 它有時會使速度變慢, 造成不好的結構或破破的結果
      * 例如 `window.onload`, 它會等 *所有* 東西都載完後才執行, 因此會比較慢 (相對於只等你真的需要的東西載完而言).
      * 對彼此獨立的兩個東西來說, 很難確保它們載入完成的先後次序
        * 注意: 尤其動態加入的 script tag 是會以非同步的方式載入
      * 有部份 lib 本身即有分段載入, 也就是它的 onload 只是你加載的那一個 script 載入了, 但是該 script 還會再載入其它東西 (而你需要的其實是那其它東西)
      * 假如有一長串 script 一個接著一個都要聽別人的 onload, 那也會灰熊慢
      * 要綁 onload 事件你非得拿到 DOM 不可, 讓你的 code 彈性更小
    * 有什麼替代方案? 為何你會用那些替代方案?
      * 自訂 event, 如
      ```javascript
      // 這是在被相依的 JS 檔

      // 載入後立馬標記一個 flag
      window.myNameSpace.loaded['hey-I-am-loaded'] = true;
      // 並觸發 event
      $(document).trigger('hey-I-am-loaded');

      // ###
      ```

      ```javascript
      // 這是在相依於上面那個檔的 JS 檔
      var executed; // 這是檔案內用來標記是否執行過的 flag
      // 當收到 event 時, 執行
      // (這是這個檔比較早載完的情形)
      $(document).on('hey-I-am-loaded', run);

      // 這是要被執行的部份
      function run () {
        // 假如還沒被執行且相依的檔載入了, 執行
        if (!executed && window.myNameSpace.loaded['hey-I-am-loaded']) {
          executed = true;
          // 做事情
          // ...
          // 然後也把自己標記為已載入
          // 並觸發 event
          window.myNameSpace.loaded['hey-I-am-also-loaded'] = true;
          $(document).trigger('hey-I-am-also-loaded');
        }
      }
      // 假如這個檔比較晚載完, 沒聽到 event 的話
      // 直接 call run 方法, run 方法內檢查要執行就會執行
      run();
      ```
      * 直接由 script 生成相依的內容, 確保不會漏掉 event
      ```html
      <!-- 這是用來放圖的區塊 -->
      <div class="the-img-area"></div>
      ```
      ```javascript
      // 由 script 建立 img 元素
      var img = new Image();
      // 然後立馬綁定 event
      // 所以你可以肯定不會漏掉
      img.onload = foo;
      img.src = src;
      $('.the-img-area').append(img);
      ```
      * 有時候你只能跑個 timer 一直檢查
      ```javascript
      var cnt = 0,
        timer = setInterval(function () {
        // 建個 timer 每秒檢查一次
        if (cnt > 10) {
          // 超過十秒就顯示錯誤訊息並停掉 timer
          outputErrorMessage();
          clearInterval(timer);
        } else if (window.theLibThatDoesntProvideEventOrCallback
          && window.someMoreThingsLoadedByThatLib) {
          // (上面兩行) 如果那個沒提供 callback 機制的爛 lib
          // 以及那個爛 lib 自行載入的別的東西都存在的話
          // 就執行並停掉 timer
          foo();
          clearInterval(timer);
        }
        cnt++;
        // check every 1000 ms
      }, 1000);
      ```
      * 也可以試試下面提到的 Promise, 等支援度更完整一些我會視情況使用
* 解釋什麼是 single page app 及如何使它 SEO-friendly.
  * Ans:
    * 解釋什麼是 single page app: single-page application (SPA) 是一個只使用一個頁面來提供如桌面應用般更流暢的使用體驗的 網站/網頁應用, 在 SPA 中一旦頁面載入後, 會根據狀態變化或使用者操作動態的改變頁面內容 (通常透過 AJAX), 不會有頁面 reload/redirect 的情形
    * 如何使它 SEO-friendly
      * 使用 url hash 建立可流覽的頁面
      * 適當的輸出 html 內容如 heading, list (ul/li), 超連結等, 或許以 CSS 讓使用者看不到只有爬蟲看得到
      * 可以試著偵測機器人爬蟲提供為 SEO 優化的內容
      * 可以使用 headless browser 在 server 端生成頁面的 snapshot
  * 參考: 
    * [Single-page application](https://en.wikipedia.org/wiki/Single-page_application)
    * [HOW TO OPTIMIZE SINGLE PAGE SITES FOR SEARCH ENGINES](http://www.webdesignerdepot.com/2013/10/how-to-optimize-single-page-sites-for-search-engines/)
    * [A proposal for making AJAX crawlable](https://googlewebmastercentral.blogspot.tw/2009/10/proposal-for-making-ajax-crawlable.html)
* 你對使用 Promises 或其 polyfills (應是非 Native 的實做) 有什麼的經驗?
  * 答:
    * 我在開發 NodeJS 後端時 (好吧非前端, 也可能是 polyfill) 時有用它來處理一些非同步的工作如連接 MongoDB 然後取資料做處理, 用它把一個個非同步的 task 串起來等等, 它讓 code 比未整理好的巢狀 callback 好讀一些, 並且不像 event 機制會有觸發順序的問題
    * 一些好文: [JavaScript Promises](http://www.html5rocks.com/en/tutorials/es6/promises/?redirect_from_locale=tw), [JavaScript Promises With Node.js](http://zpalexander.com/blog/javascript-promises-node-js/)
* 使用 Promises 取代 callback 有何優缺點?
  * 答:
    * 優:
      * 避免 [callback hell](http://stackoverflow.com/questions/25098066/what-is-callback-hell-and-how-and-why-rx-solves-it)
    * 缺:
      * 現在有很多版本的 Promise. (有 Native, 及 [這個列表的其它實做](http://complexitymaze.com/2014/03/03/javascript-promises-a-comparison-of-libraries/))
      * 某些版本的 Promise 會讓你非常難 Debug.
      * 它同時也增加了 code 的複雜度.
  * 參考:
    * [How to debug javascript promises?](http://stackoverflow.com/questions/25827234/how-to-debug-javascript-promises)
    * [Promises/A+ Considered Harmful](http://robotlolita.me/2013/06/28/promises-considered-harmful.html)
    * [Promises vs Callbacks – Code comparison](http://lkrnac.net/blog/2014/10/promises-vs-callbacks-comparison/)
* 使用再包裝語言來寫 JavaScript 有何優缺點?
  * 答 (以 CoffeeScript 為例):
    * 優
      * 會督促你使用好的 JavaScript Pattern
      * 會協助你避免 Anti-Pattern
      * 讓程式碼更短可讀性更高
    * 缺
      * 編譯可能造成困擾. (敲有感!)
      * 可能比較難 debug (因為你寫的跟你看到 bug 的 code (編譯後的) 不同)
      * 它可能是容易改變 (然後你也得改) 的.
      * 它較小眾 (會 JS 的不一定會 CS).
    * 缺 (一些個人補充)
      * 優點也可能造成一些壞處如,
        * 變數會 auto-scoped -> 一開始就寫 CoffeeScript 的人可能就會缺乏 scope 的知識.
        * 讓程式碼更短可讀性更高 -> 可能就會讓你不會寫原生 JavaScript.
      * 縮排影響編譯結果. 不熟之前很不方便.
      * 換行影響編譯結果. 不熟之前很不方便, 且有時候因此不得不寫怪怪的 code.
      * 以上這兩點尤其在寫 inline 物件實字時超討厭的科科.
  * 參考:
    [What are the pros and cons of Coffeescript?](http://programmers.stackexchange.com/questions/72569/what-are-the-pros-and-cons-of-coffeescript)
* 你用什麼工具或技術來為你的 JavaScript 除錯?
  * Ans:
    * 我通常用 Chrome Developer tools
    * 我會設中斷點, 用 `console.log console.trace console.table console.error` 等方法 log 資訊, 用 `debugger` 關鍵字 (也是中斷點)
    * 有篇很棒的除錯技巧文章 [Useful Javascript debugging tips you might not know](https://raygun.io/blog/2015/06/useful-javascript-debugging-tips-you-didnt-know/)
* 你如何 iterating 物件的所有屬性或陣列的所有元素?
  * 答
    * iterating 物件的所有屬性
    ```javascript
    var obj = {'k1' : 'v1', 'k2' : 'v2'}, // 測試用物件
      key; // 用來搭 in 關鍵字用的變數
    for (key in obj) {
      console.log(key); // 屬性名稱
      console.log(obj[key]); // 屬性值
    }
    ```
    * iterating 陣列的所有元素
    ```javascript
    var arr = ['item 1', 'item 2', 'item 3'], // 測試用 array
      idx = 0, // 開始 index 為 0
      len = arr.length; // 總 length 先存起來
    for ( ; idx < len; idx++) {
      // for 廻圈, 輸出每個元素
      console.log(arr[idx]);
    }
    ```
    * 我知道有 foreach, 但支援度不是很好某些環境沒有
* 說明 mutable 和 immutable objects 的不同.
  * 舉個 immutable object 的例子?
  * immutability (不可變性) 有什麼優缺點?
  * 你如何在程式中實現 immutability?
    * 答
      * 說明 mutable 和 immutable objects 的不同
        * mutable: 就是你可以改
        * immutable: 就是你不能改
      * 舉個 immutable object 的例子?
      ```javascript
      // string 是 immutable 的
      // 你不能改動它的某個字元如 str[1]='b'
      var s = `foo`;
      ```
      或者使用 API
      ```javascript
      var obj = {'a': 'aaa', 'b': function () {};}
      // 以下這行讓 obj 的內容通通不能改, 改了不會變
      Object.freeze(obj);
      ```
      * immutability (不可變性) 有什麼優缺點?
        * 優: 
          * 可以確保某個東西不會被改
          * 因此可以確保傳給某個 function 的物件拿回來能繼續用
          * 也可以避免意外的 side effect
        * 缺:
          * 若需要更動時就得 clone 該物件, 因此可能會佔比較多記憶體也比較慢
    * 參考:
      * [Are JavaScript strings immutable? Do I need a “string builder” in JavaScript?](http://stackoverflow.com/questions/51185/are-javascript-strings-immutable-do-i-need-a-string-builder-in-javascript)
      * [Object.freeze()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)
      * [Any performance benefit to “locking down” JavaScript objects?](http://stackoverflow.com/questions/8435080/any-performance-benefit-to-locking-down-javascript-objects)
* 說明同步 (synchronous) 和異步 (asynchronous) functions 的差異.
  * 答:
    * 同步 function: 在它跑完前不能做別的事.
    ```javascript
    // call 一個同步 function 
    syncFoo ();
    // nextTask 得等 syncFoo 整個跑完
    // 換句話說在 nextTask 裡你可以有把握
    // syncFoo 已經整個跑完了
    nextTask();
    ```
    * 異步 function: 在它跑完前可以做別的事.
    ```javascript
    // call 一個異步 function
    // 並且傳入 nextTask 當 callback
    // asyncFoo 會在它結束後呼叫 nextTask
    asyncFoo(nextTask);
    // 而跑下面這個 otherTask 時
    // 你無法確認 asyncFoo 執行到哪了
    otherTask();
    ```
    * 更多資訊請參考以下
  * 參考:
    * [Asynchronous vs synchronous execution, what does it really mean?](http://stackoverflow.com/questions/748175/asynchronous-vs-synchronous-execution-what-does-it-really-mean)
    * [What is the difference between synchronous and asynchronous programming (in node.js)](http://stackoverflow.com/questions/16336367/what-is-the-difference-between-synchronous-and-asynchronous-programming-in-node)
* 什麼是 event loop?
  * call stack 和 task queue 有何差異?
    * 答:
      * 什麼是 event loop?
        * JavaScript 的同步 model 就是基於 "event loop". 這和其它語言如 C/JAVA 的 model 有很大的不同 
        * event loop 的名稱是由它通常的實做方式來的, 大約像以下
        ```javascript
        // 這裡應是指瀏覽器如何實做
        // queue 裡放的是一堆待處理 event
        // 透過一個 loop 一個一個處理
        while(queue.waitForMessage()){
          queue.processNextMessage();
        }
        ```
        * JavaScript 在處理 IO (後端 DB 存取或 XMLHttpRequest) 時不像其它語言會 block 住, 因此能同時處理其它事如 user input.
      * call stack 和 task queue 有何差異?
        * call stack: Function calls 會形成一個框框的 stack 如.
        ```javascript
        // 這裡 stack 是空的
        foo();
        // 現在 stack 裡有 foo
        function foo () {
          // call bar 時 stack 中 foo 上面又被堆上了一個 bar
          bar();
          // bar 結束, stack 裡面又只剩 foo
        } // foo 結束, stack 空了
        function bar () {

        }
        ```
        * task queue: JavaScript runtime 包含一個用來存放待處理 message 的 message queue, 每個 message 關聯到一個 function, 當 stack 是空的時候就會從 message queue 中取一個 message 來處理, 也就是 call 它關聯到的 function (因此也產生一個初始的 stack frame), 而 stack 再度清空時就是處理完畢了
        ```javascript
        // 一開始 queue 是空的
        setTimeout(foo, 0);
        // 到此你塞了 foo 進 queue
        setTimeout(bar, 0);
        // 接著你又在 foo 之後多塞了一個 bar
        function foo () {}
        function bar () {}
        ```
  * 參考:
    * [Concurrency model and Event Loop - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)
    * [The JavaScript Event Loop: Explained - Blog by Carbon Five](http://blog.carbonfive.com/2013/10/27/the-javascript-event-loop-explained/)
    
