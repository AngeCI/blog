---
date: "2026-03-16T13:19:00+00:00"
title: "RSS 是屎山嗎？"
description: "RSS 真的有大家想像中那麼完美？"
translationsKey: "is-rss-a-mountain-of-history"
categories:
  - "Computer Science 電腦科學"
---

最近有一個叫做 [BlogBlog 同樂會](https://blogblog.club) 的社羣在不斷推廣 RSS 的好處，說 RSS 可以幫助我們擺脫演算法的控制云云，把 RSS 吹捧得天上有地下無。介紹 RSS 優點的相關文章在這個社羣裏很多，我就不列出來了。**可是大家有沒有想過，RSS 是不是真的有大家想像中那麼完美？**

# Pushing vs Polling
RSS 是一個只支持 polling 而不能 pushing 的技術，這意味着 RSS 閱讀器是需要不斷地向提供 RSS 的網站拿資料，跟 pushing 比起來，其實是會更加浪費網路頻寬的，也會增加來源伺服器負擔。而這個問題，在 AI 時代來臨之後變得嚴重了。

# 網路爬蟲之戰
來 AI 時代來臨之後，網路爬蟲為了大規模提取數據訓練，開始變得不講武德，連上古時代的君子協定 [`robots.txt`](https://zh.wikipedia.org/wiki/Robots.txt) 都索性不理了，無差別大量爬取公開網站頁面，變相成了 DDoS，令全世界的網站管理員苦不堪言。面對這個狀況，網站們祭出了各種各樣的反制措施，有些網站需要登入才能用某些功能，或者啓用人機驗證作過濾，這些反制措施完全和 RSS 只支持 polling 的機制背道而馳，變相令全世界的 RSS 閱讀器都廢了武功。

# 我們應該憎恨 RSS 嗎？
我都唔知道。我會在這個部落格暫時照樣提供 RSS，不過我心裏確實係有些改良方向：

- 使用 pushing，別用落後的 polling 方式更新資料
- 強化安全機制，防止 XSS
- 推動格式標準化
- 建立「先驗證後抓取」的機制，降低 polling 機制所帶來的 DDoS 負擔。這也為付費牆生態下的生存提供了改進。
