---
date: "2026-06-15"
type: "post"
draft: true
title: "國際象棋程式庫開發日誌（之一）"
description: "一個極簡主義的 WebAssembly + JavaScript 國際象棋程式庫。"
translationKey: "chess-library-1"
categories:
  - "Chess 國際象棋"
  - "Computer Science 電腦科學"
  - "Digital Minimalism 數位極簡主義"
tags:
  - "JavaScript"
  - "WebAssembly"
---

> [!IMPORTANT] 想立刻體驗嗎？
> - 範例：[點我](/boardgame/chess.html)
> - 開發文檔：[點我](/docs/boardgame/chess.html)
> - 下載連結：[點我](https://github.com/AngeCI/boardgame)

# 目標
- [ ] 做一個能夠完整處理國際象棋規則的程式庫，儘可能做得越輕量越好；
- [ ] 做一個可以檢視、匯入、匯出棋局，以及調用外部象棋引擎實時分析的 web app；
- [ ] 將上述整個工具打包成一個可以獨立運行的離線 Android app；
- [ ] 做一個 UCI 協議引擎框架；
- [ ] 做成互動的網頁內嵌 gadget，拿給這個網站的[一些頁面](/blog/zh/categories/chess-%E5%9C%8B%E9%9A%9B%E8%B1%A1%E6%A3%8B/)、以及開放給其他的網站使用。

# 棋盤與棋子的數據結構
許多象棋程式出於運行效率的考量，對於棋盤和棋子的存儲其實都是冗餘的。程式可以同時有一個存儲棋盤上存在的棋子的陣列（下文稱為「棋盤陣列」）、以及存儲給定種類的棋子的所在位置的陣列（下文「棋子陣列」或「bitboard[^1]」）。

透過棋盤陣列，可以很方便地與 FEN 字串相互轉換：

```js
const Board = class {
  #squares = new Uint8Array(64);

  importFromFen(fen) {
    this.#squares.fill(0, 0);
    const pieceTypeTable = {
      "k": Piece.KING,
      "q": Piece.QUEEN,
      "r": Piece.ROOK,
      "n": Piece.KNIGHT,
      "b": Piece.BISHOP,
      "p": Piece.PAWN
    };

    const [board, moveSide, castlingRights, epTarget, fiftyMoveCounter, plyCount] = fen.split(" ");
    let file = 0, rank = 0;
    for (let symbol of board) {
      if (symbol === "/") {
        file = 0;
        rank++;
      } else {
        if (!isNaN(parseInt(symbol))) {
          file += parseInt(symbol);
        } else {
          let pieceColour = symbol.charCodeAt(0) < 96 ? Piece.WHITE : Piece.BLACK;
          let pieceType = pieceTypeTable[symbol.toLowerCase()];
          this.#squares[(rank << 3) + file] = pieceType | pieceColour;
          file++;
        };
      };
    };

    // Handling other fields ...
  };

  getFENString() {
    const pieceTable = " 1234567.KQRNBP..kqrnbp.";
    let output = "";
    let emptySquareCounter = 0;

    for (let i = 0; i < 8; i++) {
      for (let j = 0; j < 8; j++) {
        let piece = this.#squares[(i << 3) + j];
        if (piece !== 0) {
          if (emptySquareCounter > 0) {
            output += emptySquareCounter;
            emptySquareCounter = 0;
          };
          output += pieceTable[piece];
        } else {
          emptySquareCounter++;
        };
      };

        if (emptySquareCounter !== 0) {
          output += emptySquareCounter;
          emptySquareCounter = 0;
        };

      if (i < 7)
        output += "/";
    };

    // Handling other fields ...

    return `${output} ${this.isWhiteToMove ? "w" : "b"} ${castlingRights} ${epTarget} ${this.fiftyMoveCounter} ${(this.plyCount >> 1) + 1}`;
  };
};
```

Bitboard 是一種特殊的數據結構，每個位元（bit）代表一個棋格，整個棋盤以 64 位表示。Bitboard 能夠在電腦程式層面實施一些非常高效的操作。本程式會分別儲存 12 種不同棋子、所有同色棋子、以及所有非空棋格的 bitboard，合共佔用 120 個位元組（bytes）的記憶體。

# 行棋規則處理
國際象棋的行棋規則實際上較為繁雜，在電腦程式的角度來說其實不太好實現。

## 兵
兵的走法可以直接用 bitboard 的位元運算（bitwise operation）[^2]解決。思路大致上是：

- 讀取棋盤上所有己方兵的位置（表示為一個 64 位元的 bitboard，代碼裏稱為 `sq`）；
- 讀取棋盤上所有有棋子的位置（代碼裏稱為 `allPieces = enemyPieces | friendlyPieces`）；
- 將 `sq` 左移 8 位，計算 `(wP << 8) & ~allPieces`，得到每個兵正前方的空棋格；
- 將 `sq` 左移 16 位，計算 `(wP << 16) & ~allPieces & rank[4]`，得到兵走兩步可以到達的空棋格；
- 接着處理兵吃子的情況。首先讀取棋盤上所有敵棋的位置（代碼裏稱為 `enemyPieces`）；
- 將 `sq` 左移 9 位，計算 `(wP << 9) & ~file[7] & enemyPieces`，得到每個兵左前方有敵棋的棋格；
- 將 `sq` 左移 7 位，計算 `(wP << 7) & ~file[0] & enemyPieces`，得到每個兵右前方有敵棋的棋格；
- 以上算法是針對白方而言的，對於黑方的狀況……。

```wat
(module
  (memory (export "$") 1)
  (func $genPawnMoves (export "a")
    (param $color i32)
    (param $sq i64)
    (param $enemyPieces i64)
    (param $friendlyPieces i64)
    (local i64)
    (result i64)
    (local.set 4 (i64.shl (local.get $wP) (i64.const 8))) ;; store a temporary variable for (wP << 8)

    (i64.and ;; single pawn-push
      (local.get 4)
      (i64.xor (local.get $occupied) (i64.const -1)) ;; i64.not
    )

    (i64.and ;; double pawn-push
      (i64.shl (local.get 4) (i64.const 8))
      (i64.xor (local.get $occupied) (i64.const -1)) ;; i64.not
    )
    (i64.and (i64.const 0xff000000)) ;; rank 4

    (i64.and ;; capture towards left
      (i64.shl (local.get 4) (i64.const 1))
      (i64.const 0xfefefefefefefefe) ;; non-h-file
    )
    (i64.and (local.get $bOcc))

    (i64.and ;; capture towards right
      (i64.shr_u (local.get 4) (local.get 4))
      (i64.const 0x7f7f7f7f7f7f7f7f) ;; non-a-file
    )
    (i64.and (local.get $bOcc))

    i64.and
    i64.and
    i64.and
  )
)
```

## 馬及王
馬的走法採用預先計算好的 bitboard 陣列，這個陣列當中的每一個元素都是一個 bitboard，代表着馬在棋盤上的給定棋格可以走到的每一個位置。

```wat
(module
  (memory (export "$") 1)
  (data (i32.const 0) "\02\03\00\00\00\00\00\00\05\07\00\00\00\00\00\00") ;; KingMoves
  (data (i32.const 512) "\00\04\02\00\00\00\00\00\00\08\05\00\00\00\00\00") ;; KnightMoves
  (func $genKnightMoves (export "b")
    (param $sq i32)
    (param $friendlyPieces i64)
    (result i64)
    (i64.load
      (i32.add (local.get $sq) (i32.const 512))
    )
  )
  (func $genKingMoves (export "c")
    (param $sq i32)
    (param $friendlyPieces i64)
    (result i64)
    (i64.load
      ;; (i32.add (local.get $sq) (i32.const 0))
      (local.get $sq)
    )
  )
)
```

當中馬的着法表可以如此生成：

```js
let mask = new BigUint64Array(64);
for (let i = 0; i < 64; i++) {
  mask[i] = 1n << BigInt(i);
}

function manhattanDistance(sq1, sq2) {
  let x1 = sq1 & 7;
  let x2 = sq2 & 7;
  let y1 = sq1 >> 3;
  let y2 = sq2 >> 3;
  let xDistance = Math.abs(x2 - x1);
  let yDistance = Math.abs(y2 - y1);
  return xDistance + yDistance;
}

// Knight moves
let temp;
let knight = new BigUint64Array(64);
let knightOffset = new Int8Array([-17, -15, -10, -6, 6, 10, 15, 17]);
for (let i = 0; i < 64; i++) {
  temp = 0n;
  for (j = 0; j < 8; j++) {
    if (i + knightOffset[j] >= 0 && i + knightOffset[j] < 64) {
      if (manhattanDistance(i, i + knightOffset[j]) === 3) {
        temp |= mask[i + knightOffset[j]];
      }
    }
  }
  knight[i] = temp;
};

let a = new Uint8Array(knight.buffer);
Array.from(a).map(e => e.toString(16).padStart(2, "0")).join("\\");
```

王的着法表的生成方式類似。

## 車、象及后
這些可以長距離走動的棋種，可以用到一種名叫 [magic bitboard](https://www.chessprogramming.org/Magic_Bitboards) 的技術快速產生着法。

為了直接引用其他人算好的魔術數字，我在計算索引值的時候實際上把整個棋盤橫向反射了一遍。也許如果我要重算魔術數字的話，可以把這一步也減省了。

`rookAttacks` 和 `bishopAttacks` 兩個陣列，到底是要每次程式初始化的時候都重算一遍，還是算好之後直接寫死在代碼裏頭，我還沒拿定主意。

```wat
(module
  (memory (export "$") 1)
  (data (i32.const 1024) "\a0\b4\08\40\81\00\80\06\fd\df\ef\f7\df\ff\bf\ff") ;; RookMagics
  (data (i32.const 1536) "\ff\5b\e4\fb\94\bb\1e\e5\7f\fe\8f\ed\67\f5\b9\c7") ;; BishopMagics
  (data (i32.const 2048) "4444444455565565566655655655666546555565456655655666556545555554") ;; RookShifts
  (data (i32.const 2112) ":<;;;;<:<;;;;;;<;;9999;;;;9779;;;;9779;;;;9999;;<<;;;;<<:<;;;;;:") ;; BishopShifts
  (func $genSliderMoves (export "d")
    (param $type i32) ;; Bishop = 3, Rook = 4, Queen = 5
    (param $sq i32)
    (param $allPieces i64)
    (param $friendlyPieces i64)
    (result i64)

    (if (i32.and (local.get $type) (i32.const 4)) ;; rook
      (then
        (i64.xor ;; rook move mask
          (i64.shr_u ;; file
            (i64.const 0x8080808080808080)
            (i64.extend_i32_u (i32.and (local.get $sq) (i32.const 7)))
          )
          (i64.shr_u ;; rank
            (i64.const 0xff00000000000000)
            (i64.extend_i32_u
              (i32.shl
                (i32.shr_u (local.get $sq) (i32.const 3))
                (i32.const 3)
              )
            )
          )
        )
        (i64.and (i64.const 0x007e7e7e7e7e7e00))
      )
    )
    (if (i32.and (local.get $type) (i32.const 1)) ;; bishop
      (then
      )
    )
  )
}
```

```js
let file = new BigUint64Array([
  0x8080808080808080n, // a-file
  0x4040404040404040n, // b-file
  0x2020202020202020n, // c-file
  0x1010101010101010n, // d-file
  0x0808080808080808n, // e-file
  0x0404040404040404n, // f-file
  0x0202020202020202n, // g-file
  0x0101010101010101n  // h-file
]);
let rank = new BigUint64Array([
  0xff00000000000000n, // rank 8
  0x00ff000000000000n, // rank 7
  0x0000ff0000000000n, // rank 6
  0x000000ff00000000n, // rank 5
  0x00000000ff000000n, // rank 4
  0x0000000000ff0000n, // rank 3
  0x000000000000ff00n, // rank 2
  0x00000000000000ffn  // rank 1
]);
let diag1 = new BigUint64Array([
  0x0100000000000000n, // h8
  0x0201000000000000n, // g8-h7
  0x0402010000000000n, // f8-h6
  0x0804020100000000n, // e8-h5
  0x1008040201000000n, // d8-h4
  0x2010080402010000n, // c8-h3
  0x4020100804020100n, // b8-h2
  0x8040201008040201n, // a8-h1
  0x0080402010080402n, // a7-g1
  0x0000804020100804n, // a6-f1
  0x0000008040201008n, // a5-e1
  0x0000000080402010n, // a4-d1
  0x0000000000804020n, // a3-c1
  0x0000000000008040n, // a2-b1
  0x0000000000000080n  // a1
]);
let diag2 = new BigUint64Array([
  0x8000000000000000n, // a8
  0x4080000000000000n, // a7-b8
  0x2040800000000000n, // a6-c8
  0x1020408000000000n, // a5-d8
  0x0810204080000000n, // a4-e8
  0x0408102040800000n, // a3-f8
  0x0204081020408000n, // a2-g8
  0x0102040810204080n, // a1-h8
  0x0001020408102040n, // b1-h7
  0x0000010204081020n, // c1-h6
  0x0000000102040810n, // d1-h5
  0x0000000001020408n, // e1-h4
  0x0000000000010204n, // f1-h3
  0x0000000000000102n, // g1-h2
  0x0000000000000001n  // h1
]);

function horizontalFlip(n) {
  n = ((n >> 1n) & 0x5555555555555555n) | ((n & 0x5555555555555555n) << 1n);
  n = ((n >> 2n) & 0x3333333333333333n) | ((n & 0x3333333333333333n) << 2n);
  n = ((n >> 4n) & 0x0f0f0f0f0f0f0f0fn) | ((n & 0x0f0f0f0f0f0f0f0fn) << 4n);

  return n;
}

// let rookMask = (n) => (file[n & 7] ^ rank[n >> 3]) & 0x07e7e7e7e7e7e00n;
let rookMask = (n) => (0x8080808080808080n >> BigInt(n & 7) ^ 0xff00000000000000n >> BigInt(n >> 3 << 3)) & 0x007e7e7e7e7e7e00n;
let bishopMask = (n) => (diag1[(n & 7 ^ 7) + (n >> 3)] ^ diag2[(n & 7) + (n >> 3)]) & 0x007e7e7e7e7e7e00n;

function createAllBlockerBitboards(movementMask) {
  let moveSquareIndices = [];
  for (let i = 0n; i < 64n; i++) {
    if (movementMask >> i & 1n)
      moveSquareIndices.push(Number(i));
  };

  let numPatterns = 1 << moveSquareIndices.length;
  let blockerBitboards = new BigUint64Array(numPatterns);

  for (let patternIndex = 0; patternIndex < numPatterns; patternIndex++) {
    for (let bitIndex = 0; bitIndex < moveSquareIndices.length; bitIndex++) {
      let bit = (patternIndex >> bitIndex) & 1;
      blockerBitboards[patternIndex] |= BigInt(bit) << BigInt(moveSquareIndices[bitIndex]);
    };
  };

  return blockerBitboards;
}

function legalMoveBitboardFromBlockers(startSq, blockerBitboard, ortho) {
  let bitboard = 0n;
  let rookDirections = new Uint8Array([-16, -1, 1, 16]);
  let bishopDirections = new Uint8Array([-17, -15, 15, 17]);

  let directions = ortho ? rookDirections : bishopDirections;
  // Convert coordinate to 0x88 system for easier boundary checking
  let startCoord = startSq + (startSq & ~7);

  for (let dir of directions) {
    for (let step = 1; step < 8; step++) {
      let coord = startCoord + dir * step;

      if ((coord & 0x88) === 0) { // within boundary?
        coord = (coord + (coord & 7)) >> 1 ^ 63; // Convert back to the original coordinate system
        bitboard |= 1n << BigInt(coord);
        // BitboardHelper.setSquare(coord);

        if (blockerBitboard >> BigInt(coord) & 1n) // blocked by an enemy piece, can't move any further
          break;
      } else break; // reached the edge of the board
    };
  };

  return bitboard;
}

function createLookupTables() {
  let rookAttacks = [];
  let bishopAttacks = [];

  let createTable = function (square, isRook, magic, leftShift) {
    let table = new BigUint64Array(1 << 64 - leftShift);

    let blockerPatterns = createAllBlockerBitboards(isRook ? rookMask(square) : bishopMask(square));

    for (let blockerBitboard of blockerPatterns) {
      let legalMoveBitboard = legalMoveBitboardFromBlockers(square, blockerBitboard, isRook);
      blockerBitboard = horizontalFlip(blockerBitboard);

      if (isRook)
        table[(blockerBitboard * magic & 0xffffffffffffffffn) >> BigInt(leftShift)] = legalMoveBitboard;
      else
        table[(blockerBitboard * magic & 0xffffffffffffffffn) >> BigInt(leftShift)] = legalMoveBitboard;
    };

    return table;
  };

  for (let startSq = 0; startSq < 64; startSq++) {
    createTable(startSq, true, rookMagics[startSq], rookShifts[startSq]);
    createTable(startSq, false, bishopMagics[startSq], bishopShifts[startSq]);
  };

  return [rookAttacks, bishopAttacks];
  // return [new BigUint64Array(rookAttacks), new BigUint64Array(bishopAttacks)];
}

function exportLookupTables(table) {
  let blob = new Blob([new BigUint64Array(table)]);
  const a = document.createElement("a");
  a.href = URL.createObjectURL(blob);
  a.addEventListener("click", () => {
    setTimeout(() => URL.revokeObjectURL(a.href), 30000);
  });
  a.click();
}

function getSliderAttacks(square, blockers, ortho) {
  if (ortho)
    return rookAttacks[square + ((blockers & rookMask(square)) * rookMagics[square] >> rookShifts[square] << 6)];
  else
    return bishopAttacks[square + ((blockers & bishopMask(square)) * bishopMagics[square] >> bishopShifts[square] << 6)];
}

function genSliderMoves(startSq, allPiecesBitboard, friendlyPiecesBitboard) {
  let moveList = [];

  let key = (allPiecesBitboard & rookMask(startSq)) * rookMagics[startSq] >> rookShifts[startSq];
  let movesBitboard = rookAttacks[startSq + (key << 6)] & ~friendlyPiecesBitboard;

  while (movesBitboard) {
    moveList.push(BitboardHelper.PopLSB(movesBitboard));
  };

  return moveList;
}
```

## 整理所有着法
```js
moves.sort((a, b) => (a & Move.START_SQ_MASK) - (b & Move.START_SQ_MASK));
```

## 將軍判斷
前人的實踐證明，在生成着法時延後判斷將軍，通常有助於提升程式的整體效能。一個簡明的將軍判斷手段為：「如果這枚王是別的棋種，它能否立即吃到同種類的敵棋？」若上述問題的答案為「是」，則王與該敵棋構成將軍。

## 吃過路兵
吃過路兵只能發生第六行（白方）或第三行（黑方）。

```js
// 3. Verify the pawn exists on the square "behind" the target
// If target is e6 (rank index 5), the pawn is on e5 (rank index 4).
// If target is e3 (rank index 2), the pawn is on e4 (rank index 3).
const targetSq = algebraicToSqNumber(epTarget) ^ 56;
const pawnSq = targetSq + (this.isWhiteToMove ? 8 : -8);
const piece = this.#squares[pawnSq];
const enemyColor = this.isWhiteToMove ? 16 : 8;
if ((piece & Piece.TYPE_MASK) !== Piece.PAWN || (piece & Piece.COLOR_MASK) !== enemyColor) {
  throw new TypeError(`No enemy pawn positioned to allow en passant at ${epTarget}.`);
};
```

## 王車易位
判斷王的走法是否合符王車易位的特徵：

```js
const m = move & ~0x100;
if (m === 132 || m === 3772) { // move === 132 || move === 388 || move === 3772 || move === 4028
  move |= 0x2000;
};
```

```wat
(module
  (func $castling (export "")
    (param $move i32)
    (result i32)
    (i32.and (local.get $move) (i32.const 0xffffefff))
    (i32.or
      (i32.eq (local.tee $move) (i32.const 132))
      (i32.eq (local.get $move) (i32.const 3772))
    )
  )
)
```

王車易位的可行性要進一步結合棋盤狀態來判斷，FEN 字串有一個專門的欄位用來記錄這些狀態。

[^1]: 有中文資料譯為「位棋盤」。
[^2]: 「位操作」是支語，但可能沒有確定的香港中文用語。
