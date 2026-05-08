---
date: "2026-05-02T20:27:20+00:00"
lastmod: "2026-05-03T08:45:32+00:00"
type: "post"
title: "Customizable Blog"
description: ""
translationsKey: "customizable-blog"
categories:
  - "Blog 部落格"
  - "Computer Science 電腦科學"
---

> [!WARNING] Notice
> This post is a draft translation from [the Chinese version](/blog/zh/customizable-blog/) which have not yet been thoroughly proofread.

I’ve read Alex Hsu’s “[The omakase blog: building an opinionated personal site](https://alexhsu.com/en/omakase)” recently, let me share some of my personal opinions.

# Light mode and dark mode
This site currently offers both light and dark themes, with automatic detection by default, but users can also switch between both themes by clicking a button on the menu.

But what if you’re forced to choose one? Alex mentioned he recently removed the light mode entirely, which I think is fine. Conversely, since I’m the kind of person who likes to [hide in a dark room late at night to look at my phone](/blog/jet-lag/), I don’t really like the constanly blindingly bright mode of [廢文小天地](https://trashposts.com) and [Wen](https://www.wen-lab.tw) (that kind of blindingly bright mode is more suitable for e-ink screens or paper reading, maybe I should invest in an e-reader?). In the ancient days before CSS, the default layout was always in light mode, which is why browsers now default to light mode unless colours are specified using CSS.

# Simplified/Traditional Chinese conversions
I’ve considered adding a “one-way Simplified/Traditional Chinese conversion” function to this website, but I’m lazy and haven’t implemented it yet.

As for “two-way Simplified/Traditional Chinese conversion”, it’s basically a cancer. A one-to-many conversion in Simplified Chinese often leads to over-conversion.

I was born and raised in Hong Kong. Compared to Taiwan, <span class="hovers-blur">due to cultural contact and changes in population structure,</span> Hong Kong people have more opportunities to encounter Simplified Chinese in their daily lives. Hong Kong people generally have better reading ability for Simplified Chinese than Taiwanese people, and there are fewer cases of “Simplified Chinese reading difficulties” or “[much slower](https://wiwi.blog/blog/blogblog-party-recap-feb-2026/#-%E5%8F%AA%E6%9C%89%E6%88%91%E9%80%99%E6%A8%A3%E5%B0%8D%E5%BE%85%E6%96%87%E5%AD%97%E5%97%8E) when trying to read Simplified Chinese”. My own fluency in reading Traditional and Simplified Chinese is actually not that different most of the time.

# Horizontal and vertical writings
Within the “Wiwi Blog Universe”, there’s a blog that’s uniquely written vertically, that’s “[Me](https://e89295.com)” (e89295).

Local language textbooks in Taiwan and Japan share a common feature: they are both written vertically. In contrast, textbooks in mainland China and Hong Kong are all written horizontally (although Hong Kong does have many vertically written Chinese books).

In the past, electronic devices, which are dominated by Westerners who lacked vertical writing traditions, often have poor support on vertical texts. However, the support for vertical text on electronic devices has nowadays improved considerably.

That being said, vertically formatted web pages like “My Blog” might be better viewed horizontally on mobile devices.

# Table formatting
Numerous studies have shown that limiting the width of text layouts can improve the reading experience (at least for horizontal text). I have many [linguistics](/blog/categories/linguistics-語言學) articles that include tables. When there are many columns in a table, it often overflows, which is not quite aesthetically pleasing. Perhaps I should look into better table formatting mechanisms, or maybe I should just use images instead?
