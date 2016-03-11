
#### <a name='network-questions'>網路問題集:</a>

* 傳統上為什麼用多個域名來放置網站資源會比較好？
  * 答: 為了增加併行下載的數量, 因為瀏覽器對一個 domain 有併行下載的數量限制
  * 參考: [Performance Research, Part 4: Maximizing Parallel Downloads in the Carpool Lane](http://yuiblog.com/blog/2007/04/11/performance-research-part-4/)
* 請詳細描述當您在網址列打入網址開始到最後網頁呈現在螢幕前的整個流程。
  * 答: 
    * 首圥檢查 cache, 如果要求的東西在 cache 中且沒過時則跳到 #9
    * 瀏覽器問 server IP 是多少
    * OS 也不知道, 所以做 DNS lookup 再跟瀏覽器說結果
    * 瀏覽器向伺服器開啟一個 TCP 連線, 結束這個回合 (在 HTTPS 時狀況則會更複雜)
    * 瀏覽器透過 TCP 連線發送 HTTP request
    * 瀏覽器收到 HTTP response (然後可能會關閉 TCP 連線)
    * 瀏覽器檢查是否為特別的回應如 3xx (重新導向 302, 或條件式回應如 304 not modified), 401 (要求驗證), 4xx / 5xx (Error, 如 403 forbidden, 404 not found, 500 server error) 等, 這些會需要不同於一般 response (2xx) 的處理
    * 若回應能 cache 就存進 cache
    * (#9 到了) 瀏覽器將回應解碼 (假如被壓縮等編碼過)
    * 瀏覽器判斷該怎麼料理回應內容 (HTML? 圖? 聲音檔?)
    * 瀏覽器顯示回應內容, 或者顯示下載視窗
  * 參考: [what happens when you type in a URL in browser](http://stackoverflow.com/questions/2092527/what-happens-when-you-type-in-a-url-in-browser)
* Long-Polling, Websockets, SSE (Server-Sent Event) 之間有什麼差異？
  * 答: 
    * 連線:
      * Long-Polling: 簡言之就是個 timeout 很長的 AJAX, 發出 request 後會維持住連線, server 有資料回傳時再 response, 之後 client 端要再重新發 request.
      * Websockets: client <-> server. Client 與 Server 之間建立起並維持一個 TCP Connection. 任一端均可將其關閉, 若需要頻繁的雙向溝通用這個效率很好
      * Server-Sent Events: client <- server. Client 取得一個連向 server 的持久連線, 只有 server 可以向 client 傳送資料
    * Cross Domain:
      * Long-Polling: 可以 Cross Domain (搭配 CORS 設定).
      * Websockets: 可以 Cross Domain.
      * Server-Sent Events: 目前基本上不能 Cross Domain
    * 瀏覽器支援度:
      * Long-Polling: 幾乎所有主要瀏覽器都支援.
      * Websockets: 比較現代的瀏覽器都支援, 見 [Can I Use WebSocket](http://caniuse.com/#feat=websockets).
      * Server-Sent Events: IE/Edge 之外較現代的瀏覽器都支援 [Can I Use SSE](http://caniuse.com/#feat=eventsource).
  * 參考:
    * [HTML5 WebSocket vs Long Polling vs AJAX vs WebRTC vs Server-Sent Events](http://stackoverflow.com/questions/10028770/html5-websocket-vs-long-polling-vs-ajax-vs-webrtc-vs-server-sent-events)
    * [Server-Sent Events at w3.org](https://www.w3.org/TR/2011/WD-eventsource-20110208/#iana-considerations)
    * [Web sockets make ajax/CORS obsolete?](http://stackoverflow.com/questions/4042691/web-sockets-make-ajax-cors-obsolete)
* 請描述下列 request 和 response headers：
  * Questions
    * Diff. between Expires, Date, Age and If-Modified-...
    * Do Not Track
    * Cache-Control
    * Transfer-Encoding
    * ETag
    * X-Frame-Options
  * 答: First of all, I am soooo glad there is a wiki listing all headers...
    * Diff. between Expires, Date, Age and If-Modified-... 
      * 由文件:
        * Expires - 給定一個時間, 在這個時間之前可以直接從 cache 中取用這個 response 的內容, 之後則需要重新發出 request 取得新的內容
        * Date - 這個 response 發出的時間
        * Age - 一份內容已經在 proxy cache 裡放多久了, 其值為原伺服器的估計值
        * If-Modified-Since - 詢問某 request 的內容是否有修改過, 若無則回傳 304 Not Modified 而不再傳送內容
        * 另外, If-Modified-Since 是 Request Header, 其它的則是 Response Header
      * 白話:
        * Expires - 跟你說何時該重抓新資料.
        * Date - 跟你說資料什麼時候生出來的
        * Age - 跟你說資料被 cache 多久了
        * If-Modified-Since - 要求 server 如果資料沒變就告訴你沒變就好.
    * Do Not Track: 告知 Web App 不要追踨使用者.
    * Cache-Control: Response header. 告知所有從 server 到 client 間的 cache 機制是否要 cache 某份資料, 時限單位為秒.
    * Transfer-Encoding: Response header. 某份資料的編碼格式, 有 chunked (切過的), compress (壓過的), [deflate](https://en.wikipedia.org/wiki/DEFLATE) (緊縮, 也是一種壓縮), gzip (gzip 過, 還是一種壓縮), identity (認證, 謎一般的東西, [官方規格](https://www.w3.org/Protocols/rfc2616/rfc2616-sec3.html#sec3.6) 宣稱定義在 3.6.2, 但似乎消失在歷史的洪流中了 QQ)
    * ETag: Response header. 一份資料的 ID, 通常是 message digest (如 MD5) 的字串
    * X-Frame-Options: Response header. 是否能顯示在 iframe 中的設置, 用以避免 Clickjacking 的漏洞, 其值有 deny - 不行, sameorigin - 只允許同 origin, allow-from - 特定 location 的可以, allowall - 非標準, 完全開放允許.
  * 參考:
    * [List of HTTP header fields](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields)
    * [Hypertext Transfer Protocol (HTTP/1.1): Semantics and Content](https://tools.ietf.org/html/rfc7231)
* 請描述 `GET` 和 `POST` 的差異性？
  * 答: 英文版原題是列出所有 action, 這裡先依英文版答 (當然是作弊的科科)
    * GET: 要求呈現特定資料, 一個 GET Request 只能取得資料, 不應該有其它效果
    * HEAD: 如同 GET 但只要 Response Header (不要 Body), 多用於取得 meta data.
    * POST: 提交資料 (表單或檔案上傳等) 請 server 處理, 可能會建立新資料或修改現有資料
    * PUT: 向 server 上傳 (若不存在則新增, 若已存在則修改) 特定資料.
    * DELETE: 要求刪除特定資料.
    * TRACE: 請 server 將所收到的 request 再回回來, 用以確認途中會被如何修改
    * OPTIONS: 用以取得某 URI 支援的所有方法, 若以 * 取代特定 URI 則是確認 server 是否運作中
    * CONNECT: 可將要求的連線轉為 TCP/IP tunnel, 通常用在經由未加密的 HTTP proxy 建立 HTTPS 連線
    * PATCH: 用於對特定資料做侷部修改.
  * 答: 再回中文原題
    * GET 的 query string (name/value pairs) 會在 URL 中如 `/test/demo_form.asp?name1=value1&name2=value2`, POST 則不會
    * GET 可以被 cache, POST 則否
    * GET 會在 browser history 中, POST 則否
    * GET 可被加為書籤, POST 則否
    * GET 有資料長度的限制, POST 則否
  * 參考:
    * [Hypertext Transfer Protocol - ](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods)
    * [HTTP Methods: GET vs. POST](http://www.w3schools.com/tags/ref_httpmethods.asp)