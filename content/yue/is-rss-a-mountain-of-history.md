---
date: "2026-03-16T13:19:00+00:00"
lastmod: "2026-03-16T14:07:14+00:00"
title: "RSS 係咪屎山？"
description: "RSS 真係有大家想像中咁完美？"
translationsKey: "is-rss-a-mountain-of-history"
categories:
  - "Computer Science 電腦科學"
---

最近有一個叫做 [BlogBlog 同樂會](https://blogblog.club) 嘅社羣喺度不斷推廣 RSS 嘅好處，講到 RSS 可以幫助我哋擺脫演算法嘅控制之類，吹到 RSS 天上有地下無咁。介紹 RSS 優點嘅相關文章喺呢個社羣入面大把，我就唔列出嚟喇。**之不過大家有冇諗過，RSS 係咪真係有大家想像中咁完美？**

# Pushing vs Polling
RSS 係一個只支持 polling 而冇得 pushing 嘅技術，咁意味住 RSS 閱讀器係需要不斷咁向提供 RSS 嘅網站攞資料，同 pushing 比起嚟，其實係會更加嘥網絡頻寬㗎，亦都會增加來源伺服器嘅負擔。而呢個問題，喺 AI 時代來臨之後嚴重咗。

# 網絡爬蟲之戰
喺 AI 時代來臨之後，啲網絡爬蟲為咗大規模拎數據訓練，開始變得唔講武德，連上古時代嘅君子協定 [`robots.txt`](https://zh.wikipedia.org/wiki/Robots.txt) 都一於唔理喇，，變相變咗做 DDoS，令全世界嘅網站管理員苦不堪言。面對呢個狀況，網站們祭出咗各種各樣嘅反制措施，有啲網站需要登入先用到某啲功能，或者啓用人機驗證做過濾，呢啲反制措施完全同 RSS 只支持 polling 嘅機制背道而馳，變相令全世界嘅 RSS 閱讀器都廢咗武功。

# 我哋使唔使憎恨 RSS？
我都唔知道。我會喺呢個部落格度照提供住 RSS 先，不過我心入面確實係有啲改良方向：

- 用 pushing，唔好用落後嘅 polling 方式更新資料
- 強化安全機制，防止 XSS
- 推動格式標準化
- 建立「先驗證後抓取」嘅機制，降低 polling 機制所帶來嘅 DDoS 負擔。咁都會為付費牆生態下嘅生存提供了改進。
