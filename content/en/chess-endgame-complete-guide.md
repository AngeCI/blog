---
date: "2026-03-09T19:18:40+00:00"
title: "Chess Endgame Complete Guide"
translationKey: "chess-endgame-complete-guide"
categories:
  - "Chess 國際象棋"
  - "Notes 筆記"
---

> This page is still **work in progress**, stay focus for **continuous upcoming updates** on this page!

This page is intended to be used as notes, pardon me for the somewhat disorganized passage. FEN strings appear on this page are planned to be made into an interactive web gadget at a later stage, in order to make the passage more convenient to read.

# Bare king endgames
## King & queen vs bare king
After forcing the defending king to the edge of the board, the queen should keep a 4 × 2 distance (a knight move stretched by one square) from the opponent’s king until both kings are close enough, to prevent stalemate.

## King & rook vs bare king
If the rook is attacked by the defending king and cannot proceed, move the rook along the same file or rank that can restrict the opponent’s king’s movement as a waiting move.

## King & two bishops vs bare king
- First force the defending king to the edge of the board.
- Arrange the attacking pieces as a “bishop-bishop-king” shape, with one of the bishop and the defending king forming a shape similar to opposition.
- W manoeuvre the bishops to force the defending king to the corner. If the defending king touches a bishop, move the attacking king to pretect the bishop.
- After the defending king is trapped on the corner, make good use of middle bishop’s waiting move to make the defending king be only able to move back and forth between two squares at the edge of the board.

> FEN string: k7/8/BBK5/8/8/8/8/8 w - - 0 1
>
> 1. Bg1 Kb8 2. Kb6 Ka8 3. Bb7 Kb8 4. Bg2#

## King & bishop & knight vs bare king
(Work in progress)

# King & pawn endgames
Many king & pawn endgames have only one correct solution, with only a single inaccuracy often leads to a draw, or even reverse the position.

## King & single pawn vs bare king
The attacking side can ensure a win if at least any two of the following conditions are met:
- The attacking king is in front of the pawn;
- The attacking king has the opposition;
- The attacking king is on the sixth rank.

If the attacking sides pawn is on a or h-file, it would be very hard to win if the defending king ever get in front of the pawn. Exception exists as follow:

> FEN string: 5k2/8/6KP/8/8/8/8/8 w - - 0 1
>
> White wins by playing h7 first.

# Rook & pawn endgames
In the situation of a single rook versus a single pawn (KRKP), the pawn side can draw the game if they can prevent the opponent’s king from approaching their pawn. The rook side should try to approach the pawn with their king, as well as watching the pawn from behind with the rook.

> FEN string: 3r4/8/1PK2p2/5k2/8/8/8/1K6 b - - 0 1
>
> If black moves first, the only solution is Ke4, shouldering the enemy king.

> FEN string: 1B6/8/4k3/1K6/4r2p/1P6/8/8 w - - 0 1
>
> After exchanging black’s passed pawn with a white bishop, a single rook versus single pawn position forms.
