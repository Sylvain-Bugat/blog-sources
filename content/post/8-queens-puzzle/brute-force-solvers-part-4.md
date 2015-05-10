+++
categories = ["code","8 queens puzzle"]
series = ["8 queens puzzle"]
date = "2015-05-10T00:18:54+02:00"
description = "N queens puzzle brute-force solvers part 4"
keywords = ["8 queens puzzle","8 queens","algorithm","N queens puzzle","N queens","puzzle","chessboard","chess","blog"]
title = "8 queens puzzle brute-force solvers part 4"
draft = true

+++

## One queen per line

### Explaination

One one queen can be placed per line, so when a queen is placed on any line it's useless to put one or more queens on the remaining position of this line.

For example with this chessboard, it's useless to put a queen on the last 7 positions:

<div id="board" style="width: 400px"></div>

<script>

var position = {
  a8: 'wQ'
};
var board = new ChessBoard('board', {
	position: position,
	showNotation: false
});

</script>

### Implementation

This is the previous implementation with a two dimensional array and a next line skip after placing a queen on a line:

```java
/** Chessboard represented by a 2 dimensional array. */
private boolean[][] chessboard;
/** Array to count queens on each column. */
private int[] columnCounts;
/** Array to count queens on ascending diagonals, diagonal number = x + y. */
private int[] ascendingDiagonalCounts;
/** Array to count queens on descending diagonals, diagonal number = x + chess board size - 1 - y. */
private int[] descendingDiagonalCounts;

/** Current number of placedQueens */
private int placedQueens;
	
private void solve(final int x, final int y) {

	// Put a queen on the current position
	chessboard[y][x] = true;
	columnCounts[x]++;
	final int ascendingDiagnonal = x + y;
	final int descendingDiagnonal = x + chessboardSize - 1 - y;
	ascendingDiagonalCounts[ascendingDiagnonal]++;
	descendingDiagonalCounts[descendingDiagnonal]++;
	placedQueens++;

	// All queens are sets then a solution may be present
	if (placedQueens >= chessboardSize) {
		if (checkSolutionChessboard()) {
			solutionCount++;
			print();
		}
	}
	else {

		//End of chessboard check
		if (y + 1 < chessboardSize) {
			//Go to the next line
			solve(0, y + 1);
		}
	}

	// Remove the placed queen
	placedQueens--;
	ascendingDiagonalCounts[ascendingDiagnonal]--;
	descendingDiagonalCounts[descendingDiagnonal]--;
	columnCounts[x]--;
	chessboard[y][x] = false;

	//End of line check
	if (x + 1 < chessboardSize) {
		//Go to the next position on the line
		solve(x + 1, y);
	}
}
```
The complete implementation is in this source file [BruteForceNQueensSolverOneQueenPerLine](https://github.com/Sylvain-Bugat/N-queens-puzzle-solvers/blob/master/src/main/java/com/github/sbugat/nqueens/solvers/bruteforce/BruteForceNQueensSolverOneQueenPerLine.java).

### Benchmarks

These benchmarks are done on a [Core i5 2500K](http://ark.intel.com/products/52210/Intel-Core-i5-2500K-Processor-6M-Cache-up-to-3_70-GHz):

| chessboard size | execution time | number of runs |
| ------------- | ----------- | ----------- |
| 4 | 6,03 µs | 5000 |
| 5 | 61,93 µs | 5000 |
| 6 | 997,29 µs | 500 |
| 7 | 22,05 ms | 50 |
| 8 | 404,07 ms | 50 |
| 9 | 9,32 s | 5 |
| 10 | too long... |
On 10x10 chessboard, the needed time to count all solutions is very long!


## Reduced recursion

### Explaination

In the N-queens-puzzle, the number of currently placed queens per column, line and diagonals can stored in arrays variables, when a queen is put on the chessboard theses variables are incremented at the correct offsets and when a queen is removed theses variables must be decremented at the same offsets.

### Implementation

This is the previous implementation with 4 new constraits for lines, columns and both diagnonals:

```java
/** Chessboard represented by a 2 dimensional array. */
private boolean[][] chessboard;
/** Array to count queens on each column. */
private int[] columnCounts;
/** Array to count queens on ascending diagonals, diagonal number = x + y. */
private int[] ascendingDiagonalCounts;
/** Array to count queens on descending diagonals, diagonal number = x + chess board size - 1 - y. */
private int[] descendingDiagonalCounts;

private void solve(final int y) {

	for (int x = 0; x < chessboardSize; x++) {

		// Put a queen on the current position
		chessboard[y][x] = true;
		columnCounts[x]++;
		final int ascendingDiagnonal = x + y;
		final int descendingDiagnonal = x + chessboardSize - 1 - y;
		ascendingDiagonalCounts[ascendingDiagnonal]++;
		descendingDiagonalCounts[descendingDiagnonal]++;

		// Last line: all queens are sets then a solution may be present
		if (y + 1 >= chessboardSize) {
			if (checkSolutionChessboard()) {
				solutionCount++;
				print();
			}
		}
		else {
			// Go to the next line
			solve(y + 1);
		}

		// Remove the placed queen
		ascendingDiagonalCounts[ascendingDiagnonal]--;
		descendingDiagonalCounts[descendingDiagnonal]--;
		columnCounts[x]--;
		chessboard[y][x] = false;
	}
}
```
The complete implementation is in this source file [BruteForceNQueensSolverReducedRecursion](https://github.com/Sylvain-Bugat/N-queens-puzzle-solvers/blob/master/src/main/java/com/github/sbugat/nqueens/solvers/bruteforce/BruteForceNQueensSolverReducedRecursion.java).

### Benchmarks

These benchmarks are done on a [Core i5 2500K](http://ark.intel.com/products/52210/Intel-Core-i5-2500K-Processor-6M-Cache-up-to-3_70-GHz):

| chessboard size | execution time | number of runs |
| ------------- | ----------- | ----------- |
| 4 | 6,48 µs | 5000 |
| 5 | 60,74 µs | 5000 |
| 6 | 850,48 µs | 5000 |
| 7 | 18,08 ms | 500 |
| 8 | 323,31 ms | 50 |
| 9 | 8,79 s | 5 |
| 10 | too long... |
On 10x10 chessboard, the needed time to count all solutions is very long!

## Next optimisations?

Other optimisations will be tested in the part 5, stay tuned of go to the [GitHub project](https://github.com/Sylvain-Bugat/N-queens-puzzle-solvers) to have some algorithms preview!
