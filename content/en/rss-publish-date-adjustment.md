---
date: "2026-04-12T20:30:00+00:00"
lastmod: ""
title: "RSS Publish Date Adjustment"
description: "What is the optimal value of the publish date inside a RSS feed?"
translationKey: "rss-publish-date-adjustment"
categories:
  - "Blog 部落格"
  - "Computer Science 電腦科學"
tags:
  - "RSS"
  - "Hugo"
---

> [!WARNING] Notice
> This post is a draft translation from [the Chinese version](/blog/zh/rss-publish-date-adjustment/) which have not yet been thoroughly proofread.

After reading about Alex Hsu’s post “[Your RSS posts might only live half as long as everyone else’s](https://alexhsu.com/en/publish-date)” today, I immediately checked the information in my RSS feed, and found out that the `<pubDate>` field inside the RSS feed takes the `date` field instead of the `lastmod` field, specified inside the post’s frontmatter. The publishing time I expect to be displayed in RSS feed should be the latter.

# My current publishing process
This site is currently built using Hugo and GitHub Pages. When I write a new article, I will look at the current time just before saving the file, then manually fill in the time into the `date` field of the frontmatter, and then push it to GitHub as soon as possible. When I update old articles, I don’t edit the `date` field but `lastmod` instead. Little did I know, that such an update method would actually screw myself up.

# Start working on it
To modify the publish date inside the RSS feed, we can start with the file [`layouts/rss.xml`](https://github.com/CaiJimmy/hugo-theme-stack/blob/master/layouts/rss.xml) (at least this is the file for the theme I am using now). There is a line in this file that looks like this:

```xml
<pubDate>{{ .PublishDate.Format "Mon, 02 Jan 2006 15:04:05 -0700" | safeHTML }}</pubDate>
```

Where [`.PublishDate`](https://gohugo.io/methods/page/publishdate/) represents the `date` field  of the frontmatter, and `lastmod` is [`.Lastmod`](https://gohugo.io/methods/page/lastmod/). Just replace this value and it should be done.

I don’t know if it is possible to automatically fill in and change the publish date based on my current publishing process. If possible, I might try it when I have time in the future.
