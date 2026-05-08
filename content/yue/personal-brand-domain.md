---
date: "2026-05-02T19:46:20+00:00"
lastmod: "2026-05-03T10:12:36+00:00"
type: "post"
title: "個人品牌網域"
description: "點樣為個人品牌揀網域？"
translationsKey: "personal-brand-domain"
categories:
  - "Computer Science 電腦科學"
  - "Blog 部落格"
  - "Fantasies 幻想"
tags:
  - "Domain Name 域名"
---

呢個網站開咗兩個月喇，一直都冇自有網域[^1]，今日嚟討論一下揀域名嘅話題。

我當然知道 GitHub Pages 可以綁定自定義網域，我而家冇咁做純粹係懶得使錢去買個新網域啫。

# `.com` 域名嘅偏好
「Wiwi 部落格宇宙」入面嘅好一啲部落格都對 `.com` 域名有強烈偏好。

> 在網路時代，`.com` 被佔走，就像名字被佔走一樣。
>
> ——Alex Hsu《[工程師爸爸如何幫小孩取英文名字](https://alexhsu.com/baby-names)》

- JN 有兩篇文（[1](https://blog.giveanornot.com/account/)、[2](https://blog.giveanornot.com/get-a-domain-or-not/)）提到網域問題，他自己就係用緊 `.com`域名。
- Wiwi 為「[推坑](https://wiwi.blog/blog/blogblog-party-jan-2026/)」主題所投稿嘅其中一篇文，就提到「[半夜較鬧鐘爬起身搶 `.com` 域名](https://wiwi.blog/blog/get-your-own-domain/)」呢回事。
- Alex Hsu 提到佢[使咗 USD $1700](https://alexhsu.com/first-post) 買咗而家個 `.com` 域名。他喺另一篇文表達咗對[ `[真名].com` 域名嘅強烈偏好](https://alexhsu.com/baby-names)。
- **域名買咗落嚟，仲要養。**Eddie Lv [提到](https://eddielv.com/musings/eddielv.com/)，養一個 `.blog` 域名**每年**要燒 USD $2600，令他果斷放棄，最終都係買咗 `.com` 域名。

# 頂級域名鄙視鏈
有啲人心目中有個「頂級域名鄙視鏈」，大概係咁樣：

> `.com` &gt; `.net`/`.org` &gt; gTLD &gt; ccTLD

按照定義，所有兩個字母嘅頂級域名都係「國家頂級網域」（ccTLD）。按照這條鄙視鏈，[光鶯社羣](https://ltgc.cc)位於呢條鄙視鏈嘅底部。

就喺唔係好耐之前，[7-zip](https://7-zip.org) 就被爆出[`.com` 域名遭到搶註做釣魚網站](https://www.hkepc.com/25072/)嘅事件，凸顯咗知名度高嘅服務註冊 `.com` 域名嘅重要性。

另外如果你要諗住自架郵件伺服器嘅話，實際上電子郵件服務喺好多年前就已經被大量嘅 spam mail 玩成咗[信任死鎖](/blog/yue/productivity/)，自架郵件服務好難出信㗎。據說 `.com` 域名發出嘅郵件送達率會高啲，而推出時代近啲嘅 `.win`、`.xyz` 之類嘅域名，<span class="chide">由於容易被人拎嚟做奇怪嘅嘢，</span>比較容易被其他網站不信任。

# 我嘅域名規劃
養一個域名係件好燒錢嘅事，齋為一個部落格買一個域名並唔划算。就算邊日呢個網站開始有自有網域，我應該都會將個部落格擺去 `b.example.com` 呢個樣嘅 subdomain 度。事實上我心入面已經有一個比較詳盡嘅 subdomain 規劃清單：
> - `a/ai`: 自架私人 AI 伺服器（如果邊日有嘅話）
> - `b`: 部落格
> - `f`: 論壇、留言板
> - `g`: GitHub Pages／自架 Gitea？（如果邊日有嘅話）
> - `i`: 圖床
> - `m/mc`: [Minecraft](/blog/yue/tags/minecraft/) 伺服器
> - `ms?`: 自架 Mastodon？（如果邊日有嘅話）
> - `s/t?`: 短網址
> - `sx`: 自架 searx（如果邊日有嘅話）
> - `v`: 自架 PeerTube（如果邊日有嘅話）

# 域名選擇
問題來喇，我應該揀點樣嘅域名呢？

有啲人鍾意用真名嚟做域名。Alex Hsu 就提過他鍾意咁嘅域名[^2]。我嘅話會考量到身份隔離嘅問題，就算我買咗用我真名構成嘅域名，我都唔會拎嚟畀我寫呢一個部落格嘅身份用。

另外，`.io` 嘅前景唔係好明朗，喺佢嘅未來走向確定之前，最好唔好買新嘅 `.io` 域名。

# 暗網域名
[光鶯社羣](https://ltgc.cc)有啲會用暗網嘅成員，當中有為成個組織配置暗網域名嘅成員。

- Tor、I2P 有現成嘅挖域名工具。<span class="hovers-blur">甚至有人挖咗一堆 `yjspi` 開頭的 `.onion` 域名。</span>
- Yggdrasil 本質上就係一層虛擬嘅 IPv6 地址，可以隨便綁定任意域名，我整個 `y/ygg` 嘅 subdomain 就得。
- Lokinet 可能冇現成嘅挖域名工具。

[^1]: 首頁列出嘅嗰個僅供收信嘅信箱，其實都唔係完全由本人所擁有，佢只係隸屬於一個我有份參與組建嘅團體。
[^2]: <span class="chide">雖然實際上華人嘅英文名可以是但改，所以實際上所謂嘅「真名域名」都係包含到姓氏㗎啫。我身邊甚至有會根據唔同環境轉換唔同嘅英文名嘅人。香港嘅身份證度都有羅馬字，而大部份華人嘅身份證上面嘅羅馬字都冇包含英文名。</span>
