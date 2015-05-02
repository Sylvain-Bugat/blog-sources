+++
categories = ["8 queens puzzle"]
series = ["8 queens puzzle"]
date = "2015-04-28T23:12:21+02:00"
description = "introduction of the N queens puzzle post series"
keywords = ["8 queens puzzle","8 queens","algorithm","N queens puzzle","N queens","puzzle","chessboard","chess","blog"]
title = "8 queens puzzle introduction"

+++

This post is the first of a article series about the 8/N queens puzzle solving.

## Introduction

The [8 queens on a chessboard](http://en.wikipedia.org/wiki/Eight_queens_puzzle) is a classic puzzle which consists of placing 8 queens on a chessboard with only one queen on each row, column and diagonal, you can try it with this chessboard (errors are not visible yet):

<div id="board" style="width: 400px"></div>
<input type="button" id="clearButton" value="Clear" />

<script>

var board = new ChessBoard('board', {
	draggable: true,
	dropOffBoard: 'trash',
	sparePieces: true,
	showNotation: false
});
$('#clearButton').on('click', board.clear);

</script>

This base Javascript is from [chessboard.js.com](http://chessboardjs.com/) ([GitHub link](https://github.com/oakmac/chessboardjs/)) under the [MIT license](https://github.com/oakmac/chessboardjs/blob/master/LICENSE).
Queen image is from [Pixabay](http://pixabay.com/en/chess-queen-meeple-white-game-36310/) under the [CC0 Public Domain](http://creativecommons.org/publicdomain/zero/1.0/deed).

### Extention to N queens

This puzzle can be extended to N queens on a N x N chessboard and a higher value of N

### Solutions of the puzzle

Known number of solutions for N from 1 to 26 are:

| N | number of solutions |
| ------------- | ----------- |
| 1 | 1 |
| 2 | 0 |
| 3 | 0 |
| 4 | 2 |
| 5 | 10 |
| 6 | 4 |
| 7 | 40 |
| 8 | 92 |
| 9 | 352 |
| 10 | 724 |
| 11 | 2 680 |
| 12 | 14 200 |
| 13 | 73 712 |
| 14 | 365 596 |
| 15 | 2 279 184 |
| 16 | 14 772 512 |
| 17 | 95 815 104 |
| 18 | 666 090 624 |
| 19 | 4 968 057 848 |
| 20 | 39 029 188 884 |
| 21 | 314 666 222 712 |
| 22 | 2 691 008 701 644 |
| 23 | 24 233 937 684 440 |
| 24 | 227 514 171 973 736 |
| 25 | 2 207 893 435 808 352 |
| 26 | 22 317 699 616 364 044 |

Source: sequence [A000170](http://oeis.org/A000170) on [OEIS](http://oeis.org/).

### Resolve the puzzle

This puzzle can be solved with various algorithms. It is quite easy to resolve when N is low and it is a good pratice for testing various algorithms and optimisations.

Next posts will explain greedy and back-tracking algorithms and possible optimisations with some benchmarks to compare. All shown code and algorithms are on this [GitHub project](https://github.com/Sylvain-Bugat/N-queens-puzzle-solvers) and are under the [MIT license](https://github.com/Sylvain-Bugat/N-queens-puzzle-solvers/blob/master/LICENSE).
