
#### 測試相關問題:

* 測試你的 code 有什麼優缺點??
  * 答:
    * 優
      * 逼你寫可測試的程式 (通常比較乾淨).
      * 不小心搞爛什麼時可以很快發現
      * Unit test 可以幫忙你 debug
    * 缺
      * 要花額外時間, 尤其是你常重構的話
* 你會用什麼工具來測試你的功能?
  * 答: 我會用 selenium.
* 單元測試 (unit test), 功能測試 (functional test) 及 整合測試 (integration test) 有何不同?
  * 答 (由描述開始):
    * Unit Tests:
      * 測試最小單位的功能, 通常是一個方法 (如某狀態的某個 class 呼叫該 class 的 x 方法後應該有何結果).
      * 應該將焦點放在特定功能 (如一個空 array 呼叫 pop 方法應回傳 undefined).
      * 應該盡可能環境無關, 盡量在 memory (當下獨立執行環境) 完成, 而不能:
        * 使用不夠直觀/單純的其它物件 (如要透過別的物件做複雜的格式轉換或計算) -> 改用一組預先算好的資料
        * 透過網路存取資料 -> 改為餵固定資料
        * 諸如此類
      * 若有任何相依的東西很 慢 難以理解/設置/維護, 則應使用 stubbed/mocked/etc 以便能專注在這 unit 本身, 而不受相依的其它東西干擾.
      * 簡言之, unit tests 應該盡可能簡單, 易於除錯, 可靠 (因為外部變動因素少), 執行快且能證明最小範圍的 function 能正確運作.
    * Functional Tests
      * Functional tests 藉由比對執行結果判斷一個功能是否正確.
      * Functional tests 不考慮執行過程或是否造成 side effect, 只看執行結果.
      * 它是用來測像 "Math.abs(-2) 要回傳 2" 這樣特定的規格.
    * Integration Tests
      * Integration tests 結合各個 unit, function 甚至是多個系統來測試它們共同運作的情形.
      * Integration tests 可以 (也應該) 盡量環境相關, 使用實際的物件、透過網路存取資料等等以確認所有程式及環境都可正確運作
        * 例如防火牆或 cross domain 問題等只有真的連網測試才會測到
      * 優點在它能找到協同運作或環境差異造成的問題.
      * 缺點則是較不穩定且不易除錯 (因為造成錯誤的可能性較多).
    * 總前
      * Unit Tests 測試的範圍最小, 也需要盡可能小才能讓測試 code 少且快速. Functional Tests 則稍大到一個可用的功能 API 等. Integration Tests 的範圍則為整個/多個產品或系統.
      * Unit Tests 必需是環境無關的 (借由使用 mock/stub). Functional Tests 則沒有限制可使用任意環境. Integration Tests 則應該使用盡可能接近 production 的環境.
      * Unit Tests 測試一個 unit (可能是一個 class 或 object) 的各個方法/狀態. Functional Tests 測試某個功能的各種 (盡可能多的) 使用案例. Integration Tests 測試整個產品/系統正常運作.
  * 參考: [What's the difference between unit, functional, acceptance, and integration tests?](http://stackoverflow.com/questions/4904096/whats-the-difference-between-unit-functional-acceptance-and-integration-test)
* code style linting tool 的目的是什麼?
  * 答: 協助你改進 code 的品質並避免 bad practice
  * 也看看: [JSLint Help](http://www.jslint.com/help.html)