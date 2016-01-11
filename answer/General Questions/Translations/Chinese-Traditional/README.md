#### <a name='general-questions'>常見問題：</a>

* 你昨天或這週學習了什麼？
  * 答: 使用 JDBC/MySQL 做分頁及多層排序. (無關前端科科)
* 寫程式的哪部份最讓你感到很興奮或是有興趣？
  * 答: 發現更新更好的事物、改進某些現有的東西及解決難題
* 你最近遇到的一項技術上的挑戰是什麼? 如何解決?
  * 答: 最近在做自動化測試時在 Rake 及 Minitest 的 life-cycle 問題上卡了很久, 後來是爬 minitest 的原碼後改寫它的 reporter 來額外做一些自己需要的事情來解決
* 當你開發Web應用程式或網站時，針對UI、安全性、效能、SEO、維護性，以及技術，你考量的點是什麼？
  * 答:
    * UI, 盡可能維持 Dom 結構單純且架構清楚, 強化可重用性 (含抽象元件及實際 Dom 元素), 避免 memory leak.
    * 安全性, XSS - 視需要將要輸出的內容編碼. CSRF - 前後端間建立一個認證方法, 例如溝通時帶一個額外的 token. 另外盡量使用 SSL 加密連線.
    * 效能, 減少 Dom 元素及要執行的 JS code 的量, 實做 "依需要載入" 和/或 "依需要呈現" 的元件, 使用 throttle/debounce 方法 (也是減少 JS 執行), 定義變數在適當的 Scope (通常越近越好, 盡量避免全域變數).
    * SEO, 適當的使用 html tag (title, meta-description, a 等), 如果有必要就做後端渲染.
    * 維護性, 元件化, 適當的命名, 使用 CSS 前處理, JS 模組化, 適當且有邏輯的檔案結構, 將 HTML 拆解為可重用/組裝的片段, 使用 Template Engine, 使用前端 Framework (Angular, Backbone, etc), 諸如此類
      * (小聲) 嘴砲不用錢科科
* 說說你喜好的開發環境 (作業系統, 編輯器或 IDE, 瀏覽器, 開發工具 … 之類)
  * 答:
    * 作業系統: Windows or Ubuntu
    * 編輯器或 IDE, : Eclipse, Sublime text
    * 版控: Git, SVN
    * 其它: Jenkins, Selenium, Docker, Bash/Shell
    * (OS) 瀏覽器 可以挑我喜歡的就好了, 不過通常要客戶喜歡...
* 你最熟悉哪一套版本控制系統？
  * 答: Git, SVN
* 你可以描述你在開發一個網站時的工作流程嗎？
  * 答: 我會從靜態頁面開始, 若有某些東西在多處用到就做成元件, 在這個過程中若有需要就跟後端討論 API 怎麼串.
* 如果有 5 種不同的樣式表 (stylesheets)，該如何整併到網站？
  * 答:
    * 如果是指有五個 CSS 檔要同時一起使用, 我會單純的用工具合併與 minify 它們.
    * 如果是五個不同的 theme, 我會給每個 theme 不同的名稱，然後用 LESS 等前處理器支援的巢狀表示方式將它們各自包在自己所屬的名稱之內, 之後改 body 的 class 就可以套用不同的 theme.
    * 對本題的 "5 種不同的樣式表" 及 "整併" 不是很肯定是什麼意思, 回答前應該要問清楚.
* 你可以描述漸進增強 (progressive enhancement) 和優美退化 (graceful degradation) 間的差異嗎？
  * 答:
    * progressive enhancement: 從一張靜態地圖開始到做到 Google Map 那樣
    * graceful degradation: 先做到 Google Map 那樣不行的就給它一張靜態地圖
    * progressive enhancement: 一開始先讓所有的環境都能跑破破的沒關係, 再針對較強的瀏覽器做優化
    * graceful degradation: 一開始先在最強的瀏覽器做出最威最炫的版本再針對老弱瀏覽器做閹割版本
    * 更多資訊請參考 [w3 wiki](https://www.w3.org/wiki/Graceful_degradation_versus_progressive_enhancement)
* 你怎麼優化一個網站的靜態檔案 (assets) 和資源 (resources)？
  * 答: 有很~多很多點勒
    * 減少 http request 的次數: 具體做法如用工具將多個 JS/CSS 檔案合併為一個 JS/CSS 檔, 將多個小 icons 合併為一張大圖 (CSS sprite), 盡量不使用 iframe 也盡量不要 redirect
    * 降低檔案大小: minify js 和 css, 壓縮圖檔等
    * 降低/優化網路流量: 使用 gzip, 依需要載入 (e.g. 例如 scroll 到一張圖片的位置時才真的載入它), 載入完成後的預載入 (Post-load Pre-loading, 例如 Google 首頁只要極少的資源載入很快, 而首頁載入完成後它會自動先載入搜尋結果頁所需要的資源), 等等
    * 使用 CDN 服務達到高可用及高效能
    * 將靜態資源分散放在數個不同的 domain 以提高併行下載數量
    * 但是也不要用太多個不同的 domain 以減少 DNS lookup 時間, 普遍來說以一個 domain 放 HTML 頁面再加上另外兩個 domain 放資源檔為宜
    * 放 assets 的 domain 不要有 cookie 以減低 request header 的大小
    * 使用 Expires Header 讓瀏覽器 cache 靜態資源
    * 將 assets 做版本控管.
    如使用額外參數 `src="assets/logo.png?v=v2"`
    或使用不同路徑 `src="assets/v2/logo.png"`
    或直接改檔名 `src="assets/logo2.png"` (個人覺得前兩個較優)
    * 將 CSS 在 head 中載入
    * 盡量將 script 放最後 (在 `</body>` 之前)
    * 參閱 [Web Site Optimization: 13 Simple Steps Article](http://www.sitepoint.com/web-site-optimization-steps/) 了解詳情
    * DNS Lookup 細節補充 [DNS Lookup](https://developer.yahoo.com/blogs/ydn/high-performance-sites-rule-9-reduce-dns-lookups-7207.html)
* 瀏覽器在一個 domain 會有多少併行下載?
  * 有何例外?
    * 答: 基本上各家各版本不同, 只要掌握上題說過的原則 (一個 domain 放 HTML 頁面再加上另外兩個 domain 放資源檔) 即可, 參見 [http://stackoverflow.com/questions/7456325/get-number-of-concurrent-requests-by-browser](http://stackoverflow.com/questions/7456325/get-number-of-concurrent-requests-by-browser)
* 說出三種能加快網頁讀取速度的方法 (感覺上的速度或是真正的讀取時間)。
  * 答: CSS Sprite, 合併/最小化 JS/CSS files, gzip 內容, 依需要載入, post-load preloading, 將 JS 盡量後移.
* 如果你加入了一個專案，但是他們的程式碼用 tabs，但是你習慣用spaces (空白鍵)，你會怎麼做？
  * 答: 我可能會也改用 tab, 或仍然使用 space 並依需要用工具自動在 space/tab 之間轉換
* 寫一個簡易的投影片頁面
  * 答: 兩種做法, 以 javascript 控制 scrollLeft (scrollTop) 或用 CSS3 animation 搭配預先定義的 frame (若需要控制純 CSS 的 case 可使用 radio button 搭配 selector)
    * JavaScript 版本: [Sample Page](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/General%20Questions/slider_js.html) Sources: [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/General%20Questions/slider_js.html), [JS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/General%20Questions/js/slider_js.js), [CSS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/General%20Questions/css/slider_js.css)
    * 純 CSS 版本: [Sample Page](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/General%20Questions/slider_css.html) Sources: [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/General%20Questions/slider_css.html), [CSS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/General%20Questions/css/slider_css.css)
* 你用什麼工具來測試你的程式碼效能？
  * 答: 啊勒英文版沒看到這題? 用 Chrome 開發者工具來看, 或線上 JS 效能測試等
* 如果今年你能精通一項技術，那會是什麼？
  * 答: 可能是 JDBC 或者 DevOps (Spark 也想碰一下)
* 描述標準和製定標準機構的重要性？
  * 答:
    * 標準: 讓所有 browser vendor 及前端工程師能遵從一些基本共試, 珍惜生命, 請照標準走.
    * 製定標準機構: 有它標準才生得出來. 
* 什麼是 FOUC？ 你怎麼避免 FOUC?
  * 答: FOUC 就是瀏覽器呈現沒有套到 style 的內容的狀況,
    * 一些可能的原因:
      * 在 IE5 以上的版本使用 `@import` 載入 style
      * 使用 JavaScript 生成內容並套 style 時沒有處理好
      * Style 及圖檔沒有搭配好, 例如一個元素 hover 時會換背景圖但這背景圖是獨立的另一張圖片, 則在圖片載入期間會呈現無背景圖的狀態.
    * 一些可能的解法:
      * 在 head 中加入 `link` 或 `script` 可以避免上述第一點
      * 以 JS/CSS 控制一開始將內容全部隱藏, 在生好所有內容、套好所有 style、載入所有檔案後才顯示 
      * 對上述第三點, 要將各狀態的背景圖合成一張, 以 `background-position` 控制
    * 更多資訊請見: [wiki](https://en.wikipedia.org/wiki/Flash_of_unstyled_content), [question at SO](http://stackoverflow.com/questions/11640238/how-to-stop-flash-of-unstyled-content)
* 解釋什麼是 ARIA 及 screenreaders, 及如何建立無障礙網站.
  * 答:
    * ARIA: WAI-ARIA, the Accessible Rich Internet Applications Suite (無障礙豐富網際網路應用程式), 定義了網站內容及應用如何讓有視聽障礙的人士更容易使用.
    * screenreaders: 螢幕閱讀器. 又稱為螢幕報讀軟體，是一種安裝於電腦上的應用程式軟體，用來將文字、圖形以及電腦介面的其他部分（藉文字轉語音（Text-To-Speech, TTS）技術）轉換成語音及／或點字。對於視障者或閱讀障礙者甚有助益，有些人會搭配放大軟體一齊使用。 
    * 如何建立無障礙網站
      * 圖片 & 動畫: 使用 alt 屬性以文字做說明.
      * Image maps. 使用 client-side map/area 元素並使用 alt 屬性做說明.
      * Multimedia. 對音訊部份提供文字內容, 視訊部份提供說明.
      * 超連結. 使用閱讀時能提供較豐富資訊的文字. 例如不要使用 "點這裡"
      * 頁面結構. 使用大標題、清單及有邏輯且一致的結構, 使用 CSS 做 layout 及調 style.
      * 圖表. 以文字做簡單說明或使用 `longdesc` 屬性連結到有詳細說明的頁面
      * Scripts, applets, & plug-ins. 提供當互動功能無法運作時的替代內容.
      * Frames. 使用 `noframes` 元素提供文字說明, 使用有意義的 title
      * Tables. 確保一行一行讀下來內容清楚易懂
      * Check your work. Validate. Use tools, checklist, and guidelines at http://www.w3.org/TR/WCAG
    * Ref: [Screen Reader Wiki](https://en.wikipedia.org/wiki/Screen_reader), [ARIA at w3](http://www.w3.org/WAI/intro/aria), [How to make a website accessible at w3](http://www.w3.org/WAI/quicktips/)
* 說明 CSS 動畫及 JS 動畫的優缺點.
  * 答:
    * CSS 動畫: 優 - 對單純動畫及 3D Transform 較易撰寫, 缺 - 需要較複雜的流程控制時不易達成,
    * JS 動畫: 優 - 極佳的彈性, 能較好的控制複雜的動畫, 豐富的互動性, 支援較老舊的瀏覽器, 缺 - 撰寫 JS 通常較為複雜
    * Ref: [Myth Busting: CSS Animations vs. JavaScript](https://css-tricks.com/myth-busting-css-animations-vs-javascript/)
* 什麼是 CORS? 主要用來處理什麼問題?
  * Ans: 
    * CORS: Cross-origin resource sharing (跨域資源共享), 是一個機制, 允許一個網頁可以存取另一個 domain 上的特定資源 (如字型).
    * CORS 定義了瀏覽器及伺服器間如何介定是否允許跨域請求的方式, 它比只允許 same-origin requests 有彈性, 但又比完全允許所有的 cross-origin requests 來得安全, 它是 W3C 建議的標準
    * 例, Public data API 的提供痕可允許其它站台以跨網域 AJAX 向它要求資料.
    * Ref: [CORS Wiki](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)