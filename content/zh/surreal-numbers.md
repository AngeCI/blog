---
date: "2026-04-05"
lastmod: ""
draft: true
title: "超實數"
description: ""
translationKey: "surreal-numbers"
categories:
  - "Mathematics 數學"
  - "Game Theory 博弈論"
---

# 定義
## 排序
> x = {x<sub>L</sub> | x<sub>R</sub>} &lt; y = {y<sub>L</sub> | y<sub>R</sub>} **if and only if**:
> - ∄ x<sub>L</sub> ∈ X<sub>L</sub> such that y ≤ x<sub>L</sub>.
> - ∄ y<sub>R</sub> ∈ Y<sub>R</sub> such that y<sub>R</sub> ≤ x.

## 運算
- 加法：L = { u + y | u ∈ L(x) } ∪ { x + v | v ∈ L(y) }, R = { u + y | u ∈ R(x) } ∪ { x + v | v ∈ R(y) }
- 減法：
- 乘法：
- 除法：

# 定理
- 傳遞性（Transitivity）：若 x ≤ y ，且 y ≤ z，則有 x ≤ z。

對於任意一個數 x 都有：
: x ≰ xL
: x ≱ xR
: x ≤ x

- 完全性（）：x ≤ y 和 y ≤ x 至少有一個成立，即 x ≰ y ⇒ y ≤ x。
- 但 x ≥ y 不能推出 x ≰ y！因而不滿足反對稱性（Antisymmetric）。
- 若 x ≤ y ，且 z ≤ w ，則 x + z ≤ y + w
- 若 x + z ≤ y + w ，但是 z ≥ w ，則 x ≤ y
- 交換律（Commutative property）：x + y = y + x
- 結合律（Associative property）：(a + b) + c = a + (b + c)

# 非超實數的遊戲
所有「先手必勝」的遊戲都不是超實數。

對於所有遊戲而言（不只是超實數），都有「若 a &lt; b，則有 -b &lt; -a」。

> ∗ (STAR)
> - 定義：∗ = {0 | 0}
> - 定理：∗ + ∗ = {∗ | ∗} = 0
> - 定理：∗ = -∗
> - 定理：∗ || 0（同時滿足「∗ ≰ 0」及「∗ ≱ 0」）
>
> ↑(UP)與↓(DOWN)
> - 定義：↑ = {0 | ∗} = -↓
> - 定義：↓ = {∗ | 0} = -↑
> - 定理：↑ &gt; 0, ↓ &lt; 0
> - 定理：↑ || ∗, ↓ || ∗
> - 定理：↑∗ = ↑ + ∗ = ↑ - ∗ = {∗, ↑ | ∗ + ∗, ↑} = {↑ | 0}
> - 定理：↑∗ = ∗ || 0
> - 定理：↑ + ↑ = {↑ | ↑ + ∗}
> - 定理：↑ + ↑ &gt; 0, ↑ + ↑ || ∗
> - 定理：{0 | ↑} = {↑↑ | ↑} = ↑ + ↑ + ∗
> - 定理：↑↑∗ &gt; 0
>
> ∗n, n↑與 n↓
> - 定義：∗n = {0, ∗, …, ∗(n - 1) | 0, ∗, …, ∗(n - 1)}
> - 定義：n↑ = n + ↑
> - 定義：n↓ = n + ↓
>
> ±
> - 定義：±1 = {1 | -1}
> - ∀i: -1 ≤ i ≤ 1 ⇒ ±1 || i

# 應用
- [圍棋](/blog/go-and-cgt/)

# 參考
- [Matrix67](https://matrix67.com/blog/archives/6333)
- [Wikibooks](https://en.wikibooks.org/wiki/Surreal_Numbers_and_Games)
