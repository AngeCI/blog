---
date: "2026-05-02T19:46:20+00:00"
type: "post"
title: "Personal Brand Domain"
description: "How to choose a domain name for a personal brand?"
translationsKey: "personal-brand-domain"
categories:
  - "Computer Science 電腦科學"
  - "Blog 部落格"
  - "Fantasies 幻想"
tags:
  - "Domain Name 域名"
---

> [!WARNING] Notice
> This post is a draft translation from [the Chinese version](/blog/zh/productivity/) which have not yet been thoroughly proofread.

這個網站開了兩個月了，一直都沒自有網域[^1]，今天來討論一下選擇域名的話題。

我當然知道 GitHub Pages 可以綁定自定義網域，我目前沒有這樣做純粹是懶得花錢去買一個新網域而已。

# `.com` 域名的偏好
「Wiwi 部落格宇宙」中的好些部落格都對 `.com` 域名有強烈偏好。

> 在網路時代，`.com` 被佔走，就像名字被佔走一樣。
>
> ——Alex Hsu《[工程師爸爸如何幫小孩取英文名字](https://alexhsu.com/baby-names)》

- JN 有兩篇文章（[1](https://blog.giveanornot.com/account/)、[2](https://blog.giveanornot.com/get-a-domain-or-not/)）提到網域問題，他自己就是用的 `.com`域名。
- Wiwi 為「[推坑](https://wiwi.blog/blog/blogblog-party-jan-2026/)」主題所投稿的其中一篇文章，就提到了「[半夜設鬧鐘爬起來搶 `.com` 域名](https://wiwi.blog/blog/get-your-own-domain/)」這回事。
- Alex Hsu 提到他[花了 USD $1700](https://alexhsu.com/first-post) 買下了目前的 `.com` 域名。他在另一篇文章表達了對[ `[本名].com` 域名的強烈偏好](https://alexhsu.com/baby-names)。
- **域名買下來了，還要養。**[Eddie Lv](https://eddielv.com/musings/eddielv.com/) 提到，養一個 `.blog` 域名**每年**得燒掉 USD $2600，讓他果斷放棄，最後還是買了 `.com` 域名。

# 頂級域名鄙視鏈
有些人心目中有一個「頂級域名鄙視鏈」，大概長這樣：

> .com &gt; .net/.org &gt; gTLD &gt; ccTLD

按照定義，所有兩個字母的頂級域名都是「國家頂級網域」（ccTLD）。按照這條鄙視鏈，[光鶯社羣](https://ltgc.cc)位於這條鄙視鏈的底部。

就在不是很久前，[7-zip](https://7-zip.org) 就被爆出[`.com` 域名遭到搶註作釣魚網站](https://www.hkepc.com/25072/)的事件，凸顯了知度度高的服務註冊 `.com` 域名的重要性。

# 我的域名規劃
養一個域名是很燒錢的事情，只為一個部落格買一個域名並不划算。就算哪天這個網站開始有了自有網域，我大概也會把部落格放到 `b.example.com` 這種形式的 subdomain 裏。事實上我心裏已經有一個比較詳盡的 subdomain 規劃清單：
> - `a/ai`: 自架私人 AI 伺服器（如果哪天有的話）
> - `b`: 部落格
> - `f`: 論壇、留言板
> - `g`: GitHub Pages／自架 Gitea？（如果哪天有的話）
> - `i`: 圖床
> - `m/mc`: [Minecraft](/blog/zh/tags/minecraft/) 伺服器
> - `ms?`: 自架 Mastodon？（如果哪天有的話）
> - `s/t?`: 短網址
> - `sx`: 自架 searx（如果哪天有的話）
> - `v`: 自架 PeerTube（如果哪天有的話）

# 域名選擇
問題來了，我該選購什麼樣的域名呢？

有些人喜歡用本名來當作域名。Alex Hsu 就提過他偏好這樣的域名[^2]。我的話會考量到身份隔離的問題，就算我買下了以我本名構成的域名，我也不會拿來給我寫作這個部落格的身份用。

另外，`.io` 的前景不是很明朗，在它的未來走向確定之前，最好不要買新的 `.io` 域名。

# 暗網域名
[光鶯社羣](https://ltgc.cc)有些會使用暗網的成員，當中有為整個組織配置暗網域名的成員。

- Tor、I2P 有現成的挖域名工具。<span class="hovers-blur">甚至有人挖了一堆 `yjspi` 開頭的 `.onion` 域名。</span>
- Yggdrasil 本質上就是一層虛擬的 IPv6 地址，可以隨便綁定任意域名，我弄一個 `y/ygg` 的 subdomain 就好。
- Lokinet 可能缺乏現成的挖域名工具。

[^1]: 首頁列出的那個僅供收信的信箱，其實也不是完全由本人所擁有，那只是隸屬於一個我有份參與組建的團體。
[^2]: <span class="chide">雖然實際上華人的洋名可以隨便亂取，所以實際上所謂的「本名域名」也是包含到姓氏而已。我身邊甚至有會根據不同環境轉換不同洋名的人。香港的身份證上都有羅馬字，而大部份華人的身份證上的羅馬字都不包含洋名。</span>
