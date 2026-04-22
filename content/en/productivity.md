---
date: "2026-04-14T17:50:30+00:00"
lastmod: ""
title: "Productivity: Prisoner’s Dilemma, Nash Equilibrium and Twilight of the Gods"
description: "Prisoner’s Dilemma, Nash Equilibrium and Twilight of the Gods"
translationsKey: "productivity"
categories:
  - "BlogBlog 同樂會"
  - "Artificial Intelligence 人工智能"
  - "Game Theory 博弈論"
---

> [!WARNING] Notice
> This post is a draft translation from [the Chinese version](/blog/zh/productivity/) which have not yet been thoroughly proofread.

> [!TIP] BlogBlog Club!
> This is my submission article for “[BlogBlog Club Party - April 2026](https://blogblog.club/party)”. This month’s topic is “[Productivity](https://www.wen-lab.tw/blogblog-party-productivity/)”, hosted by [Wen](https://www.wen-lab.tw). If you have your own blog, feel free to join us together!

# Incomplete information game: Prisoner’s dilemma and Nash equilibrium
Many games in this world involve incomplete information, meaning that each party involved has hidden information that is unknown to the other parties.

舉個例子，警局和兩名疑犯做認罪協商，如果兩名疑犯都不認罪（共同合作），則二人同樣判監半年；如果其中一名疑犯認罪 (“defects”) 而另一名疑犯保持沉默 (“cooperates”) 的話，認罪的一方將即時獲釋，沉默者將判監十年；如果兩名疑犯一同認罪（互相背叛），則二人同樣判監五年。而兩名疑犯之間並不能互相交流。

|  | B cooperates | B defects |
| -- | -- | -- |
| A cooperates | -0.5, -0.5 | -10, 0 |
| A defects | 0, -10 | -5, -5 |

This is called the [**prisoner’s dilemma**](https://en.wikipedia.org/wiki/Prisoner%27s_dilemma). You’ll find that when neither side can trust the other, the optimal strategy for both, considering only their own interests, is to defect! However, mutual betrayal doesn’t bring better overall benefit to either side (it only maximizes the individual interests of each suspect), creating a lose-lose situation of “better to die than live in dishonour.” When neither side has any incentive to unilaterally change their choice, a **Nash equilibrium** is formed.

It can be said that many consequences of incomplete information games actually reduce the overall productivity of the entire world.[^1]

(If the benefits of cooperation outweigh the losses from betrayal, it’s not called the prisoner’s dilemma but a [stag hunt](https://en.wikipedia.org/wiki/Stag_hunt). In a stag hunt, both “cooperation” and “betrayal” are Nash equilibria.)

# Twilight of the gods: A lose-lose situation
Facing [the never-seen change of lavatories](https://wiwi.blog/blog/notebooklm-enshittification/), there’s no perfect ready-made product in every way. You can only pick the least smelly one from all kinds of shit, and try to make it into a way that you can accept.

## Advertising dilemma
Applying the prisoner’s dilemma to the business world, it can be interpreted as follows: Each company has two choices: one is to invest more resources in advertising to weaken or defeat competitors (mutual betrayal); the other is to reach an agreement with competitors to reduce wasted resources on advertising (cooperation). In the real business environment, mutual betrayal is extremely common, resulting in a large amount of resources and network traffic being wasted on advertising, while the vast majority of consumers don’t even glance at it.

In this game, the ultimate victims are the end users. Overly aggressive advertising has forced them to activate ad blockers. With Google’s declaration of war on ad blockers, a new dilemma is quietly unfolding. Some websites and services (which may have some value) have to rely on advertising to survive or they will starve, but advertising services rely on various privacy-violating trackers, scaring away privacy-conscious users. However, this user loss indirectly threatens the platform’s finances, creating a dilemma.

## Browser wars
Let’s first take a look at the browsers you’re using. What are the advantages and disadvantages of common browsers?

- It’s not the first day that Google Chrome has been criticized as spyware and adware.
- Microsoft Edge’s interface is relatively annoying, and has also been criticized as spyware and adware.
- Firefox’s backgrounds appear more free, and its interface is cleaner, but due to its market share, some websites may not be as Firefox-friendly. Besides, [Pront](https://prontlin.com/posts/leave-chrome/#%E7%82%BA%E4%BB%80%E9%BA%BC%E4%B8%8D%E9%81%B8-firefox) and (past[^2]) [Wiwi](https://wiwi.blog/use#-%E7%80%8F%E8%A6%BD%E5%99%A8) accuse it of being a spyware, and the Android version of Firefox is reportedly not very handful.
- According to Lumière Élevé, one of the co-founders of [Lightingale Community](https://ltgc.cc), Brave should receive the same treatment as browsers that “threaten user security” (to be blocked in LTGC’s services), and says that “No one’s going to cry over a crypto bro’s scheme anyway, it’s even using the Chromium kernel controlled by Google.”, “I don’t care if it’s held at gun point or not”.
- Safari is exclusive to Apple platforms, and it used some unsavory methods to forcibly maintain its market share.
- LibreWolf is quite suitable for privacy enthusiasts, but it’s not worth to be considerated for loading pages that require hardware acceleration.
- Cromite is probably the most privacy-conscious browser in the Chromium-family, and it doesn’t have any shit from the cryptocurrency communities, but it still has some of the original sins of the Chromium-family[^3].

From above, there’s no perfect ready-made product in every way. You can only pick the least smelly one from all kinds of shit, and try to make it into a way that you can accept.

## Ragnarok of instant messaging softwares
Instant messaging softwares are also a pile of shit, you can only choose from different kinds of shit.

- Discord 商業味太濃，而且還在為年齡驗證的事炎上着呢。
- Telegram 現在收費功能越搞越多了，「永遠免費」已成絕響。而且 Telegram 的聲音傳輸質量比 Discord 差。
- Matrix 的穩定性（特別是基於 Synapse 的 homeserver）似乎還不夠好。
- Stoat 目標是替代 Discord ，雖然打着「反對歧視」的標語，但似乎對來自一些國家的人有[地域歧視](https://github.com/stoatchat/for-android/issues/58)，一竹篙打一船人。
- Fluxer 也是 Discord 的一個替代品，但它把 Discord 的氪金手段也學得別無二致，甚至有過之而無不及。
- SimpleX 能用並且保密性還是頂尖的，但跨裝置登錄就不用想了。
- Signal 雖然加密本身沒有什麼挑剔，但除了加密以外的地方問題就多了，除了私密訊息的通知推送包含消息明文外，背後的團隊也對反審查毫無概念，至今仍向受審查地區的用戶推薦他們有嚴重問題的 TLS 代理，將用戶置身於危險之中。
- Whatsapp 會把你的資料源源不斷地餵給 Meta，商業味太濃。

# Powered by love
能夠單靠參與自由軟體專案來維持生活當然很[理想](/blog/zh/perfect-days/)，但現實是能夠單單靠開發自由軟體來維持生計的人並不是太多。

現實當中 FOSS 往往靠以下方式來維生：
- 售賣對應的周邊服務（例如 Caddy、cURL、SQLite3 售賣技術支援、以及某吃相糟糕的[打譜軟體](https://wiwi.blog/blog/musescore-com-scam/)）
- 將軟體雙授權，開源軟體可以以開放原始碼協定免費使用依賴，閉源軟體必須購買付費授權（如 Qt、JUCE）
- 獲取專項撥款（如從 NLnet、FOSS.United 等獲取撥款，例如 Rethink）
- 售賣預構建二進位檔案（如 Ardour）
- 獲取政府合同（Tuwunnel 從瑞士政府獲取資助合同）或公司合同（Valve 以公司合同資助一些 KDE 開發者）
- 吃抖內（就是純粹的用愛發電，需要足夠有人氣）
- 鑽授權協定漏洞，直接售賣授權（RHEL）

這些生存手段往往對自由軟體起動時的成功率不高，結果就是自由軟體往往淪為不會被投放充足勞動力的**副業**，造成現在自由軟體市場**質素參差、良莠不齊**。

# The stagnant free software market
> “What’s the use of free software if it can’t even get anything done?” — Anonymous

The consequence of games is that many necessary software and services often fail to meet (all) ideal conditions, even though there are actually quite a few readily available options.

Wiwi once compiled a [guide to real tech nerds](https://wiwi.blog/blog/fake-vs-real-tech-nerd/). 我是覺得他裏面的不少看法（至少在 2026 年當下）實在是有點太激進，一些項目實際上並不太現實。預算不足的情況下甚至會構成對生產力的負累。

- 你以為全世界所有人都能力自架離線 AI？我的話瓶頸不在技術力，而是在某些天龍人意想不到的方面，請見[下文](#如何運用人工智能提升生產力？)。
- 「對廣告的態度」是個兩難，[上面](#廣告兩難)已經闡述了。
- 避免訂閱服務（或曰「租用」）聽起來很理想很崇高，但在當今這個諸神黃昏的時代，能用的買斷制軟體又有多少？
- 個人使用當然可以避免專有格式，但當你為了維持生計而不得不出來工作的時候，這些專有格式，你以為你能完全避免？
- 當自由軟體市場還是一潭死水的狀況下，市場需求會更加傾向 Windows 和 macOS 這些主流作業系統，開發 Linux 軟體在商業角度上並不討好。Wine 不是萬能的。
- 自架郵件伺服器你以為這麼容易？實際上在很多年前就已經被大量的 spam mail 玩成了信任死鎖，自架郵件服務很難發信的。
- 即時通訊軟體是諸神黃昏，[上文](#即時通訊軟體的諸神黃昏)已述。

不過我目前還是經濟拮据。你現在所看到的這個部落格，就連自有網域都還沒有[^4]，更何談各種極其燒錢的自架服務了。

# The impact of artificial intelligence on productivity
在人工智能能快速完成人類以往需要花更多時間和心力才能完成的工作的背景下，可以說人工智能（在某些領域的）生產力已經超越了人類。在這些被 AI 主導的領域當中，只有最頂尖的人類才能存活，生產力追不上 AI 的人就會被社會淘汰。

- 小時候有點憧憬當軟體工程師，但是現在 vibe coding 大行其道的時代下，單純當個軟體工程師在商業角度上已經不那麼討好了。
- 小時候有點憧憬當同人繪師，結果 AI 現在都能畫得比人手快且品質不錯了。
- 靜態的翻譯行為，就算不夠準確也被「夠用就行」踢下去了，只剩更傷腦筋的即時傳譯還有需求，或者可以考慮轉行當「低資源語言」（如各種漢語「方言」）的翻譯。

不過運行人工智能很吃電力，在當今全球能源危機的形勢下，人工智能的發展趨勢會否被迫放緩？

# How can artificial intelligence be used to improve productivity?
在人工智能時代下，我們應該怎樣有效運用人工智能來提升自身的生產力，以免被社會淘汰？

- 我們不應該完全抗拒 vibe coding，但也不應完全依賴它，不應對 AI 生成的代碼囫圇吞棗。<span class="chide">天知道哪天會出現[智能叛變](https://zh.wikipedia.org/wiki/%E6%A9%9F%E6%A2%B0%E5%85%AC%E6%95%B5)？</span>
- 當藝術工作者的要麼想辦法變成最頂尖的人類，要麼可以先用 AI 打草稿再想辦法精修。
- 試着強化[人與人的連結](https://uncyclopedia.hk/wiki/%E4%BA%BA%E8%88%87%E4%BA%BA%E7%9A%84%E9%80%A3%E7%B5%90)吧，有些行業會比較強調與人類之間的交流，應該會沒那麼容易被 AI 取代。

[^1]: For the sake of convenience, we will not take the difference between a single-episode prisoner’s dilemma and a multiple-episode one into account for now.
[^2]: <span class="chide">While writing this article, I checked Wiwi’s website and found that he had quietly removed the description of “Firefox is a spyware”.</span>
[^3]: For example some Chromium-only APIs, the issue of whether to take down Manifest V3, and that secretly connection to Google’s server when you query DNS.
[^4]: The mailbox listed on the home page for receiving emails only is not actually completely owned by me, it just belongs to a group that I helped to establish.
