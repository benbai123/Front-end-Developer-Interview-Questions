#### <a name='css-questions'>CSS 問題：</a>

* CSS 的 class 和 ID 兩者有何差異？
  * 答:
    * ID 在整個頁面上應該是唯一的, 不同的 Dom 元素不能有相同的 ID, class 則可用在複數個元素上.
    * ID 能當成 "書籤" (url#hash 即會在連到 url 後將視點捲動到 id="hash" 的元素上) [Sample](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/CSS%20Questions/id_anchor_test.html#top), [Source](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/CSS%20Questions/id_anchor_test.html)
* 描述 "resetting" 和 "normalizing" 的差異性？你會選擇哪一種，為什麼選擇它？
  * 答:
    * "resetting" 是移除所有瀏覽器預設的 style, "normalizing" 則是要建立一組在各瀏覽器上看來都一致的 style
    * 我會選擇 "normalizing", 它讓你在每次建立一個新網站時要做的事更少. 也提供比較能看的初始 style.
* 描述 Floats 並解釋如何運作。
  * 答 (簡短版): Float 將一個元素從頁面 flow 中移出, 然後依指定的值將它推往 左/右 側, 所有沒有被指定為 float 的 siblings 會圍繞著 float element, 通常搭配 clear 屬性使用來避免破版
  * 答:
    * Float 屬性依指定的 left/right 值將 Dom 元素 "推" 往左側或右側, 然後其它元素會使用餘下的空間並圍繞著 Float 的元素, 注意苔一個區塊中所有的元素都是 Float 的話它將會沒有高度, 通常可以
      * 加入一個使用 clear 屬性的元素來解決 (如 `<div style="clear: both;"></div>`).
      * 若一個區塊本身有 `overflow: auto;` 或 `overflow: hidden`, 則會自動擴展到正確的 size 而不需要額外的 clear 元素.
      * 也可以對該區塊使用 pseudo selector 插入含 clear 修正的元素如下:
      ```css
      /* 假設該區塊的 class 包含 clearfix */
      .clearfix:after { 
         content: "."; 
         visibility: hidden; 
         display: block; 
         height: 0; 
         clear: both;
      }
      ```
  * Ref: [All About Floats](https://css-tricks.com/all-about-floats/)
* 描述 z-index 並且描述堆疊內容 (stacking context) 如何形成。
  * Ans:
    * z-index
      * z-index 屬性是用來指示元素堆疊的順序
      * 注意: z-index 只對被定位的元素起作用 (position:absolute, position:relative, or position:fixed).
      * 一個元素永遠在它的 parent 元素之上, z-index 無法改變它們的順序
    * 描述堆疊內容 (stacking context) 如何形成
      * 一個元素永遠在它的 parent 元素之上, z-index 無法改變它們的順序
      * 後出現的元素有較高的堆疊順序 (比 previous sibling 們高), z-index 可以改變它們的順序.
      * z-index 值越高的元素有越高的堆疊順序.
  * 一些研究用的範例: [Sample](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/CSS%20Questions/stacking_context_test.html), [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/CSS%20Questions/stacking_context_test.html), [CSS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/CSS%20Questions/css/stacking_context_test.css)
* 說明什麼是 BFC(Block Formatting Context) 及它如何運作.
  * 答:
    * 什麼是 BFC: 它是關於 CSS 如何視覺化的呈現頁面的一個部份, 算是一個規格, 它同時也指一個 "符何可以套用這個規格" 的條件的元素
    * 它如何運作
      * BFC 下的內容 (boxes) 會被一個接著一個、從頂端由上往下排列放置
      * 上下兩塊內容 (boxes) 間的距離由 margin 屬性定義, 兩塊上下相鄰的內容的 margin 會被合併 (若沒有一塊 box 成為一塊新的 BFC 的話)
      * 在 BFC 下每一塊 box 的左側外緣緊貼 BFC 的左側內緣 (在 `dir="rtl"` 時則是右側), 即使 BFC 下的 box 為 float 亦同
      * BFC 下使用 Float 元素可以免 clear fix
      * BFC 中的文字不會圍繞 Float 元素
      * BFC 可協助避免預期之外的 size 偏差造成的斷行
  * Sample: [Testing page](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/CSS%20Questions/block_formatting_contexts.html), [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/CSS%20Questions/block_formatting_contexts.html), [CSS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/CSS%20Questions/css/block_formatting_contexts.css)
  * Ref: [Block formatting contexts](http://www.w3.org/TR/CSS21/visuren.html#block-formatting), [Understanding Block Formatting Contexts in CSS](http://www.sitepoint.com/understanding-block-formatting-contexts-in-css/)
* 有哪些不同的 clearing 技術？哪個適用在哪種內容上？
  * 答:
    * 有哪些不同的 clearing 技術？
      * 多加入一個元素並給它 `clear: both;` 的 style.
      * 使用 BFC (以 `overflow: hidden;` 屬性造成 BFC 是個好選擇) 也可讓 Float 元素運作正常.
      * 使用 pseudo element (`::after`) 並給 style `clear: both:`.
    * 哪個適用在哪種內容上？
      * 第一個適用在各種內容, 但若非常介意語意無關的額外元素則不適用
      * 第二種適用在大部份內容, 可避免額外元素, 可支援到 IE6, 但可能無法總是讓父元素成為 BFC
      * 第三種適用在各種內容, 但只支援 IE8+
* 描述 CSS sprites, 你如何實作在網頁或網站上？
  * 答:
    * 描述 CSS sprites: CSS sprites 指將多張較小圖片合併成一張大圖, 多個元素共同那同一張大圖, 然後以 CSS 控制要顯示哪個部份, 以減少 HTTP request 次數.
    * 你如何實作在網頁或網站上
      * 我會盡量把小 icon 集中成一張然後放在 CDN 伺服器, 若有實做一組預計在多個網站使用的元件, 則會考慮以元件或模組為單位做 CSS Sprite
      * 對一般多頁網站, 我會傾向以頁面為單位製作 CSS Sprite, 對 SPA (Single Page App) 則傾向做一份整站的 CSS Sprite
* 你最喜愛的圖片取代技術是什麼？你什麼時候會用到？
  * 答:
    * 我最喜歡的做法是以絕對定位將圖片蓋在文字上.
    * 通常用在 SEO 優化, 例如為了 SEO 需要以文字寫入公司名但你想秀的是 Logo 圖片
  * Ref: [Nine Techniques for CSS Image Replacement](https://css-tricks.com/css-image-replacement/)
* 你如何修正只發生在特定瀏覽器的 style 問題? CSS 屬性 hacks, 也條件引用 .css 檔案, 或是… 其他的？
  * 答:
    * 首先我會嚐試找到一個跨瀏覽器的做法.
    * 如果找不到上述方法但有可靠的 CSS hacks 就使用 CSS hacks
    * 有可能該 browser 需要不同的 style 而會影響到其它 browser, 則針對該 browser 引用其它 CSS 檔.
* 你怎麼讓你的網頁支援有功能限制的瀏覽器？
  * 你會使用什麼樣的技術/流程 ？
  * 答: 首先試著找出解法或自己實做 (如補上缺的部份, 像 [CSS3 PIE](http://css3pie.com/) 或 [excanvas](https://github.com/arv/explorercanvas) 會幫忙讓較舊的 IE 支援 CSS3 與 Canvas), 或針對該特定瀏覽器做較弱的版本再漸進增強 (progressive enhancement).
  * 你會使用什麼樣的技術/流程 ？
    * 答: 
      * 我用的流程是漸進增強
      * 技術則有多種
        * 估狗技巧, 主要用來找到現有解法或可用 lib
        * JS/CSS/HTML 技能, 有些舊瀏覽器不支援的東西 (如 textarea resize) 可以靠自己做出來 (拼了!)
        * 溝通技巧：真的不行就溝通 "請接受弱化版本" 吧
* 有什麼方法來隱藏網頁的內容？ (只顯示在 screen readers)？
  * 答: 以 `text-indent: -10000px;` 或 `position: fixed; left: -10000px;` 控制文字位置, 可能也可以靠控制文字的 透明度/顏色 來讓它無法被看到, 某些做法 (如透明度) 可能也會被 screen readers 略過, 控制位置比較安全.
* 你使用過 grid system 嗎？如果有的話？你較推薦哪個？
  * 答: 我用過 ZK 的 grid 元件和 bootstrap 的 grid system. 我較喜歡 bootstrap 的 grid system, 你只需要知道一些 class 名稱跟簡單的規則就能很容易的控置它. 雖然它將 style 與 html 混在一起但我覺得在可接受的範圍.
  * 更新: 後來發現 Grid System 也與 平面設計 (Graphic Design) 有關, 考慮到這點後 bootstrap 的 Grid System 就更好了, 它讓工程師與設計師之間更加的溝通無礙.
  * Ref: [Grid (graphic design)](https://en.wikipedia.org/wiki/Grid_%28graphic_design%29)
* 你曾經實作 media queries 或是 mobile specific (手機規格的) layouts/CSS?
  * 答: 有. (這是答案嗎?), 對小螢幕常需要調整 圖片/文字/按鈕 的大小, 觸碰控制比滑鼠點擊更為困難.
* 你熟悉任何有關 SVG 嗎？
  * 答: 不熟, 只有在使用如 Morrischart 這種基於 SVG 的繪圖套件時試著調整文字大小這樣的程度.
* 你如何優化你的網頁以利於列印？
  * 答:
    * 關於網站結構
      * 建立一個列印專用的 CSS 檔: `<link rel="stylesheet" type="text/css" href="print.css" media="print"></link>`.
      * 非必要不使用 HTML Tables: 使用 Table 會讓你很難調整你的內容區塊 (for 列印).
      * 檢視各區塊是否值得列印: 例如 banner 或廣告在列印時應隱藏. 建立一個 "不列印" 的 class 然後將它加到所有列印時要隱藏的元素
      ```css
      .no-print   { display:none; }
      ```
      ```html
      <!-- Example -->
      <div id="navigation" class="no-print">
        .... <!-- who needs navigation when you're looking at a printed piece? -->
      </div>
      ```
      * 使用 Page Breaks: page-break-before / page-break-after 屬性很有用. 前提是使用 DIV 而非 table-cells.
      ```css
      .page-break { page-break-before: always; } /* put this class into your main.css file with "display:none;" */
      ```
      ```html
      Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Fusce eu felis. Curabitur sit amet magna. Nullam aliquet. Aliquam ut diam...
      <div class="page-break"></div>
      Lorem ipsum dolor sit amet, consectetuer adipiscing elit....
      ```
      * 調整你頁面的 Size: 將內容區塊設為 600px 寬. 以確保文字不會超出列印範圍.
      * 測試: 使用多種 browser 實際印印看
    * 關於網站內容
      * 調整 Heading Tag: 確保 `<h1>` tag 夠大到看得出來是標題. `<h2>` 加個底線當副標. `<h3>` 比起段落文字應相等或稍大. 以上皆粗體. 並將 padding 設為 0 以 margin 製造間隔以便於控制; 除了 `<h1>` 外其它的都加一些 margin.
      ```css
      h1,h2,h3  { font-weight:bold; }
      h1    { font-size:24px; }
      h2    { font-size:16px; border-bottom:1px solid #ccc; padding:0 0 2px 0; margin:0 0 5px 0; }
      h3    { font-size:13px; margin:0 0 2px 0; }
      ```
      * 調整段落文字: 將 margin-bottom 設得比行高大, 設定字行及 font-size.
      ```css
      p   { font-size:11px; font-family:times new roman, serif; margin:0 0 18px 0; }
      ```
      * 調整超連結文字: 加下底線:
      ```css
      a   { text-decoration:underline; }
      ```
      再加上顯示 URL
      ```css
      a:link:after, a:visited:after { content: " (" attr(href) ") "; font-size: 90%; }
      ```
      * 檢查: 確認這些 style 不會讓你的頁面破版.
  * Ref: [Optimizing Your Website Structure For Print Using CSS](https://davidwalsh.name/optimizing-structure-print-css), [Optimizing Your Website Content For Print Using CSS](https://davidwalsh.name/optimizing-content-print-css)
* 在寫高效的 CSS 時，有什麼要注意的？
  * 答: 靠北多 (也有一點看個人)
    * 避免會降低效能的屬性: 可能有多種做法能達成相同效果, 但效能各不相同, 如,
    將以下
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
    換成
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
    * 減少瀏覽器重排 (Reflow): 
      * 減少非必要過深層的 Dom 結構 (以減低重繪一個區塊的 Loading).
      * 減少 CSS 規則並移除未用到的規則 (以減低不必要的 selector matching).
      * 複雜的呈現改變如動畫等到 flow 之外做. (使用 `position: absolute;` 或 `position: fixed;`).
      * 避免太複雜的 CSS selectors - 如過長的 chaining selectors - 以降低 CPU 做 selector matching 的負擔.
    * 使用 LESS 等前處理工具使 CSS Code 更結構化.
    * 不使用 ID, 使用 tag (`h1, p, ...`) 設置通用預設 style, 使用 Class 更進一步修飾 (個人偏好)
    * 對頁面以區塊的角色來定 selector, 如 `.header .slogan`, `.sidebar .menu`, etc.
    * 對到處使用的元件則定 "component based" 的 CSS, 這提昇維護性跟重用性, 降低 file size (因為不必對不同頁各自寫 style), 避免過深的 selectors (只從 Component root 開始) 並減少未用到規則的發生機率. 如,
    ```css
    .chosenbox {...}
    .chosenbox-sel-item {...}
    .chosenbox-del-btn {...}
    ```
    * 對一些通用樣式特別寫一個 class 方便套用. 如,
    ```css
    .inline-block {
      display: inline-block;
      /* IE hack */
      *display: inline;
      zoom: 1;
    }
    ```
    * Ref: [CSS performance revisited: selectors, bloat and expensive styles](http://benfrain.com/css-performance-revisited-selectors-bloat-expensive-styles/), [Profiling CSS for fun and profit](http://perfectionkills.com/profiling-css-for-fun-and-profit-optimization-notes/), [Minimizing browser reflow](https://developers.google.com/speed/articles/reflow?hl=en), [How to animate "box-shadow" with silky smooth performance](http://tobiasahlin.com/blog/how-to-animate-box-shadow/)
* 使用 CSS preprocessors 的優點和缺點是什麼？ (Sass, Compass, Stylus, LESS)
  * 答:
    * 優點
      * 可撰寫巢狀 CSS 不用一直重覆 父 Selector.
      * SASS 有 extend 功能讓你能撰寫可在多處重用的 style
      * mixin 類似 extend 並支援變數及計算
      * 使用變數讓你能容易地修改一個被用在很多地方的值.
    * disadvantages
      * 巢狀結構讓你很難掌控規則的優先度而不利於維護
      * 假設一個檔案中在多組巢狀結構下都有某個 Selector, 那你只想改某一組的 Selector 時搜尋該 Selector 外還得 (上下捲動) 確認它是哪一組的.
      * 你可能會被巢狀結構引誘一層層寫下去而編出太長的 selector chain
      * extend 可能會編出超大量的 CSS Selector
      * variables, extend 及 mixin 降低維護性. 因為使用的地方和定義的地方可能相距很遠, 較容易不小心改動時影響到預期之外的地方.
      * 簡言之, 若 style 常改 (尤其無邏輯性、不可預測的話) 那可能不要用 variables, extend 和 mixin 較好.
    * Ref: [The problem with CSS pre-processors](http://blog.millermedeiros.com/the-problem-with-css-pre-processors/), [@extend performance #12](https://github.com/sass/sass/issues/12), [LESS/SASS: The Advantages of CSS Preprocessing Explained](http://blog.blakesimpson.co.uk/read/37-less-sass-the-advantages-of-css-preprocessing-explained)
  * 描述你使用過的喜歡和不喜歡的 CSS preprocessors
    * 答: 我相當喜歡 LESS, 因為它可在 Client 端編譯, 這給了它更大的使用彈性如 Client Side 動態改變 Style 等. 沒有特別不喜歡的, 它們差距沒有很大, 若不喜歡某部份只管寫純 CSS 就是.
* 你如何使用非標準字體來實作網頁設計？
  * 答:
    * 我會先找找有沒有合適的雲端字型如 [Google Fonts](https://www.google.com/fonts) 可用.
    * 若無我會先設法找到字型檔 (.ttf, .woff, etc, probably need to convert file manually) 並在 CSS 中使用 `@font-face` 屬性:
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
    * 若上述字型檔格式不齊全可能需要找一些工具進行轉檔
  * Ref: [Using @font-face](https://css-tricks.com/snippets/css/using-font-face/)
* 解釋瀏覽器如何按照 CSS selector 找到對應的 element。
  * 答 (簡短版): 瀏覽器會從最右邊的 Selector 開始比對, 若元素符合最右邊的 Selector 再拿向左一個 的 Selector 往上找是否有符合的元素, 一直到最左邊為止
  * 答 (完整版):
    * 當瀏覽器在做 selector 比對時它有 一個元素 (就那個要比對有沒有 match 到某些 rule 的) 以及一海票規則 (你寫的科科), 然後它要找哪些規則要套用到這個元素. 對一條規則從最右邊一個 Selector 開始比的話有機會立馬就不 match 然後這條就比完了, 若 match 才需要再往上比, 但這時最多就比跟 Dom tree 深度一樣的次數而已通常沒幾次.
    * 假如從最左邊的開始比你得爬整個 Dom Tree, 可能光找到最左側不 Match 的就要花上好些時間.
  * Ref: [Why do browsers match CSS selectors from right to left?](http://stackoverflow.com/questions/5797014/why-do-browsers-match-css-selectors-from-right-to-left)
* 說明 pseudo-elements 及它們的功用. 
  * 答: pseudo-element 一個元素特定部份的樣式, 例如:
    * 調整一個元素中第一個字元或第一行的 Style
    * 在一個元素的內容之前或之後插入內容
  * Ref: [CSS Pseudo-elements](http://www.w3schools.com/css/css_pseudo_elements.asp)
* 解釋你所認知的 box model，以及你如何在 CSS 告訴瀏覽器使用不同 box model 來呈現圖層？
  * 答:
    * 所有的 HTML 元素可以當成 "box" 來看, 在 CSS 中 "box model" 是用來說設計或 layout, 它實際上就是包圍住元素的一個 box, 包含 margin, border, padding 及實際內容
    * CSS 3 有一個 `box-sizing` 屬性用來指示瀏覽器 width 及 height 屬性該包含什麼.
      * `content-box`: 這是預設值. width 跟 height 屬性只包含實際內容. Border, padding, 或 margin 則不在其中, 亦即若有 border 及 padding 則要計算後才知道實際寬/高.
      * `border-box`: width 和 height 屬性包含實際內容、border 和 padding 但不含 margin, 因此你可以任意指定 border 及 padding 而不必擔心它們影響到實際大小
      * 過去似乎有個 `padding-box` 但未出現在 w3school 的教學頁, 可能被移除了.
  * Ref: [CSS Box Model](http://www.w3schools.com/css/css_boxmodel.asp), [CSS3 box-sizing Property](http://www.w3schools.com/cssref/css3_pr_box-sizing.asp), [Box Sizing](https://css-tricks.com/box-sizing/)
* 請解釋 `* { box-sizing: border-box; }`？並且說明使用它的好處？
  * 答:
    * 它告訴瀏覽器 width 及 height 屬情 (還有 min/max 屬性) 包含內容, padding 及 border, 但不含 margin.
    * 它讓你指定 border/padding 時不用擔心會影響版面, RWD (Responsive Web Design) 時很有用.
    * 但注意 CSS 3 only
* 請列出您記憶中 display 屬性的全部值
  * 答: (沒有作弊(心虛 ) `none`, `inline`, `inline-block`, `block`, `table`, `table-cell`, `table-caption`, `flex`
* 請說明 inline 和 inline-block 的差異性？
  * 答:
    * `inline`: 這元素的內部及元素本身都被當成 inline-level box 來處理.
    * `inline-block`: 這元素的內部被當成 block-level box, 元素本身則被當成 inline-level box 來處理
    * Inline elements:
      * 可有 左/右 margin 及 padding, 但無 上/下 margin 及 padding
      * width/height 屬性沒有作用
      * 左右可有其它元素
    * Inline-block elements
      * 可有 上/下/左/右 margin 及 padding
      * width/height 有用
      * 左右可有其它元素
  * Ref: [CSS display Property](http://www.w3schools.com/cssref/pr_class_display.asp), [What is the difference between display: inline and display: inline-block?](http://stackoverflow.com/questions/8969381/what-is-the-difference-between-display-inline-and-display-inline-block)
* 請說明 relative、fixed、absolute 和 static 元件差異性？
  * 答:
    * `position: static;`: 這是所有 HTML 元素的預設樣式, `position: static;` 的元素不受 top, bottom, left, 和 right 屬性的影響, 它並沒有持別調整它的位置, 而是由 page flow 決定它的位置
    * `position: relative;`: 受 top, bottom, left, 和 right 屬性的影響, 會以原本的位置為基準, 再依前述四屬性設定的值做位移, 而其它元素不會受到它移位的影響而改變位置. 通常會用 `position: relative;` 但不帶任何位移的元素當作 `position: absolute;` 元素的定位點
    * `position: fixed;`: 它會以整個 viewport (視窗)為基準來定位, 不論視窗如何捲動它都會留在眼睛所見的同一位置而不會有視覺上的移位, top, bottom, left, 和 right 屬性用來定位它, `position: fixed;` 的元素會被移出頁面 flow 因此它原本的位置不會留下空白
    * `position: absolute;`: 它會以第一個定位點為基準來定位, 所謂定位點是比它上層而 position 屬性又不為 static 的元素, 若沒有這樣的元素它就會以 body 做為定位點, 另外它會受到 scroll 的影響
  * Ref: [CSS Layout - The position Property](http://www.w3schools.com/css/css_positioning.asp)
* CSS 的 'C' 是 Cascading (層疊/串接/共用/級聯) 的 'C'. 一個屬性的樣式是如何被決定的呢 (一些例子)? 你如何善用這套系統?
  * 答:
    * 一個屬性的樣式是如何被決定的
      * 一些例子: [Sample](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/CSS%20Questions/css_priority.html), [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/CSS%20Questions/css_priority.html), [CSS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/CSS%20Questions/css/css_priority.css)
      * ID 的優先度比 Class 高.
      * Class 的優先度比 Tag 高.
      * 較晚出現 (在檔案中寫比較下面) 的規則有較高的優先度
      * 套用到較內層元素的規則有較高的優先度
      * 較明確的規則 (例如指定上層的 selector `.parent .child` 或同時使用多個 selector `.child.special.extra.specific`) 有較高的優先度.
      * Inline (直接寫在 style 屬性) 定義的樣式有較高的優先度
      * `!important` 會大幅提高優先度.
      * 此外有一些預設樣式, 如 `display: block;` for `div`.
    * 你如何善用這套系統?
      * 以較低優先度的方式 (如使用 tag selector 並寫在較上方) 做 reset/normalize, 以便於覆寫樣式.
      * 以較高優先度的方式 (如單一特殊 class 並寫在較下方甚至加 `!important`) 定義常用樣式 (如 `.clear {clear: both !important;}`), 以確保它總是會生效
      * 對不同狀態間的樣式 (如一個按鈕的不同狀態) 以較中等優先度的方式 (如數個不同 class) 定義, 以方便切換
      * 這部份其實我沒什麼概念無法答得很好科科
  * Ref: [Specifics on CSS Specificity](https://css-tricks.com/specifics-on-css-specificity/)
* 你目前有使用哪一套 CSS Framework 在開發環境或產品線上？
  * 答: 我目前使用 bootstrap
  * 如果有，請問是哪一套，並且描述如果改善或提昇 CSS Framework？
    * 答:
      * 我目前使用 bootstrap (再問一遍?? 囧)
      * 有好些點我會改改
        * 它的 menu 預設 click 才展開, 我會改成 hover 就展開
        * 它的 grid system 是靠 float, 不同的 col 有不同高度時不太方便使用, 我想把它們改成 `display: inline-block` 應該比較方便
      * 我剛讀了 [5 reasons NOT to use Twitter Bootstrap](http://www.zingdesign.com/5-reasons-not-to-use-twitter-bootstrap/) 因此可能會試一下其它 Framework (如, [Foundation](http://foundation.zurb.com/) )
* 請問你有使用過 CSS Flexbox 或 Grid specs？
  * 答: 有, 我我大概在 2012 年試著以 Flexbox 做了一個 [ZK](https://www.zkoss.org/) Framework 的[元件](https://github.com/benbai123/flexlayout), 但後來 Flexbox 的規格有改, 可能已無法使用了.
* 如何區分 responsive design 與 adaptive design 有何不同？
  * 答: responsive design 是使用一組有彈性的 styles (或許含一些 media query) 而能適用於多種 screen size, adaptive design 則是根據多種 screen size 去製作多組 styles 再依 screen size 選擇套用哪一組 style
  * Ref: [Adaptive web design (wiki)](https://en.wikipedia.org/wiki/Adaptive_web_design), [What is the difference between responsive vs. adaptive web design?](http://www.techrepublic.com/blog/web-designer/what-is-the-difference-between-responsive-vs-adaptive-web-design/), [Responsive vs. Adaptive Design: What’s the Best Choice for Designers?](https://studio.uxpin.com/blog/responsive-vs-adaptive-design-whats-best-choice-designers/)
* 你曾經使用過 retina graphics？如果有，是在什麼時機以及用了什麼技術？
  * 答:
    * 沒有, 但我知道它是指高解析度 Mac 上的顯示問題 (因為我做弊科科.
    * 我會試兩種做法, 偵測是 retina 時給它大圖, 或使用 SVG.
  * Ref: [I cheat before answer this (and lots of others, actually) question :p](http://www.pro-tekconsulting.com/blog/have-you-ever-worked-with-retina-graphics-if-so-when-and-what-techniques-did-you-use/)
* 何時你會想用 `translate()` (X/Y)? 何時你會想用 *absolute positioning*? 原因是?
  * Ans:
    * 何時你會想用 `translate()` (X/Y)?
      * `translate()` (X/Y) 效能比較好, 因此想要好一點的效能時
      * `translate()` (X/Y) 不必有上層定位點 (不過它的行為比較像 `position: relative;`)
    * 何時你會想用 *absolute positioning*
      * translate()` (X/Y) 會受 padding 影響, 因此若實際上要的是對上一層元素左上角的位移需要做額外計算, 懶得算就用 absolute position 吧
      * 當需要支援較舊瀏覽器時使用 *absolute positioning*
      * 時用 *absolute positioning* 時 `z-index` 才會有作用, 也才不會留一塊空白在那 (不過可以用 `translate()` (X/Y) 並指定 `position: absolute;`, 要仔細測一下就是)
    * 一些範例: [Demo](http://benbai123.github.io/examples/Front-end-Developer-Interview-Questions/CSS%20Questions/absolute_and_translate.html), [HTML](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/CSS%20Questions/absolute_and_translate.html), [CSS](https://github.com/benbai123/benbai123.github.io/blob/master/examples/Front-end-Developer-Interview-Questions/CSS%20Questions/css/absolute_and_translate.css)
  * Ref: [Why Moving Elements With Translate() Is Better Than Pos:abs Top/left](http://www.paulirish.com/2012/why-moving-elements-with-translate-is-better-than-posabs-topleft/), [CSS transform vs position](http://stackoverflow.com/questions/7108941/css-transform-vs-position)