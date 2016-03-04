
#### 效能問題集:

* 你會用什麼工具來檢查你的頁面慢在哪裡?
  * 答: Chrome 的開發者工具 (profiler 為主)
  * 參考: [Profiling JavaScript Performance](https://developer.chrome.com/devtools/docs/cpu-profiling)
* 有哪些做法可以改進 scrolling 的效能?
  * 答:
    * 首先談到有幾個因素會使  scrolling 的效能 變差:
      * 頁面上有太多的 Dom 元素
      * 在 scroll 事件時處理太多事情
      * 使用到效能較差 (需要做較多工作) 的 CSS 屬性
    * 對應於以上幾點因素, 可以如下改進
      * 為了減少頁面上的元素, 可以使用 Render/Load on Demand (依需要 render / 載入), 或調整頁面結構
      * 利用 throttle/debounce 機制減少 scroll 事件時做的事情.
      * 調整 CSS 或頁面結構以降低 Repaint 的負擔, 例如將背景移到獨立的一層.
* 解釋 layout, painting 和 compositing 有何不同.
  * 答:
    * layout: 當每個元素的 rule/style 確定後, 計算它們的位置跟大小
    * painting: 以 pixels 將元素各個部份畫出來, 如畫出文字、圖片、顏色、border 及 shadow 等, 通常會畫出多個圖層 (layer)
    * compositing: 將前述多個圖層以正確的順序堆疊, 正確的顯示畫面
  * 參考: [Rendering performance](https://developers.google.com/web/fundamentals/performance/rendering/?hl=en)