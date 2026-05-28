---
date: "2026-04-12T20:30:00+00:00"
lastmod: "2026-04-24T10:45:25+00:00"
type: "post"
title: "RSS 日期調整"
description: "RSS 裏面的發佈日期，最佳值是什麼？"
translationKey: "rss-publish-date-adjustment"
categories:
  - "Blog 部落格"
  - "Computer Science 電腦科學"
tags:
  - "RSS"
  - "Hugo"
---

今天讀到了支共喉舌 Alex Hsu 的這篇《[你的RSS文章寿命可能只有别人的一半](https://alexhsu.com/publish-date)》，馬上檢查了一下自己網站的 RSS 資訊，發現 RSS 輸出裏的 `<pubDate>` 拿的是 `date` 而不是我在 frontmatter 裏指定的 `lastmod` 屬性。後者才是我期望在 RSS 裏顯示的發佈時間。

# 我目前的發佈流程
本站目前是用 Hugo 和 GitHub Pages 架起來的。我在撰寫一篇新文章的時候，會在臨保存文件的時候看一下當下的時間，然後手動把時間填上去 frontmatter 的 `date`，然後盡快推上 GitHub。我在更新舊文的時候，我不會動 `date` 而是動 `lastmod`。殊不知原來這樣的更新方法，反而是把自己給坑了。

# 開工
要改動 RSS 裏的發佈時間，我們可以從 [`layouts/rss.xml`](https://github.com/CaiJimmy/hugo-theme-stack/blob/master/layouts/rss.xml) （至少我現在用的這個主題是這個文件）這個文件入手。這個文件裏面有一行長這樣：

```xml
<pubDate>{{ .PublishDate.Format "Mon, 02 Jan 2006 15:04:05 -0700" | safeHTML }}</pubDate>
```

其中 [`.PublishDate`](https://gohugo.io/methods/page/publishdate/) 代表 frontmatter 的 `date` 屬性，而 `lastmod` 屬性就是 [`.Lastmod`](https://gohugo.io/methods/page/lastmod/)。把這個值替換掉應該就行了。

我不知道以我目前的發佈流程來看，可不可以做到自動填入和更改發佈日期，如果可以的話以後有空也許會試着做做看。

> [!IMPORTANT] 2026/04/24 更新
> 我找到了可以從 Git 抓取日期的 [Hugo 選項](https://gohugo.io/configuration/all/#enablegitinfo)。不確定這是否對我有用。
