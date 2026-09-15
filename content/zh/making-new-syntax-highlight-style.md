---
date: "2026-04-20"
lastmod: ""
draft: true
title: "手搓語法高亮主題過程"
description: ""
translationsKey: "making-new-syntax-highlight-style"
categories:
  - "Computer Science 電腦科學"
---

在[翻新網站配色](/blog/zh/blog-maintenance/)時，要決定網站使使用的語法高亮配色，於是乎我逛了一下 Hugo 提供的 [presets](https://gohugo.io/quick-reference/syntax-highlighting-styles/) ，覺得沒有找到完全符合我心裏所想的配色主題，就決定嘗試手搓一個了。

GNOME Terminal “GNOME light”
: `#171421`, `#5e5c64`, `#c01c28`, `#f66151`, `#26a269`, `#33d17a`, `#a2734c`, `#e9ad0c`, `#12488b`, `#2a7bde`, `#a347ba`, `#c061cb`, `#2aa1b3`, `#33c7de`, `#d0cfcc`, `#ffffff`

GNOME Terminal “GNOME dark”
: `#000000`, `#555555`, `#c10000`, `#f60000`, `#00aa00`, `#00f900`, `#abaa00`, `#f7f700`, `#c160c9`, `#2aa2b3`, `#31c8dd`, `#aaaaaa`, `#ffffff`

```sh
hugo gen chromastyles --style=monokai > static/css/syntax.css
```
