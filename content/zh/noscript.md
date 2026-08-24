---
date: "2026-08-04T17:25:33+00:00"
lastmod: "2026-08-24T10:17:24+00:00"
type: "post"
title: "NoScript"
translationKey: "noscript"
categories:
  - "JavaScript"
  - "Computer Science 電腦科學"
---

有些人習慣預設禁用瀏覽器執行 JavaScript 的權限，甚至對一切含有 JavaScript 的網頁持反感態度。最近讀到了 Taxodium 的《[請開啟 JavaScript 以繼續搜尋](https://taxodium.ink/google-turn-on-javascript-to-keep-searching.html)》和廢文小天地的《[關掉 JS 也沒問題！](https://trashposts.com/blog/no-js-no-problem/)》。我想提出一些對激進 NoScript 觀點的反駁。

Taxodium 把網站被禁用 JavaScript 的表現分成了五個 tier：
> 1. 一片空白或者就一直加載，沒有任何提示
> 2. 無法閱讀，但會檢測沒有開啟 JavaScript (一般會用 [&lt;noscript&gt;](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/noscript))，並給出相關提示
> 3. 可以閱讀，但因為禁用 JavaScript 導致樣式有問題，部分功能受限 (例如切換明暗主題)
> 4. 可以閱讀，樣式正常，部分功能受限
> 5. 可以閱讀，樣式正常，部分功能受限，合理地處理了需要 JavaScript 的功能 (可能隱藏、可能給出提示、或者用 CSS 禁用)

我得為 JavaScript 辯護一下：
- 上述的一個 tier，其實有可能是客戶端防自動程式指紋測試，也就是反爬蟲程式。雖然說不少部落格作者[不太介意](https://wiwi.blog/blog/ai-learns-me)自己的網站被 AI 爬蟲爬，只是現今的爬蟲與以前比起來不太講武德，對網站伺服器構成了巨大的負擔，於是要不要啟用反爬蟲功能其實是一個兩難問題。
- 第二個 tier 有可能是用以減輕網路頻寬負荷的延遲載入（lazy loading）手段。合適的延遲載入或者客戶端網頁生成可以（在犧牲一點點客戶端性能的前提下）在加快 FCP 的同時降低客戶端帶寬要求。
- 有些網站有非服務端多語言支援，這個時候你禁用 JavaScript 的話就有可能變成上述的第三個 tier。本站目前語言的切換是全手動的，不會遇到上述問題，但明暗主題切換需要用到 JavaScript。
- JavaScript 也有寫得好與不好之分。
- 你現在看到的這個部落格，目前需要依賴 JavaScript 的功能應該只有文章搜尋和隨機文章，禁用 JavaScript 對閱讀體驗的影響應該不是很大。以後可能會加入的依賴 JavaScript 的功能有：內嵌外部影片播放器、留言版、自製互動工具。不過我的 [GitHub Pages](https://angeci.github.io) 除了文檔之外，其他內容基本上都是 JavaScript 程序。

# 網站搬家？
最近留意到 [It's小白](https://itsxiaobai.codeberg.page/pages/)某天決定把整個網站[從 GitHub Pages 搬到 Codeberg 上](https://itsxiaobai.codeberg.page/pages/blog/finally-changed-to-codeberg/)，同時把舊網域直接下架了，結果他還得親自給引用過舊域名的每一個網站寫信改連結，然後他的郵箱就被當成 spam account 了。如果他允許自己使用 JavaScript 的話，他其實只要在舊有的 GitHub repo 塞個 `404.html` 透過 JavaScript 自動跳轉到新的網站就好，那就不用這麼麻煩。

```html
<!DOCTYPE html><head><title>Redirecting...</title><script>this.location.href=this.location.href.replace("https://old.example.com/blog/","https://new.domain.net/blog/");</script></head><body><p>Redirecting to the new website...</p><noscript><p style="color: #f00">Static hosting requires JavaScript for redirection. Enable JavaScript in your browser first.</p></noscript></body>
```

> [!TIP]
> 讀者對於以上的論述有什麼想法？歡迎來信 angeci (at) ltgc.cc 與我討論（或者本站出現留言版的時候留個言）！
