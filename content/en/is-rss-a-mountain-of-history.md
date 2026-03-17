---
date: "2026-03-16T13:19:00+00:00"
title: "Is RSS a Mountain of History?"
description: "Is RSS really as perfect as you imagine?"
translationsKey: "is-rss-a-mountain-of-history"
categories:
  - "Computer Science 電腦科學"
---

> [!WARNING] Notice
> This post is a draft translation from [the Chinese version](/blog/zh/is-rss-a-mountain-of-history/) which have not yet been thoroughly proofread.

There is a community called [BlogBlog Club](https://blogblog.club) which is promoting the advantages of RSS recently, claiming that RSS can help us break free from the control of algorithms, and so on. There are many articles in this community introducing the advantages of RSS, so I won't list them here. **But have you ever considered whether RSS is really as perfect as you imagine?**

# Pushing vs Polling
RSS is a technology that only supports polling and cannot push, which means that RSS readers need to constantly get data from websites that provide RSS. And this problem became serious after the advent of the AI ​​​​era.

# Battle of webcrawlers
After the advent of the AI ​​​​era, web crawlers in order to extract data on a large scale for training, began to become unethical, even the ancient gentleman protocol [`robots.txt`](https://zh.wikipedia.org/wiki/Robots.txt) simply ignored, indiscriminately crawled public website pages in large numbers, disguised as DDoS, making webmasters all over the world miserable. Faced with this situation, websites have resorted to various countermeasures. Some websites require login to use certain functions, or enable human-machine verification for filtering. These countermeasures are completely contrary to the mechanism of RSS only supporting polling.

# Should we hate RSS?
I don’t know either. I will provide RSS in this blog for now, but I do have some improvements in mind:

- Use pushing, don’t update data using lagging polling
- Strengthen security mechanisms to prevent XSS
- Promote format standardization
- Establish a “verify first then grab” mechanism to reduce the DDoS burden brought by the polling mechanism. This also provides an improvement for survival in the paywall ecosystem.
