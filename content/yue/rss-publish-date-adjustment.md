---
date: "2026-04-12T20:30:00+00:00"
lastmod: ""
title: "RSS 日期調整"
description: "RSS 入面嘅發佈日期，最佳值係乜嘢？"
translationKey: "rss-publish-date-adjustment"
categories:
  - "Blog 部落格"
  - "Computer Science 電腦科學"
tags:
  - "RSS"
  - "Hugo"
---

今天睇咗 Alex Hsu 嘅呢篇《[你的RSS文章壽命可能只有別人的一半](https://alexhsu.com/publish-date)》，即刻 check 下自己網站嘅 RSS 資訊，發現 RSS 輸出入面嘅 `<pubDate>` 係拎 `date` 而唔係我喺 frontmatter 度指定嘅 `lastmod` 屬性。後者先至係我想喺 RSS show 出嚟嘅發佈時間。

# 我而家嘅發佈流程
呢度而家係用 Hugo 同 GitHub Pages 起出嚟嘅。響我寫一篇新文章嘅時候，會喺臨 save file 嗰陣𥄫下而家個時間，然後手動填個時間上去 frontmatter 嘅 `date`，然後盡快推上 GitHub。響我更新舊文嘅時候，我就唔郁 `date` 而係郁 `lastmod`。殊不知原來咁嘅更新方法，反而係伏咗我自己。

# 開工
要改動 RSS 入面嘅發佈時間，我哋可以從 [`layouts/rss.xml`](https://github.com/CaiJimmy/hugo-theme-stack/blob/master/layouts/rss.xml) （至少我而家用緊呢個主題係呢個 file）呢個 file 入手。呢個 file 裏面有行係咁樣：

```xml
<pubDate>{{ .PublishDate.Format "Mon, 02 Jan 2006 15:04:05 -0700" | safeHTML }}</pubDate>
```

其中 [`.PublishDate`](https://gohugo.io/methods/page/publishdate/) 代表 frontmatter 嘅 `date` 屬性，而 `lastmod` 屬性就係 [`.Lastmod`](https://gohugo.io/methods/page/lastmod/)。換咗呢個值之後應該就搞掂喇。

我唔知以我而家嘅發佈流程嚟睇，可唔可以做到自動填同改發佈日期，如果得嘅話以後有空也許會試着做做看。
