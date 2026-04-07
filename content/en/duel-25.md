---
date: "2026-04-04"
lastmod: ""
draft: true
title: "Duel 25"
description: "Improve and extend Wiwi’s card game."
translationKey: "duel-25"
categories:
  - "Computer Science 電腦科學"
  - "Digital Minimalism 數位極簡主義"
tags:
  - "Playing Cards 撲克牌"
  - "JavaScript"
  - "WebAssembly"
---

> [!IMPORTANT] Want to play right now?
> - Demo: [Click here](/boardgame/duel-25.html)
> - Development docs: [Click here](/docs/boardgame/duel-25.html)
> - Download: [Click here](https://github.com/AngeCI/boardgame)

[Duel 25](https://wiwi.blog/blog/simple-card-battle-game/) is a card game invented by Wiwi at one night before he went to sleep. Below are the original rules ([video version in Chinese](https://youtu.be/3EoWWA1TRi4)):
- Use standard playing cards. Deal 5 cards to each player as their hands, other cards become the draw pile. **Each player has  25 HP at the beginning of the game**.
- During each turn, both players choose a card from their hands and place it face down on the table. After both sides decide, they turn the chosen cards over together. After the cards are revealed, the effects are treated according to the following rules.
- Black suits ♠️, ♣️ are attackers, which deals damage equivalent to the card’s rank to the opponent (A=1、J=11、Q=12、K=13).
- Diamonds ♦️ are counter-attackers, which are only effective when the opponent plays an attacker. Diamonds can avoid any attacks from the opponent, and also impose a damage equivalent to the **diamond card’s rank**. If the opponent doesn’t play an attacker, then **diamonds would have no effect**.
- Hearts ♥️ are healers, which heal HP equivalent to the heart card’s rank for the player who plays it. **HPs are capped at a maximum of 25**, any exceeding HPs would be simply ignored.
- When attacker and healers are played at the same time, **always attack first**. If the attacker is able to drop opponent’s HP into 0 or below, the game is over, and there would no longer be any chance of healing. **The player with no HP loses the game**.
- When a turn is over, and both players still have positive HPs, then both players draw a card to their hands from the draw pile, and begin the next turn.
- If both players die at the same turn, it’s a draw.
- If neither player used up all their HPs when the draw pile is used up, it’s a draw. (By the way, in Wiwi’s original applet, new cards are drawn before determining if the draw pile is used up, that means at the end of turn 21, it will be determined that the draw pile is exhausted and a draw will be declared. The last cards drawn in the final round would be never actually used.)

<small>(By the way there’s also a [strategy](https://shuojen.com/blog/2025/09/23/game/) for the original rule written by Shuo-Jen Huang.)</small>

Aside from Wiwi’s original ruleset, I also came up with an improved ruleset based on opinions on the Internet:
- 不論花色，一律**由出牌點數較小的一方先結算**，讓紅心在生命值低下時仍然有作用。這樣如果雙方的攻擊力都足以殺死對方時，玩家就需要衡量到底要「出剛好能夠扣完對方血量的黑牌，避免對方出更小的黑牌」還是「盡量出數字大的黑牌，避免對方用紅心補血」，避免無腦出大牌。
- **削弱方塊。**原版規則下方塊牌在對手未有攻擊的情況下無任何效果。若雙方都出方塊牌，則把兩張方塊都視作攻擊，雙方各自扣除對方牌面點數的生命值。如果是一張方塊對一張紅心，則紅心正常回血，方塊按照方塊的點數反傷自身。
- 抽牌堆用完後（實戰中其實不常發生），**繼續把手牌打完**。如果手牌打完後仍未有一方生命值用完就當作平手，或者按剩餘生命值多寡決定勝負。

Below are extension rules:
- K 清空手牌重新抽五張。若抽牌堆剩餘牌數不足則把棄牌堆洗勻後補足。
- A 在棄牌堆選一張重進手牌，A 本身在結算原有功能後進入棄牌堆。

# Play now!
- Standalone web page: [Click here](/boardgame/duel-25.html)
- Source code: [Click here](https://github.com/AngeCI/boardgame)
