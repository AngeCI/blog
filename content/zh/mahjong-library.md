---
date: "2026-03-25"
lastmod: ""
draft: true
title: "麻將程式庫"
description: "突然手癢想搓一個極簡主義的 WebAssembly + JavaScript 麻將程式庫。"
translationKey: "mahjong-library"
categories:
  - "Computer Science 電腦科學"
  - "Digital Minimalism 數位極簡主義"
tags:
  - "Mahjong 麻將"
---

突然手癢想搓一個極簡主義的 WebAssembly + JavaScript 麻將程式庫。

> [!IMPORTANT] 想立刻體驗嗎？
> - 範例：[點我](/boardgame/)
> - 開發文檔：[點我](/docs/boardgame/)
> - 下載連結：[點我](https://github.com/AngeCI/boardgame)

# 單張牌的表示
我們可以用三位元表示花色，四位元表示數字：
```js
const MahjongHelper = {
  getSuit: (n) => n >> 4,
  getRank: (n) => n & 15,
  toString: (n) => {
    return `${(n & 15) + "psmzf"[n >> 4]}`;
  }
};
```

日本麻將有一套約定俗成的手牌表示方法，我把對應的轉換函數也弄出來：
```js
const MahjongHelper = {
  textToHand: (hand) => {
    const output = [];

    const suits = hand.split(/\d+[psmzf]/gi);
    for (const set of suits) {
      let arr = Array.from(set);
      const suit = "psmzf".indexOf(arr.splice(-1));
      output.push(...arr.map(n => parseInt(n) + suit));
    };

    return output;
  }
};
```

基於種種因素，我覺得我應該會需要用到不止一種編號方式，所以在這裏也順便把不同編號系統的轉換函數也弄出來：
```js
const MahjongHelper = {

};
```

## Unicode 字符
Unicode 有定義麻將牌字元，不過 mapping 和我用的有點不同，需要做一點轉換：
```js
const MahjongHelper = {
  toChar: (n) => {
    return `\ud83c${String.fromCharCode(MahjongHelper.s1tos4(n) + 0xdc00)}`;
  }
};
```

# 極簡化
最後就是將整個程式庫的邏輯盡量簡化，然後將看起來適合塞進 WebAssembly 塞進 WebAssembly 就大功告成了。
