---
date: "2026-04-28"
type: "post"
draft: true
title: "撲克牌程式庫"
description: "一個極簡主義的 WebAssembly + JavaScript 撲克牌程式庫。"
translationKey: "playing-card-library"
categories:
  - "Computer Science 電腦科學"
  - "Digital Minimalism 數位極簡主義"
tags:
  - "Playing Cards 撲克牌"
  - "JavaScript"
  - "WebAssembly"
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
  toString: (n) => ["Joker", "A", "2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K"][n & 15] + "♠♥♣♦"[n >> 4],
  toEmojiString: (n) => ["Joker", "A", "2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K"][n & 15] + "♠♥♣♦"[n >> 4] + "\ufe0f",
  toASCIIString: (n) => ".A23456789TJQK"[n & 15] + "SHCD"[n >> 4],
  getColor: (n) => n & 16
};
```

我選擇了「♠️♥️♣️♦️」作為我的程式庫中撲克牌花色的順序，在某些接龍類遊戲需要判斷花色「紅黑」屬性的時候會比較簡單。

為了維持與某些平台的相容性，我也會需要用到一些其他不同的編號系統，以下是轉換函數：
```js
const CardHelper = {
  s2Tos3SuitMap: new Uint8Array([3, 2, 0, 1]),
  s3Tos2SuitMap: new Uint8Array([2, 3, 1, 0]),
  s1tos2: (i) => ((--i >> 4) * 13) + (i & 15),
  s2tos1: (i) => (Math.floor(i / 13) << 4) + (i % 13) + 1,
  s2tos3: (i) => ((i % 13) << 2) + CardHelper.s2To3SuitMap[Math.floor(i / 13)],
  s3tos2: (i) => (CardHelper.s3Tos2SuitMap[i & 3] * 13) + (i >> 2),
  s1tos3: (i) => ((--i & 15) << 2) + CardHelper.s2To3SuitMap[i >> 4],
  s3tos1: (i) => (CardHelper.s3Tos2SuitMap[i & 3] << 4) + (i >> 2) + 1
};
```

## Unicode 字符
Unicode 有定義撲克牌字元，不過 mapping 和我用的有點不同，需要做一點轉換：
- 花色的順序是「♠️♥️♦️♣️」而不是「♠️♥️♣️♦️」。
- 「J」和「Q」之間有個額外的「C」。

```js
const CardHelper = {
  toChar: (n) => {
    n += (n & 12) === 12; // adjust Q and K
    n ^= (n & 32) >> 1; // reverse clubs and diamonds

    return String.fromCharCode(
      0xd83c,
      (n & 15) ? n + 0xdca0 : !(n & 16) + 0xdcbf // adjust jokers
    );
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

const CardHelper = {
  deckToIndex: function (deck, mapFunc, postProcessing = n => bigIntToUint8Array(n).toBase64()) {
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
  },
  indexToDeck: function (inputIndex, mapFunc, n = 52) {
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
  }
};
```

## 例子：通用洗牌算法
使用標準的 [Fisher–Yates shuffle](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle) 算法，不過從哪裏找來一個支持至少 52! 種狀態的 RNG 是個問題。
```js
const CardHelper = {
  shuffle: function (deck) {
    for (let i = deck.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [deck[i], deck[j]] = [deck[j], deck[i]];
    };
    return deck;
  }
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

  const rand = new RNG(seed);
  for (i = 0; i < 52; i++) deck[i] = 51 - i;
  for (i = 0; i < 51; i++) {
    let j = 51 - rand.next() % (52 - i);
    [deck[i], deck[j]] = [deck[j], deck[i]];
  };

  return deck.map(CardHelper.s3tos1); // format conversion
};
```

原本的種子編號可以單向轉換為上述的格式：
```js
CardHelper.deckToIndex(shuffleFreecell(seed), CardHelper.s1tos2); // encode
CardHelper.indexToDeck(Uint8Array.fromBase64(str), CardHelper.s2tos1); // decode
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

# 其他功能
## 五張牌的牌型分析
> 目標：給定任意五張不重複的撲克牌，分析出它的[牌型](https://zh.wikipedia.org/wiki/%E6%92%B2%E5%85%8B%E7%89%8C%E5%9E%8B)。
```js
const CardHelper = {
  analyzeHand: function (hand) {
    const ranks = hand.map(CardHelper.getRank);
    const suits = hand.map(CardHelper.getSuit);
    const cards = hand.filter(card => CardHelper.getRank(card) !== 0); // filter out jokers

    if (cards.some((card, i, self) => i !== self.indexOf(card))) // check for duplicates
      return -1; // Invalid

    const flush = suits.every(suit => suit === suits[0]);
    const groups = new Uint8Array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13]).map((rank) => ranks.filter(j => rank === j).length).sort((x, y) => y - x);
    const shifted = ranks.map(n => ++n < 14 ? n : 1);
    const distance = Math.min(Math.max(...ranks) - Math.min(...ranks), Math.max(...shifted) - Math.min(...shifted));
    const straight = groups[0] === 1 && distance < 5;
    groups[0] += hand.length - cards.length; // number of jokers

    if      (groups[0] === 5)                    return 9; // Five of a kind
    else if (straight && flush)                  return 8; // Straight flush
    else if (groups[0] === 4)                    return 7; // Four of a kind
    else if (groups[0] === 3 && groups[1] === 2) return 6; // Full house
    else if (flush)                              return 5; // Flush
    else if (straight)                           return 4; // Straight
    else if (groups[0] === 3)                    return 3; // Three of a kind
    else if (groups[0] === 2 && groups[1] === 2) return 2; // Two pair
    else if (groups[0] === 2)                    return 1; // One pair
    else                                         return 0; // High card
  }
};
```

## 找最大牌
```js
const CardHelper = {
  findLargest: function (cards, trump) {
    if (typeof trump !== "undefined")
      cards = cards.filter(card => CardHelper.geSuit(card) === trump);

    const ranks = cards.map(card => {
      const rank = CardHelper.getRank(card);
      return (rank === 1) ? 14 : rank;
    });
    return cards[ranks.indexOf(Math.max(...ranks))];
  }
};
```

# UI 繪製函數
```html
<div class="card">
  <div>
    <div class="card-front"></div>
    <div class="card-back"></div>
  </div>
</div>
```
```css
.card {
  width: 15rem;
  height: 25rem;
  transition: transform 0.2s ease;
  perspective: 1000px;
}
.card > div {
  position: relative;
  border-radius: 20px;
  box-shadow: 0 0 10px 2px rgba(0, 0, 0, 0.25);
  width: 100%;
  height: 100%;
  transition: transform 0.8s ease;
  transform-style: preserve-3d;
}
.card:hover {
  transform: translateY(-1rem);
}
.card:hover~.card {
  transform: translateX(130px);
}
.card:not(:first-child) {
  margin-left: -130px;
}
.card-flipped > div {
  transform: rotateY(180deg);
}
.card-front, .card-back {
  background-color: white;
  border-radius: 20px;
  position: absolute;
  width: 100%;
  height: 100%;
  -webkit-backface-visibility: hidden;
  backface-visibility: hidden;
}
.card-back {
  background-color: #0e3075;
  transform: rotateY(180deg);
}
```
```js
const CardHelper = {
  createDOM: function (n) {
    const card = document.createElement("div");
    card.classList.add("card");

    const cardInner = document.createElement("div");
    const cardFront = document.createElement("div");
    const cardBack = document.createElement("div");
    cardFront.classList.add("card-front");
    cardBack.classList.add("card-back");

    cardInner.appendChild(cardFront);
    cardInner.appendChild(cardBack);
    card.appendChild(cardInner);

    return card;
  }
};
```

# 極簡化
最後就是將整個程式庫的邏輯盡量簡化，然後將看起來適合塞進 WebAssembly 塞進 WebAssembly 就大功告成了。

## 邏輯簡化
實際上我在上面就有用到一些簡化邏輯的例子了。簡化邏輯的手段不是完全純手搓的，有用到 AI 輔助啦。

```js
// Given n is an integer between 0 to 63
if ((n & 15) > 11) n++;
// else do nothing.
// 上述規則可以簡化成：
n += (n & 12) === 12;
```

```js
// Given n is an integer between 0 to 63
if ((n & 48) == 32) n += 16;
else if ((n & 48) == 48) n -= 16;
// If both conditions do not meet, do nothing.
// 上述兩條規則可以簡化成：
n ^= (n & 32) >> 1;
```

有些整數除法也可以簡化，比如：
```js
[Math.floor(i / 13), i % 13];
// 可以簡化成：
[i * 5042 >>> 16, (i * 5042 & 0xffff) * 13 >>> 16];
```

```js
const a = new Uint32Array(52);
for (let i = 1; i < 52; i++) {
  a[i] = Math.ceiling(65536 / i);
};
a.map(e => e.toString(16).padStart(2, "0")).join("\\");
```
