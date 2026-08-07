---
date: "2026-03-25"
lastmod: ""
draft: true
title: "麻將程式庫"
description: "一個極簡主義的 WebAssembly + JavaScript 麻將程式庫。"
translationKey: "mahjong-library"
categories:
  - "Computer Science 電腦科學"
  - "Digital Minimalism 數位極簡主義"
tags:
  - "Mahjong 麻將"
  - "JavaScript"
  - "WebAssembly"
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
    return (n & 15) + "psmzh"[n >> 4];
  }
};
```

日本麻將有一套約定俗成的手牌表示方法，我把對應的轉換函數也弄出來：
```js
const MahjongHelper = {
  textToHand: (hand) => {
    if (/[^0-9mpszh]/.test(hand))
      throw new Error("Invalid characters detected. Only digits and 'mpszh' are allowed.");

    const output = [];

    const suits = hand.match(/\d+[psmzh]/gi) || [];

    if (segments.join("").length !== hand.length) 
      throw new Error("Invalid format: Every number sequence must end with a suit character (m, p, s, z, or h).");

    for (const set of suits) {
      let arr = Array.from(set);
      const suit = "psmzf".indexOf(arr.splice(-1));
      output.push(...arr.map(n => parseInt(n) + (suit << 4)));
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
  s1tos2: (i) => {
    const v = i - 1;
    return ((v >> 4) * 9) + (v & 15);
  },
  s2tos1: (i) => {
    return (Math.floor(i / 9) << 4) + (i % 9) + 1;
  },
  s1tos3: (i) => {
    const v = i - 1;
    return (s2Tos3SuitMap[v >> 4] * 9) + (v & 15);
  },
  s3tos1: (i) => {
    return (s3Tos2SuitMap[Math.floor(i / 9)] << 4) + (i % 9) + 1;
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
我想制定一個特殊的索引法，方便玩家分享牌堆。

對於 136 張麻將牌而言，組合數 一共有 136! ÷ (4!<sup>34</sup>) ≈ 1.591 × 2<sup>616</sup> 種，需要 617 個二進制位來表示，編碼成 base64 字串需要 103 個字符。

對於 144 張麻將牌而言，組合數 一共有 144! ÷ (4!<sup>34</sup>) ≈ 1.675 × 2<sup>673</sup> 種，需要 674 個二進制位來表示，編碼成 base64 字串需要 113 個字符。

```js
const factorialCache = [1n, 1n];
function factorial(n) {
  if (n < 0) return 0n;
  while (factorialCache.length <= n) {
    factorialCache.push(factorialCache[factorialCache.length - 1] * BigInt(factorialCache.length));
  };
  return factorialCache[n];
};

/**
 * Converts a BigInt into a Uint8Array (Big-Endian).
 */
function bigIntToUint8Array(bigInt) {
  let hex = bigInt.toString(16);
  // Ensure even length for hex string
  if ((hex.length & 1) !== 0) hex = "0" + hex;

  const len = hex.length >> 1;
  const u8 = new Uint8Array(len);
  for (let i = 0; i < len; i++) {
    u8[i] = parseInt(hex.substr(i << 1, 2), 16);
  };
  return u8;
};

const MahjongHelper = {
  deckToIndex: function (deck, mapFunc, postProcessing = n => bigIntToUint8Array(n).toBase64()) {
  },
  indexToDeck: function (inputIndex, mapFunc) {
  }
};
```

# 洗牌算法
理論上我們可以使用標準的 [Fisher–Yates shuffle](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle) 算法來洗牌，不過如果 RNG 內部狀態不足 136! ÷ (4!<sup>34</sup>) ≈ 1.591 × 2<sup>616</sup> 種的話，洗出來的牌會不夠均勻。從哪裏找來一個支持如此多的內部狀態的 RNG 是個問題。

# 牌型分析
> 目標：給定任意手牌組合，分析其是否滿足胡牌型（包括基本胡牌型、和一些特殊胡牌型），並計算所有可能的役種。

> 目標：給定任意手牌組合，計算其向聽數，也就是至少要換掉多少張牌才能胡牌。

# 計分
## 日本麻將
```js
const MahjongHelper = {
  jpPoints: (fu, fan) => {
    if (fan < 1)
      throw new Error();

    if (fan < 5) {
      // const a = fu * 2 ** (fan + 2);
      const a = fu * (1 << (fan + 2));

      if (a < 2000)
        return new Uint32Array([a, a << 1, a << 2, a * 6]);
    };

    // Mangan cases
    const manganCoef = new Uint8Array([0, 0, 0, 0, 0, 1, 1.5, 1.5, 2, 2, 2, 3, 3, 4]);
    let a = manganCoef[fan] * 2000;
    if (fan == 6 || fan == 7)
      a *= 1.5;
    return new Uint32Array([a, a << 1, a << 2, a * 6]);
  }
};
```

# 極簡化
最後就是將整個程式庫的邏輯盡量簡化，然後將看起來適合塞進 WebAssembly 塞進 WebAssembly 就大功告成了。

## 邏輯簡化
實際上我在上面就有用到一些簡化邏輯的例子了。簡化邏輯手段不是完全純手搓的，有用到 AI 輔助啦。

有些整數除法也可以簡化，比如：
```js
[Math.floor(i / 9), i % 9];
// 可以簡化成：
[i * 7282 >>> 16, (i * 7282 & 0xffff) * 9 >>> 16];
```
