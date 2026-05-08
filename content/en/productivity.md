---
date: "2026-04-14T17:50:30+00:00"
lastmod: ""
type: "post"
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

Let’s say the police is negotiating a plea bargain with two prisoners. If both prisoners remain silent (“mutual cooperation”), they will each serve to six months in prison; If one testifies against the other (“defects”) while the other does not (“cooperates”), the one testifying will be set free while the other serves ten years in prison; If they both testify against each other (“mutual defection”), they will each serve five years. The two prisoners are separated and cannot communicate with each other.

|  | B cooperates | B defects |
| -- | -- | -- |
| A cooperates | -0.5, -0.5 | -10, 0 |
| A defects | 0, -10 | -5, -5 |

This is called the [**prisoner’s dilemma**](https://en.wikipedia.org/wiki/Prisoner%27s_dilemma). You’ll find that when neither side can trust the other, the optimal strategy for both, considering only their own interests, is to defect! However, mutual defection doesn’t bring better overall benefit to either side (it only maximizes the individual interests of each prisoner), creating a lose-lose situation of “better to die than live in dishonour.” When neither side has any incentive to unilaterally change their choice, a **Nash equilibrium** is formed.

It can be said that many consequences of incomplete information games actually reduce the overall productivity of the entire world.[^1]

(If the benefits of cooperation outweigh the losses from betrayal, it’s not called the prisoner’s dilemma but a [stag hunt](https://en.wikipedia.org/wiki/Stag_hunt). In a stag hunt, both “cooperation” and “betrayal” are Nash equilibria.)

# Twilight of the gods: A lose-lose situation
Facing [the never-seen change of lavatories](https://wiwi.blog/blog/notebooklm-enshittification/), there’s no perfect ready-made product in every way. You can only pick the least smelly one from all kinds of shit, and try to make it into a way that you can accept.

## Advertising dilemma
Applying the prisoner’s dilemma to the business world, it can be interpreted as follows: Each company has two choices: one is to invest more resources in advertising to weaken or defeat competitors (mutual defection); the other is to reach an agreement with competitors to reduce wasted resources on advertising (cooperation). In the real business environment, mutual defection is extremely common, resulting in a large amount of resources and network traffic being wasted on advertising, while the vast majority of consumers don’t even glance at it.

In this game, the ultimate victims are the end users. Overly aggressive advertising has forced them to activate ad blockers. With Google’s declaration of war on ad blockers, a new dilemma is quietly unfolding. Some websites and services (which may have some value) have to rely on advertising to survive or they will starve, but advertising services rely on various privacy-violating trackers, scaring away privacy-conscious users. However, this user loss indirectly threatens the platform’s finances, creating a dilemma.

## Browser wars
Let’s first take a look at the browsers you’re using. What are the advantages and disadvantages of common browsers?

- It’s not the first day that Google Chrome has been criticized as spyware and adware.
- Microsoft Edge’s interface is relatively annoying, and has also been criticized as spyware and adware.
- Firefox’s backgrounds appear more free, and its interface is cleaner, but due to its market share, some websites may not be as Firefox-friendly. Besides, [Pront](https://prontlin.com/posts/leave-chrome/#%E7%82%BA%E4%BB%80%E9%BA%BC%E4%B8%8D%E9%81%B8-firefox) and (past[^2]) [Wiwi](https://wiwi.blog/use#-%E7%80%8F%E8%A6%BD%E5%99%A8) accuse it of being a spyware, and the Android version of Firefox is reportedly not very handful.
- According to Lumière Élevé, one of the co-founders of [Lightingale Community](https://ltgc.cc), Brave should receive the same treatment as browsers that “threaten user security” (to be blocked in LTGC’s services), and says that “No one’s going to cry over a crypto bro’s scheme anyway, it’s even using the Chromium kernel controlled by Google.”, “I don’t care if it’s held at gun point or not”.
- Safari is exclusive to Apple platforms, and it used some unsavoury methods to forcibly maintain its market share.
- LibreWolf is quite suitable for privacy enthusiasts, but it’s not worth to be considerated for loading pages that require hardware acceleration.
- Cromite is probably the most privacy-conscious browser in the Chromium-family, and it doesn’t have any shit from the cryptocurrency communities, but it still has some of the original sins of the Chromium-family[^3].

From above, there’s no perfect ready-made product in every way. You can only pick the least smelly one from all kinds of shit, and try to make it into a way that you can accept.

## Ragnarok of instant messaging softwares
Instant messaging softwares are also a pile of shit, you can only choose from different kinds of shit.

- Discord is too commercialized, and there’s still controversy surrounding age verification.
- Telegram now has more and more paid features, the idea of “forever free” is a thing of the past. Furthermore, the Telegram’s sound transmission quality is worse than that of Discord’s.
- Matrix’s stability (especially the Synapse-based home server) doesn’t seem to be good enough.
- Stoat aims to replace Discord. Although it uses the slogan “anti-discrimination”, it seems to have [regional discrimination](https://github.com/stoatchat/for-android/issues/58) against people from certain countries, which is a case of generalizing from one to the whole group.
- Fluxer is also an alternative to Discord, but it has learned Discord’s monetization methods exactly, and even surpassed them.
- SimpleX is usable and offers top-notch security, but cross-device login is out of the question.
- While Signal’s encryption itself is not particularly problematic, there are many issues beyond encryption. In addition to the notification push of private messages containing plaintext, the team behind it has no concept of anti-censorship and continues to recommend their seriously problematic TLS proxy to users in censored regions, putting users at risk.
- WhatsApp will keep feeding your information to Meta, it’s too commercial.

# Powered by love
It’s quite [ideal](/blog/perfect-days/) that one could make a living solely by participating in free software projects, but in reality, very few people can sustain themselves entirely through free software development.

In reality, FOSSes often rely on the following methods to survive:
- Selling related peripheral services (such as technical support for Caddy, cURL, SQLite3, as well as a certain [scorewriter software](https://wiwi.blog/blog/musescore-com-scam/) with a bad eating manner)
- Dual licensing of software allows open-source software to use its dependencies for free under open-source licenses, while closed-source software requires a paid license. (such as Qt, JUCE)
- Obtain special grants (such as grants from NLnet, FOSS.United, etc., such as Rethink).
- Selling pre-built binary files (such as Ardour)
- Obtaining government contracts (Tuwunnel obtained a funding contract from the Swiss government) or corporate contracts (Valve funded some KDE developers through corporate contracts).
- Getting donations (Purely powered by love, requiring a lot of popularity.)
- Exploiting loopholes in licensing agreements to directly sell licenses (RHEL)

These survival strategies often have a low success rate when starting free software, resulting in free software often becoming a **side hustle** that doesn’t receive sufficient labour, leading to the current free software market being of **inconsistent quality**.

# The stagnant free software market
> “What’s the use of free software if it can’t even get anything done?” — Anonymous

The consequence of games is that many necessary software and services often fail to meet (all) ideal conditions, even though there are actually quite a few readily available options.

Wiwi once compiled a [guide to real tech nerds](https://wiwi.blog/blog/fake-vs-real-tech-nerd/). I think many of his views (at least in 2026) are too radical, and some projects are actually not very realistic. Insufficient budgets could even become a burden on productivity.

- Do you really think everyone in the world is capable of building their own offline AI? My bottleneck isn’t in technical ability, but rather in aspects that the most Taiwanese wouldn’t even aware a shit of. See [below](#how-can-artificial-intelligence-be-used-to-improve-productivity) for more details.
- “Attitude towards advertising” is a dilemma, as stated [above](#advertising-dilemma).
- Avoiding subscription services (or “renting”) sounds very ideal and lofty, but in this era of twilight of the gods, how many buy-to-play software options are actually available?
- Proprietary formats can surely be avoided for personal use, but when you have to work to make a living, do you think you can completely avoid them?
- With the free software market still stagnant, market demand will lean more towards mainstream operating systems like Windows and macOS, making Linux software development less commercially viable. Wine is not a panacea.
- Do you really think setting up a self-hosted mail server is that easy? The global email service has been plagued by spam mails for many years, leading to a trust deadlock that makes it very difficult to send emails from a self-hosted mail service.
- Instant messaging software is a twilight of the gods, as stated [above](#ragnarok-of-instant-messaging-softwares).

However, I’m currently still struggling financially. This blog you see here doesn’t even have its own domain name[^4], let alone any of the extremely expensive self-hosted services.

# The impact of artificial intelligence on productivity
Given that artificial intelligence can quickly accomplish tasks that previously required far more time and effort from humans, it’s fair to say that AI’s productivity (in some areas) has already surpassed that of humans. In these AI-dominated fields, only the most outstanding humans will survive, those whose productivity cannot keep up with AI will be eliminated by society.

- When I was a child, I dreamed of becoming a software engineer, but now that vibe coding is so popular, being a software engineer is no longer as commercially viable.
- When I was a kid, I dreamed of becoming a fan artist, but now AI can draw faster and with fair quality than humans.
- Static translations, even if not accurate enough, has been eliminated by “good enough” products. Only the more challenging real-time interpreting still has demand, or one could consider switching to translating “low-resource languages” (such as various Chinese “dialects”).

However, running artificial intelligence consumes a lot of electricity. Given the current global energy crisis, will the development of artificial intelligence be forced to slow down?

# How can artificial intelligence be used to improve productivity?
In the age of artificial intelligence, how can we effectively utilize AI to improve our productivity and avoid being left behind by society?

- We shouldn’t completely reject Vibe coding, but we also shouldn’t rely on it entirely, and we shouldn’t blindly accept AI-generated code. <span class="chide">Who knows when an [intelligent rebellion](https://en.wikipedia.org/wiki/I,_Robot_(film)) occurs?</span>
- Artists either need to find a way to become the best human being, or they can use AI to create a draft first and then find a way to refine it.
- Try to strengthen the [connections between people](https://uncyclopedia.hk/wiki/%E4%BA%BA%E8%88%87%E4%BA%BA%E7%9A%84%E9%80%A3%E7%B5%90). Some industries emphasize communication with humans and should not be so easily replaced by AI.

[^1]: For the sake of convenience, we will not take the difference between a single-episode prisoner’s dilemma and a multiple-episode one into account for now.
[^2]: <span class="chide">While writing this article, I checked Wiwi’s website and found that he had quietly removed the description of “Firefox is a spyware”.</span>
[^3]: For example some Chromium-only APIs, the issue of whether to take down Manifest V3, and that secretly connection to Google’s server when you query DNS.
[^4]: The mailbox listed on the home page for receiving emails only is not actually completely owned by me, it just belongs to a group that I helped to establish.
