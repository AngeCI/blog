---
date: "2026-03-24"
lastmod: ""
draft: true
title: "撲克牌程式庫"
description: "一個極簡主義的 WebAssembly + JavaScript 撲克牌程式庫。"
translationKey: "playing-card-library"
categories:
  - "Computer Science 電腦科學"
  - "Digital Minimalism 數位極簡主義"
tags:
  - "Playing Cards 撲克牌"
---

突然手癢想搓一個極簡主義的 WebAssembly + JavaScript 撲克牌程式庫。

> [!IMPORTANT] 想立刻體驗嗎？
> - 範例：[點我](/boardgame/)
> - 開發文檔：[點我](/docs/boardgame/)
> - 下載連結：[點我](https://github.com/AngeCI/boardgame)

# 單張牌的表示
我們可以用兩位元表示花色，四位元表示數字：
```js
const CardHelper = {
  getSuit: (n) => n >> 4,
  getRank: (n) => n & 15,
  toString: (n) => {
    const charTable = ["Joker", "A", "2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K"];
    return `${"♠♥♣♦"[n >> 4] + charTable[n & 15]}`;
  },
  toASCIIString: (n) => `${"SHCD"[n >> 4] + ".A23456789TJQK"[n & 15]}`,
  getColor: (n) => n & 16
};
```

我選擇了「♠♥♣♦」作為我的程式庫中撲克牌花色的順序，在某些接龍類遊戲需要判斷花色「紅黑」屬性的時候會比較簡單。

為了維持與某些平台的相容性，我也會需要用到一些其他不同的編號系統，以下是轉換函數：
```js
const CardHelper = {
  s2Tos3SuitMap: new Uint8Array([3, 2, 0, 1]),
  s3Tos2SuitMap: new Uint8Array([2, 3, 1, 0]),
  s1tos2: (i) => {
    const v = i - 1;
    return ((v >> 4) * 13) + (v & 15);
  },
  s2tos1: (i) => (Math.floor(i / 13) << 4) + (i % 13) + 1,
  s2tos3: (i) => ((i % 13) << 2) + CardHelper.s2To3SuitMap[Math.floor(i / 13)],
  s3tos2: (i) => (CardHelper.s3Tos2SuitMap[i & 3] * 13) + (i >> 2),
  s1tos3: (i) => {
    const v = i - 1;
    return ((v & 15) << 2) + CardHelper.s2To3SuitMap[v >> 4];
  },
  s3tos1: (i) => (CardHelper.s3Tos2SuitMap[i & 3] << 4) + (i >> 2) + 1
};
```

## Unicode 字符
Unicode 有定義撲克牌字元，不過 mapping 和我用的有點不同，需要做一點轉換：
- 花色的順序是「♠♥♦♣」而不是「♠♥♣♦」。
- 「J」和「Q」之間有個額外的「C」。

```js
const CardHelper = {
  toChar: (n) => {
    n += (n & 12) === 12; // adjust Q and K
    n ^= (n & 32) >> 1; // reverse clubs and diamonds

    return `\ud83c${String.fromCharCode(n + 0xdca0)}`;
  }
};
```

# 牌堆的表示
對於完整的 52 張不重覆牌的牌堆，我想制定一個特殊的索引法，方便玩家分享牌堆。這樣的牌堆一共有 52! ≈ 1.496 × 2<sup>225</sup> 種組合，需要 226 個二進制位來表示，編碼成 base64 字串需要 38 個字符。

一種似乎可行的做法是使用 factorial number system：

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

function deckToIndex(deck, mapFunc, postProcessing = n => bigIntToUint8Array(n).toBase64()) {
  let n = deck.length;
  let deck2 = deck;

  if (mapFunc)
    deck2 = deck.map(mapFunc);

  let index = 0n;
  for (let i = 0; i < n; i++) {
    // Count the number of smaller elements to the right of deck2[i]
    let smaller = 0n;
    // Update the index using the factorial number system
    for (let j = i + 1; j < n; j++) {
      if (deck2[j] < deck2[i]) {
        smaller++;
      };
    };
    index += smaller * factorial(n - i - 1);
  };
  return postProcessing(index);
};

function indexToDeck(inputIndex, mapFunc, n = 52) {
  // Convert Uint8Array back to BigInt if necessary
  let index = typeof inputIndex === "bigint" 
    ? inputIndex 
    : BigInt("0x" + Array.from(inputIndex).map(b => b.toString(16).padStart(2, "0")).join(""));

  const deck = [];
  const nums = Array.from({ length: n }, (_, i) => i);
    
  for (let i = n - 1; i >= 0; i--) {
    const fact = factorial(i);
    const digit = Number(index / fact);
    index %= fact;

    deck.push(nums[digit]);
    nums.splice(digit, 1);
  };

  if (mapFunc) return deck.map(mapFunc);
  return deck;
};
```

## 例子：通用洗牌算法
使用標準的 [Fisher–Yates shuffle](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle) 算法，不過從哪裏找來一個支持至少 52! 種狀態的 RNG 是個問題。
```js
function shuffle(deck) {
  for (let i = deck.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [deck[i], deck[j]] = [deck[j], deck[i]];
  }
  return deck;
};
```

## 例子：FreeCell 牌堆生成
以下是一個 FreeCell 遊戲（繁體中文 Windows 譯為「新接龍」，簡體中文 Windows 譯為「空當接龍」）的牌堆生成算法範例：

```wat
(module
  (global $seed (mut i32) (i32.const 0))
  (func $setSeed (export "a")
    (param $newSeed i32)
    (global.set $seed (local.get $newSeed))
  )
  (func $getSeed (export "b")
    (result i32)
    global.get $seed
  )
  (func $next (export "c")
    (result i32)
    global.get $seed
    (i32.mul (i32.const 214013))
    (i32.add (i32.const 2531011))
    (i32.and (i32.const 0x7fffffff)) ;; seed = (seed * 214013 + 2531011) & 0x7fffffff
    global.set $seed
    (i32.shr_u (global.get $seed) (i32.const 16)) ;; seed >> 16
  )
)
```

```js
const wasmObj = await WebAssembly.instantiateStreaming(fetch("data:application/wasm;base64,AGFzbQEAAAABCQJgAX8AYAABfwMEAwABAQYGAX8BQQALBw0DAWEAAAFiAAEBYwACCisDBgAgACQACwQAIwALHQAjAEH9hw1sQcO9mgFqQf////8HcSQAIwBBEHYLAA4EbmFtZQIHAwAAAQACAA=="));

const RNG = class {
  constructor(seed) {
    this.setSeed(seed);
  };
  setSeed(seed) {
    wasmObj.instance.exports.a(seed);
  };
  getSeed = wasmObj.instance.exports.b;
  next = wasmObj.instance.exports.c;
};

function shuffleFreecell(seed) {
  const deck = new Uint8Array(52);
  for (let i = 0; i < 52; i++) {
    deck[i] = i;
  };

  const rand = new RNG(seed);
  for (i = 0; i < 52; i++) deck[i] = 51 - i;
  for (i = 0; i < 51; i++) {
    let j = 51 - rand.next() % (52 - i);
    seed = deck[i], deck[i] = deck[j], deck[j] = seed;
  };

  return deck.map(CardHelper.s3tos1); // format conversion
};
```

原本的種子編號可以單向轉換為上述的格式：
```js
deckToIndex(shuffleFreecell(seed), CardHelper.s1tos2); // encode
indexToDeck(Uint8Array.fromBase64(str), CardHelper.s2tos1); // decode
```
```
1 → At1i6oKsSCfOM5c3Nm6DdDdeVqsvhE9oQAWeabw
2 → AutXHnJsKMEyvzPPfPk9vmQeJ91o97UqoNVxswA
3 → Ajsr99HZnOwWqqkmZO5umpTGc2N4qYZbLstO+Ro
4 → u15L5opQdsWEZr4GstHmOobxRDwhrEK+GofO4g
5 → wbdTYlPpRsGbLMRMgwlPzk/V1t5XGWbq26B/lQ
6 → 0R3qjziS+h+By6fnid6UXXh5y+TW69I10iOyJw
7 → Al3sT4m7f1O3LM5ysVfVL+0ftRMhWXuGegfJPoE
8 → AbAPdnPFcw47xdBF4NUEWEpYS37WcZdU7+I1pqA
9 → AbwjF5w2XvlS5eSnDCwoxou3RcDOCOhsPLQd0x8
10 → P6D37kUy0krxIomXMnDDqdXh6+aor+a/mAloxw
```

# 五張牌的牌型分析
> 目標：給定任意五張不重複的撲克牌，分析出它的[牌型](https://zh.wikipedia.org/wiki/%E6%92%B2%E5%85%8B%E7%89%8C%E5%9E%8B)。

# 極簡化
最後就是將整個程式庫的邏輯盡量簡化，然後將看起來適合塞進 WebAssembly 塞進 WebAssembly 就大功告成了。

## 邏輯簡化
實際上我在上面就有用到一些簡化邏輯的例子了。簡化邏輯手段不是完全純手搓的，有用到 AI 輔助啦。
