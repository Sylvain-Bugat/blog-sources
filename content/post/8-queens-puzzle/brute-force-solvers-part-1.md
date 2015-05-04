+++
categories = ["code","8 queens puzzle"]
series = ["8 queens puzzle"]
date = "2015-05-02T12:38:15+02:00"
description = "N queens puzzle brute-force solver part 1"
keywords = ["8 queens puzzle","8 queens","algorithm","N queens puzzle","N queens","puzzle","chessboard","chess","blog"]
title = "8 queens puzzle brute-force solver part 1"
draft = "true"

+++

## Explanations

### Brute-force

Brute-force algorithms are also known as exhaustive algorithms, they consist of testing all possibilities with 8 (or more) queens on a chessboard like that for the first one:

<div id="board" style="width: 400px"></div>

<script>

var position = {
  a8: 'wQ',
  b8: 'wQ',
  c8: 'wQ',
  d8: 'wQ',
  e8: 'wQ',
  f8: 'wQ',
  g8: 'wQ',
  h8: 'wQ'
};
var board = new ChessBoard('board', {
	position: position,
	showNotation: false
});

</script>

In this case a limit of 8 queens is set to end the test and rollback the last placed queen, the next posibility tested will be:

<div id="board2" style="width: 400px"></div>

<script>

var position = {
  a8: 'wQ',
  b8: 'wQ',
  c8: 'wQ',
  d8: 'wQ',
  e8: 'wQ',
  f8: 'wQ',
  g8: 'wQ',
  a7: 'wQ'
};
var board2 = new ChessBoard('board2', {
	position: position,
	showNotation: false
});

</script>

It is clear that this algorithm is uneficient because the second placed queen is already invalid and next positionned and tested queens are just a waste of time. No solution can be found with 2 queens on the 2 first positions for example.

### Uber-brute-force

It's possible to make an algorithm **more uneficient** with no limit of simultaneous placed queens like this chessboard as the first tested "solution":

<div id="board3" style="width: 400px"></div>

<script>

var position = {
  a8: 'wQ', b8: 'wQ', c8: 'wQ', d8: 'wQ', e8: 'wQ', f8: 'wQ', g8: 'wQ', h8: 'wQ',
  a7: 'wQ', b7: 'wQ', c7: 'wQ', d7: 'wQ', e7: 'wQ', f7: 'wQ', g7: 'wQ', h7: 'wQ',
  a6: 'wQ', b6: 'wQ', c6: 'wQ', d6: 'wQ', e6: 'wQ', f6: 'wQ', g6: 'wQ', h6: 'wQ',
  a5: 'wQ', b5: 'wQ', c5: 'wQ', d5: 'wQ', e5: 'wQ', f5: 'wQ', g5: 'wQ', h5: 'wQ',
  a4: 'wQ', b4: 'wQ', c4: 'wQ', d4: 'wQ', e4: 'wQ', f4: 'wQ', g4: 'wQ', h4: 'wQ',
  a3: 'wQ', b3: 'wQ', c3: 'wQ', d3: 'wQ', e3: 'wQ', f3: 'wQ', g3: 'wQ', h3: 'wQ',
  a2: 'wQ', b2: 'wQ', c2: 'wQ', d2: 'wQ', e2: 'wQ', f2: 'wQ', g2: 'wQ', h2: 'wQ',
  a1: 'wQ', b1: 'wQ', c1: 'wQ', d1: 'wQ', e1: 'wQ', f1: 'wQ', g1: 'wQ', h1: 'wQ'
};
var board3 = new ChessBoard('board3', {
	position: position,
	showNotation: false
});

</script>

This tested chessboard is just insane only a brute-force program can test this as a solution. But this algorithm is a floor value to test speed-ups and optimisations.

## Implementations

```java
private void solve(final int x, final int y) {

	// Place a queen on the current position
	chessboard.get(x).set(y, Boolean.TRUE);

	// Test if the chessboard is a solution with exactly N queens
	if (checkSolutionChessboard()) {
		solutionCount++;
		print();
	}
	else {

		//Recursive call to the next position
		final int nextX = (x + 1) % chessboardSize;
		//Switch to the next line
		if (0 == nextX) {

			//End of the chessboard check
			if (y + 1 < chessboardSize) {
				solve(nextX, y + 1);
			}
		}
		else {
			solve(nextX, y);
		}
	}

	// Place the queen on the current position
	chessboard.get(x).set(y, Boolean.FALSE);

	//Recursive call to the next position
	final int nextX = (x + 1) % chessboardSize;
	//Switch to the next line
	if (0 == nextX) {

		//End of the chessboard check
		if (y + 1 < chessboardSize) {
			solve(nextX, y + 1);
		}
	}
	else {
		solve(nextX, y);
	}
}
```


Complete implementation in the file [SlowBruteForceNQueensSolverWithListsNoQueensLimit](https://github.com/Sylvain-Bugat/N-queens-puzzle-solvers/blob/master/src/main/java/com/github/sbugat/nqueens/solvers/bruteforce/SlowBruteForceNQueensSolverWithListsNoQueensLimit.java)

