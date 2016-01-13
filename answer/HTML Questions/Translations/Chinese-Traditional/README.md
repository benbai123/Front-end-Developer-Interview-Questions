#### <a name='html-questions'>HTML 問題:</a>

* `doctype` 做什麼用的？
  * 答:
    * 簡短版: 告訴瀏覽器你的頁面使用的 HTML 版本
    * 詳細版: `<!DOCTYPE>` 應該定義在你頁面 HTML 的最上方 (第一行), 在 `<html>` tag 之前. `<!DOCTYPE>` 定義 本身不是一個 HTML tag; 它是給瀏覽器的指令，指出你的頁面是使用那個版本的 HTML tag 所撰寫的. 在 HTML 4.01 中, `<!DOCTYPE>` 定義會參考到一個 DTD (Document Type Definition - 文檔類型定義, 定義一份文檔應有的結構及合法的元素/屬性等), 因為 HTML 4.01 是基於 SGML 的 (一個很老但是反正要用到 DTD 的東西). DTD 會定義標記語言 (markup language, 就是 ML) 的規則, 因此瀏覽器才確定要怎麼正確的呈現你頁面的內容. HTML5 則不是基於 SGML 的, 因此 doctype 不需要參考到一份 DTD.
      * 小祕訣: 永遠在你的 HTML 文件中加入正確的 `<!DOCTYPE>` 定義, 讓瀏覽器可以預先知道要呈現的文件的類型.
      * 一些範例
      ```html
      <!-- HTML 5 的 doctype -->
      <!DOCTYPE html>
      ```
      ```html
      <!-- HTML 4.01 嚴格模式的 doctype -->
      <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
      ```
  * Ref: [w3school](http://www.w3schools.com/tags/tag_doctype.asp), [SGML - wiki](https://en.wikipedia.org/wiki/Standard_Generalized_Markup_Language)
  * 非 Ref: 但是 但是 但是 假如你不幸要支援很舊的瀏覽器跟維護一些有奇怪老舊東西的頁面的話, 你可能會需要這個 (probably you never need them) [Activating Browser Modes with Doctype](https://hsivonen.fi/doctype/)
* standards mode 和 quirks mode 有什麼不同？
  * 答:
    * quirks mode 中, layout 會模擬 Navigator 4 和 IE 5 中的非標準行為. 這對支援標準普及之前製作的老舊網站是必要的.
    * 在完全的 standards mode 中, 則 (但願) 會完全依照 HTML/CSS 規格定義的行為去運作.
    * 個人覺得現在來說只要遵循標準就好了, 不用去考慮 quirks mode
    * Ref: [Quirks Mode and Standards Mode (MDN)](https://developer.mozilla.org/en-US/docs/Quirks_Mode_and_Standards_Mode), [What is quirks mode? (SO)](http://stackoverflow.com/questions/1695787/what-is-quirks-mode)
* 使用 XHTML 有什麼限制？
  * 答: 在 XHTML 中
    * XHTML 一定要有 DOCTYPE
    * `<html>` 標籤中一定要有 xmlns 屬性
    * 一定要有 `<html>, <head>, <title>, and <body>` 這幾個 tag
    * XHTML 元素必需有正確的巢狀結構, `<b><i></b></i>` 像這樣是不行的, `<b><i></i></b>` 這樣才可以
    * XHTML 元素一定要有開始跟結束 `<input></input>` 或 `<input />`
    * XHTML 元素只能用小寫字元
    * XHTML 文件必需有唯一一個根元素
    * 屬性名稱只能用小寫字元
    * 屬性值必需以 `""` 括起來
    * 屬性省略 (無屬性值) 是禁止的, `<button disabled></button>` 這樣不行, `<button disabled="disabled"></button>` 這樣才可以
  * Ref: [HTML and XHTML (w3school)](http://www.w3schools.com/html/html_xhtml.asp)
* 如果網頁使用 `application/xhtml+xml` 會有問題嗎？
  * 答:
    * 瀏覽器就會把你的文件當成嚴格遵守 XHTML 規則的文件, 假如不是就給你報錯而非試著矯正它, 很不方便.
    * 好像還有一些其它問題 (見 Ref) 但個人看得不是佷明白
  * Ref: [What are the problems associated with serving pages with Content: application/xhtml+xml (SO)](http://stackoverflow.com/questions/351380/what-are-the-problems-associated-with-serving-pages-with-content-application-xh), [Article at Haker News](https://news.ycombinator.com/item?id=8917715)
* 你怎麼做一個需要支持多國語言的網頁？
  * 答: 
    * 確定有以 code 指定頁面語言
      * 對 html 元素套用 language code 以指定主要語言, 如 `<html lang="en">` (html) 或 `<html xmlns="http://www.w3.org/1999/xhtml" lang="en" xml:lang="en">` (xhtml)
    * 適當的控制多種語言
      * 若有特定區塊使用不同語言則對該區塊改變 lang 設置, 如 `<blockquote lang=”fr”><p>就這一塊特別使用法文.</p></blockquote>`
      * 對超連結提供連結到的頁面的語言資訊, 如 `<a href="" hreflang="fr">French</a>` (這個連結連到的頁面主要語言是法語) or `<a href="" lang="fr" hreflang="fr">Francais</a>` (這個連結連到的頁面主要語言是法語, 連結文字也是法語)
    * 注意：Google 會自行試圖找出你頁面的主要語言, 因此它建議一個頁面只使用一種語言.
    * 調整文字方向
      * 如 `<html dir="rtl">` (由右往左) 是阿拉伯語，波斯語和烏爾都語的方向。
      * 也需要以 CSS 適當的調整版面配置 (如 靠右對齊, 區塊順序調換等)
    * 指定字元編碼, 通常是 unicode `<meta http-equiv="Content-Type" content="text/html;charset=UTF-8">` (HTML 4) `<meta charset="UTF-8">` (HTML 5)
    * 不同的語言可能需要不同的 font-size (和/或 font-family)
      * 使用 pseudo class, 如 `:lang(en) {font-size: 85%;} 或 :lang(zh) {font-size: 125%;}` (CSS) 對應 `<html lang="en"> 或 <html lang="zh">` (HTML)
      * 以 language code 設為 class, 如 `<body class="en"> or <body class="zh">` 然後撰寫相關 CSS.
    * 不同的語言可能會佔用不同的空間 (如 請 vs Please).
      * 確認你旳 layout 有足夠的彈性
      * 或都調整不同語言的文字內容 (如 謝謝你的合作 -> Thank you (O) Thanks for your cooperation (X)).
      * 第一種做法應該較優
    * Ref: [7 Tips and Techniques For Multi-lingual Website Accessibility](http://www.nomensa.com/blog/2010/7-tips-and-techniques-for-multi-lingual-website-accessibility/)
* 當開發和設計一個多國語言網站時，有什麼需要小心的？
  * 答: 我對設計不熟, 僅列出以下參考資料中的幾點:
    * 使用較普遍的語言當預設語言 (如使用英文, 而非拉丁文)
    * 提供好的使用者介面切換語言
    * 依 IP 等資訊自動套用不同語言 (對在網咖用電腦的遊客可能不方便)
    * 若有內含文字的圖片, 需要有載入不同語言的圖片的機制.
    * (如上題) 留意不同語言對文字長度的影響.
    * 留意使用的顏色是否在不同文化中有特別意涵, 參考: [Colours in Culture](http://infobeautiful4.s3.amazonaws.com/2015/05/1276_colours_in_culture.png)
    * 需要翻譯的內容不要寫死在頁面中 (以特定機制從語系檔中載入).
    * 不同的語言可能有不同的日期或時間格式.

  * Ref: [What kind of things one should be wary of when designing or developing for multilingual sites?](https://www.quora.com/What-kind-of-things-one-should-be-wary-of-when-designing-or-developing-for-multilingual-sites)
* `data-` 屬性的好處在哪？
  * 答: 它讓你可以在 dom 元素帶自訂資料, 儲存額外資訊, 而且是標準屬性不用等別技巧.
    e.g.,
    ```html
      <button data-modal="#modal">show modal</button>
    ```
    data-modal 指定按下按鈕時會跳出的 modal box 的 selector
* 考慮 HTML5 作為一個開放式的網站平台。HTML5 的 building blocks 有哪些？
  * Ans:
    * 更多語意化的標籤 (`<header>, <footer>, <article>, and <section>`)
    * 更多的表單元素 (`datalist` `keygen` `output`)
    * vedio 和 audio 標籤 (`<audio> and <video>`)
    * 新的 javascript API (History, etc)
    * 新的 Graphic 元素 (`<svg> and <canvas>`)
    * 新的通訊 API (WebSocket, SSE)
    * geolocation API
    * web worker API
    * 新的 Client 端資料儲存方式 (JS LocalStorage)
  * Ref: [HTML Questions:Front-end Developer Interview Questions](http://flowerszhong.github.io/2013/11/20/html-questions.html)
* 請描述 `cookies`, `sessionStorage` 和 `localStorage` 的不同？
  * 答:
    * Cookies 主要是讓後端伺服器端讀的, local storage (含 `localStorage` 及 `sessionStorage`) 只有前端客戶端能存取
    * Cookies 在每一次的 request 會隨著 header 送出, local storage 則不會.
    * Cookies 大小的上限是 4096 bytes, local storage 則可以到 5MB/domain.
    * Cookies 必需要有期限設置, `localStorage` 則沒有期限, `sessionStorage` 則會隨著 session 結束就消失. 存在 `sessionStorage` 中的資料在關閉頁面時就消失了.
  * Ref: [HTML5 Local Storage](http://www.w3schools.com/html/html5_webstorage.asp), [Local Storage vs Cookies](http://stackoverflow.com/questions/3220660/local-storage-vs-cookies).
* 解釋 `<script>`, `<script async>` & `<script defer>` 有什麼不同.
  * 答:
    * `<script>` tag 是用來定義一個 client-side script, 例如一份 JavaScript. 它可能直接內含 script 程式, 或者從特定網址下載遠端的 script 檔.
    * `<script async>` (或 `<script async="async">` for xhtml): 它指定一份 script 要被非同步地執行 (一旦下載完成). 這一份 script 不會 block 住頁面的 parsing, 在這一份 script 被下載及執行的同時, 頁面仍然會繼續進行 parse
    * `<script defer>` (or `<script defer="defer">` for xhtml): 它指定一份 script 要在頁面的 parsing 完成後才執行.
    * 注意： async 和 defer 只對載入外部 script 的情形生效.
  * Ref: [HTML script Tag](http://www.w3schools.com/tags/tag_script.asp), [HTML script async Attribute](http://www.w3schools.com/tags/att_script_async.asp), [HTML script defer Attribute](http://www.w3schools.com/tags/att_script_defer.asp).
* 為何將 CSS `<link>`s 放在 `<head></head>` 中及將 JS `<script>`s 放在 `</body>` 之前較好? 有任何例外嗎?
  * 答 (簡短版): 簡言之, 因為 CSS 通常包含如何呈現內容 (style) 的資訊, 瀏覽器需要它以 render 大部份的頁面內容, 而 JS 則否
  * 答 (for link):
    * [HTML spec](http://www.w3.org/TR/html401/struct/links.html#edef-LINK) 說道, link 在 HTML 4.1 只能放在 head 中, 而在 HTML5 link 可放在 body 但要有 "itemprop" 屬性.
    * 假如某些 style 放在 body 中, 當瀏覽器 parse 到時可能會需要重新 render 目前的頁面
    * 把 link 放在 head 可以幫助你避免 FOUC.
  * 答 (for script):
    * script 若未設置 `async` 或 `defer` 會 block 住頁面 parsing 的動作, 若放在 header 和/或分散在 body 中可能會增加 network roundtrip 並延後頁面開始 render 的時間 (Start Render Time).
  * 例外 (for link):
    * 一個與 RWD 相關的例子是, 假設你以 media query 指定了多個不同狀況下使用的 CSS 檔, 瀏覽器仍然會將它們全部下載, 假若你真的想只下載需要的檔案就需要搭配一些 JS 的技巧.
    * Samples:
      * 一般版 media query: [Sample](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/HTML%20Questions/CSS%20media%20query%20general/media_query_test.html), [Sources](https://github.com/benbai123/benbai123.github.io/tree/master/examples/Front-end-Developer-Interview-Questions/HTML%20Questions/CSS%20media%20query%20general)
      * 特別版 media query: [Sample](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/HTML%20Questions/CSS%20media%20query%20tricky/media_query_test.html), [Sources](https://github.com/benbai123/benbai123.github.io/tree/master/examples/Front-end-Developer-Interview-Questions/HTML%20Questions/CSS%20media%20query%20tricky)
        * 先以 media query 生成相關連結. (見 base.css)
        * 再靠 JS 載入檔案. (見 HTML 檔)
      * 注意: 特別版可能不是好做法, 但重點也不在那.
  * 例外 (for script): 對 script 來說有幾種例外
    * 有些 JS 套件就是規訂要放前面 (如 Modernizr.js).
    * 對於有指定 `async` 或 `defer` 的 script, 放在前面不會 block parser 並且可提早下載它們.
    * 假若頁面主要內容要靠 script 產生, 如 Google Map (且它可支援 async load)
    * 假如 style 會被這個 script 並造成大量重繪 (如上述 Modernizr) 那可能放前面會比較好
    * 假如需要靠 script 來 render 內容, 如你的內容是一串數列而需要靠 script 生成圖片, 那可能可以把 script 放 header 之後但在 footer 之前, 而不必放到最後面.
    * 簡言之, 當你無法決定放哪, 或當 script 可 async/defer, 以及當 script 對 render 內容很重要時就可以往前放
  * Ref:
    * For CSS: [What's the difference if I put css file inside head or body?](http://stackoverflow.com/questions/1642212/whats-the-difference-if-i-put-css-file-inside-head-or-body), [load external css file in body tag [duplicate]](http://stackoverflow.com/questions/4957446/load-external-css-file-in-body-tag)
    * For JS: [Where is the best place to put script tags in HTML markup?](http://stackoverflow.com/questions/436411/where-is-the-best-place-to-put-script-tags-in-html-markup), [Remove Render-Blocking JavaScript](https://developers.google.com/speed/docs/insights/BlockingJS).
* 什麼是 progressive rendering (漸進式渲染)?
  * 答: 主要的概念是盡快呈現可見的內容, 但一開始時內容可能品質較低或不完整, 之後再載入更多資料提升品質或補完內容, 一些例子
    * 目前的瀏覽器就是以這種方式呈現頁面, 不會等所有內容都下載完畢才一次呈現
    * 或者像 Google 圖片搜尋, 點擊時會先秀低解析度的圖再載入高清圖檔
  * Ref: [What is progressive rendering?](https://coronarenderer.freshdesk.com/support/solutions/articles/5000516260-what-is-progressive-rendering-), [The Lost Art of Progressive HTML Rendering](http://blog.codinghorror.com/the-lost-art-of-progressive-html-rendering/)
* 你用過哪些 HTML templating languages?
  * 答: JSP, JSF, ZK, Struts, node + express, Rails, 也試過 AngularJS 的 template
  * 實際上我不確定這裡的 HTML templating languages 是指什麼, 如果是指一些純 client 端的東西那只有 AngularJS 一項.
