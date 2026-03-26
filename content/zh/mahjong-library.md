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

基於種種因素，我覺得我應該會需要用到不止一種編號方式（當中程式庫內部主要使用的是 system 1），所以在這裏也順便把不同編號系統的轉換函數也弄出來：
```js
const MahjongHelper = {
  s2Tos3SuitMap: new Uint8Array([2, 0, 1, 3]),
  s3Tos2SuitMap: new Uint8Array([1, 2, 0, 3]),
  s2Tos4SuitMap: new Uint8Array([25, 16, 7, 0]),
  s3Tos4SuitMap: new Uint8Array([7, 25, 16, 0]),
  s1tos2: (i) => ((i >> 4) * 9) + (i & 15),
  s2tos1: (i) => (Math.floor(i / 9) << 4) + (i % 9) + 1,
  },
  s1tos3: (i) => (s2Tos3SuitMap[i >> 4] * 9) + (i & 15),
  s3tos1: (i) => (s3Tos2SuitMap[Math.floor(i / 9)] << 4) + (i % 9) + 1,
  },
  s1tos4: (i) => ((s2Tos4SuitMap[i >> 4]) * 9) + (i & 15),
  s3tos4: (i) => ((s3Tos4SuitMap[i >> 4]) * 9) + (i & 15)
};
```

## Unicode 字符
Unicode 有定義麻將牌字元，不過 mapping 和我用的有點不同，需要做一點轉換：
```js
const MahjongHelper = {
  toChar: (n) => `\ud83c${String.fromCharCode(MahjongHelper.s1tos4(n) + 0xdc00)}`
};
```

# 牌堆的表示
對於 136 張麻將牌而言，組合數 一共有 136! ÷ (4!<sup>34</sup>) ≈ 1.591 × 2<sup>616</sup> 種，需要 617 個二進制位來表示，編碼成 base64 字串需要 103 個字符。

對於 144 張麻將牌而言，組合數 一共有 144! ÷ (4!<sup>34</sup>) ≈ 1.675 × 2<sup>673</sup> 種，需要 674 個二進制位來表示，編碼成 base64 字串需要 113 個字符。

# 極簡化
最後就是將整個程式庫的邏輯盡量簡化，然後將看起來適合塞進 WebAssembly 塞進 WebAssembly 就大功告成了。

## 邏輯簡化
實際上我在上面就有用到一些簡化邏輯的例子了。簡化邏輯手段不是完全純手搓的，有用到 AI 輔助啦。
