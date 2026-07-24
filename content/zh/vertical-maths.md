---
date: "2026-06-01T18:46:14+00:00"
draft: true
type: "post"
title: "縱排數學 Vertical Maths"
description: ""
translationsKey: "vertical-maths"
categories:
  - "Unclassified 未分類"
---

> [!WARNING] 注意
>
> 此文章含有較多自定義 CSS，透過 RSS 閱讀器瀏覽的讀者，可[點此](/blog/zh/vertical-maths/)造訪敝站閱讀。

如何在縱書排版當中討論數學表達式？將數學式子強行旋轉 90 度並不利閱讀。我參考了現代[阿拉伯文](https://en.wikipedia.org/wiki/Modern_Arabic_mathematical_notation)使用的排版方法、以及少量中國古籍，提出了以下的實驗。

# 數字
若使用阿拉伯數字或者源自中國的[蘇州碼子](https://zh.wikipedia.org/wiki/%E8%8B%8F%E5%B7%9E%E7%A0%81%E5%AD%90)（花碼字），數位傳統上都是橫着排的。在日文縱書排版上，兩位數字可以左右並排，佔用一個漢字的位置。若用在標題的話三、四位數也可以這樣做。這種排列方式可以以如下 CSS 實現：
```css
text-combine-upright: all;
```
<blockquote style="writing-mode: vertical-rl; font-family: 'Noto Serif TC', MingLiU, serif;">
<p style="margin: 0.5em;"><span style="text-combine-upright: all;">89</span></p>
<p style="margin: 0.5em;"><span style="text-combine-upright: all;">64</span></p>
<p style="margin: 0.5em;"><span style="text-combine-upright: all;">〨〩</span></p>
<p style="margin: 0.5em;"><span style="text-combine-upright: all;">〦〤</span></p>
</blockquote>

內文超過兩位數、標題超過四位數的，日文的習慣是使用全形數字，每一個數位各自佔用一個漢字的位置。縱書花碼字我目前找不到超過三位數的用例。

## 小數與分數

# 四則運算
首先，四則運算的符號以及等號，照樣轉橫。

<blockquote style="writing-mode: vertical-rl; font-family: 'Noto Serif TC', MingLiU, serif;">
<p style="margin: 0.5em;"><span style="text-combine-upright: all;">1</span> + <span style="text-combine-upright: all;">1</span> = <span style="text-combine-upright: all;">2</span></p>
<p style="margin: 0.5em;"><span style="text-combine-upright: all;">〡</span> + <span style="text-combine-upright: all;">〡</span> = <span style="text-combine-upright: all;">〢</span></p>
</blockquote>

```css
text-orientation: upright;
```

# 冪、開方與對數
指數……
<blockquote style="writing-mode: vertical-rl; font-family: 'Noto Serif TC', MingLiU, serif;">
<p style="margin: 1.5em;"><span style="text-combine-upright: all; display: inline-block; transform: scaleX(2);">338²</span> + <span style="text-combine-upright: all; display: inline-block; transform: scaleX(1.5);">270</span> = <span style="text-combine-upright: all; display: inline-block; transform: scaleX(3);">114514</span></p>
<p style="margin: 1.5em;"><span style="text-combine-upright: all; display: inline-block; transform: scaleX(2);">〣三〨²</span> + <span style="text-combine-upright: all; display: inline-block; transform: scaleX(1.5);">〢〧〇</span> = <span style="text-combine-upright: all; display: inline-block; transform: scaleX(3);">〡一〤〥〡〤</span></p>
</blockquote>

根號順時針轉 90 度，原來的頂線變成右邊的豎線。
<blockquote style="writing-mode: vertical-rl; font-family: 'Noto Serif TC', MingLiU, serif;">
<p><span style="text-combine-upright: all;">φ</span> = ½ (√<span style="text-combine-upright: all;">5</span> + <span style="text-combine-upright: all;">1</span>)</p>
</blockquote>

對數，若註明底數
<blockquote style="writing-mode: vertical-rl; font-family: 'Noto Serif TC', MingLiU, serif;">
<span style="text-combine-upright: all;">log</span>
</blockquote>

# 三角函數（圓函數）與雙曲函數
這些函數在西文當中的記號本身其實挺爛的。阿拉伯文的記號也只是單純將那幾個單字翻譯成阿拉伯語再縮寫，本質上和西文記號沒有什麼不同。這些函數的中文名稱寫起來都頗為費時，乾脆重新發明一套記號好了。

| 英語 | 西文縮寫 | 中文 | 中文合字 | 自造符號 |
| -- | -- | -- | -- | -- |
| sine | sin | 正弦 | ⿰正玄 |  |
| cosine | cos | 餘弦 | ⿰余玄 |  |
| tangent | tan | 正切 | ⿰正七 |  |
| cotangent | cot | 餘切 | ⿰余七 |  |
| secant | sec | 正割 | ⿰正害 |  |
| cosecant | cosec, csc | 餘割 | ⿰余害 |  |

# 求和與求積

# 極限

# 微積分
