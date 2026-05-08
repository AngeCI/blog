---
date: "2026-05-02T19:46:20+00:00"
lastmod: "2026-05-03T10:12:36+00:00"
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
> This post is a draft translation from [the Chinese version](/blog/zh/personal-brand-domain/) which have not yet been thoroughly proofread.

This website has been running for two months, but it still doesn’t have its own domain [^1]. Today, let’s discuss the topic of choosing a domain name.

Of course, I know that GitHub Pages can be linked to a custom domain, but I haven’t done so simply because I’m too lazy to spend money to buy a new domain.

# Preference to `.com` domains
Many blogs inside the “Wiwi Blog Universe” show a strong preference to `.com` domain names.

> In the internet age, having your `.com` taken is like having your name taken.
>
> ——Alex Hsu “[How an engineer dad picks baby names](https://alexhsu.com/en/baby-names)”

- JN has two posts ([1](https://blog.giveanornot.com/account/), [2](https://blog.giveanornot.com/get-a-domain-or-not/)) discussing getting domains, himself using a `.com` domain.
- One of Wiwi’s submissions to the “[Pushing the Pit](https://wiwi.blog/blog/blogblog-party-jan-2026/)” theme mentioned that he would “[set an alarm to wake up in the middle of the night to grab a `.com` domain](https://wiwi.blog/blog/get-your-own-domain/)”.
- Alex Hsu says that he [spent USD $1700](https://alexhsu.com/en/first-post) to buy his current `.com` domain. He also expressed [his strong preference to `[firstnamelastname].com` domains](https://alexhsu.com/en/baby-names).
- **You’ve bought the domain name, and you still need to feed it.** Eddie Lv [mentioned](https://eddielv.com/musings/eddielv.com/) that maintaining a `.blog` domain would cost him USD $2600 **per year**, which made him give up on the idea, and he ended up buying a `.com` domain instead.

# Top-level domain hierarchy
Some people have a “top-level domain hierarchy” in their minds, which looks something like this:

> `.com` &gt; `.net`/`.org` &gt; gTLD &gt; ccTLD

By definition, all two-letter top-level domains are “country code top-level domains” (ccTLDs). According to this hierarchy, [Lightingale Community](https://ltgc.cc) is at the bottom of this hierarchy.

Not long ago, [7-zip](https://7-zip.org) was found that [it has its `.com` domain name being squatted as a phishing website](https://www.hkepc.com/25072/), this highlights the importance of registering `.com` domains for services with high brand awareness.

Additionally, if you’re considering setting up your own mail server, you would have to consider the fact that the global email service has been plagued by spam mails for many years as well, creating a [trust deadlock](/blog/productivity/) that makes sending emails from new domain names very difficult. It’s said that emails sent from `.com` domains would have a higher deliver rate, while newer domains like `.win` and `.xyz` <span class="chide">are often used for suspicious purposes, and</span> are more likely to be distrusted by other websites.

# My domain plans
Maintaining a domain name is very expensive, and buying a domain name just for a blog isn’t worthwhile. Even if this website eventually has its own domain, I’ll probably put the blog in a subdomain like `b.example.com`. In fact, I already have a fairly detailed list of subdomains in mind:
> - `a/ai`: Self-hosted privte AI service (if ever made)
> - `b`: Blog
> - `f`: Forum, message board
> - `g`: GitHub Pages／self-hosted Gitea? (if ever made)
> - `i`: Image host
> - `m/mc`: [Minecraft](/blog/tags/minecraft/) server
> - `ms?`: Self-hosted Mastodon? (if ever made)
> - `s/t?`: Tiny URLs
> - `sx`: Self-hosted searx (if ever made)
> - `v`: Self-hosted PeerTube (if ever made)

# Choosing a domain name
The question is, what kind of domain name should I choose?

Some people like to use their real names as domain names. Alex Hsu mentioned that he prefers such domain names[^2]. I would consider the issue of identity segregation, even if I bought a domain name containing my real name, I wouldn’t use it for this particular blogging identity.

Furthermore, the future of `.io` is quite uncertain. Until its future is clear, it’s best not to buy a new `.io` domain.

# Dark web domain names
[Lightingale Community](https://ltgc.cc) has members regularly using dark web services, some of them also help the entire community set up dark web domains.

- Tor and I2P have readily available tools for mining domain names. <span class="hovers-blur">Someone even mined a bunch of `.onion` domain names starting with `yjspi`.</span>
- Yggdrasil is essentially a virtual IPv6 address that can be bound to any domain name. I would just have to create a subdomain called `y/ygg` for it.
- Lokinet may lack readily available domain mining tools.

[^1]: The mailbox listed on the home page for receiving emails only is not actually completely owned by me, it just belongs to a group that I helped to establish.
[^2]: <span class="chide">Though Chinese people can actually choose any Western name they like, so the so-called “real name domain name” actually only includes the surname. I even know people who change their Western names depending on the situation. Identity cards in Hong Kong have Romanizations, but most ethnic Chinese people do not actually include their Western names on their identity cards.</span>
