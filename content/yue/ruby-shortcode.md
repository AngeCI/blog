---
date: "2026-04-28T14:51:40+00:00"
type: "post"
title: "Ruby Shortcode"
description: "用喺 Hugo 嘅注音標示 shortcode。"
translationKey: "ruby-shortcode"
categories:
  - "Unclassified 未分類"
tags:
  - "Hugo"
---

> [!IMPORTANT] 想即刻體驗？
> - [源碼喺度！](https://github.com/AngeCI/blog/blob/main/layouts/_shortcodes/ruby.html)
> - [配套 CSS](https://github.com/AngeCI/blog/blob/main/assets/scss/custom.scss#L355-L358)

喺研究 Hugo 嘅過程當中整出嚟嘅一個實用小玩具。

# 用法
Markdown 語法：
```md
{{</* ruby 文字 注音 */>}}
```
渲染效果：
{{< ruby 文字 注音 >}}
