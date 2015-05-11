+++
categories = ["code","8 queens puzzle"]
series = ["8 queens puzzle"]
date = "2015-05-09T00:12:54+02:00"
description = "N queens puzzle brute-force solvers part 3"
keywords = ["8 queens puzzle","8 queens","algorithm","N queens puzzle","N queens","puzzle","chessboard","chess","blog"]
title = "8 queens puzzle brute-force solvers part 3"

+++


## Use a one-dimensional array for the chessboard

### Explaination

The data structure used to represent the chessboard can be changed from a two-dimensional array to a one-dimensional array. Recursive calls are faster because all N x N positions are sequentially acceded.

### Implementation

This is the previous implementation with a one-dimention array to represent the chessboard, his size is equal to N x N:

```java
/** Chessboard with only one dimension with all lines. */
private final boolean[] chessboard;
/** Current number of placedQueens */
private int placedQueens;

private void solve(final int position) {

	// Put a queen on the current position
	chessboard[position] = true;
	placedQueens++;

	// All queens are sets then a solution may be present
	if (placedQueens >= chessboardSize) {
		if (checkSolutionChessboard()) {
			solutionCount++;
			print();
		}
	}
	else {

		// End of the chessboard check
		if (position + 1 < chessboard.length) {
			solve(position + 1);
		}
	}

	// Remove the queen on the current position
	placedQueens--;
	chessboard[position] = false;

	// End of the chessboard check
	if (position + 1 < chessboard.length) {
		solve(position + 1);
	}
}
```
The complete implementation is in this source file [BruteForceNQueensSolverOneDimensionArray](https://github.com/Sylvain-Bugat/N-queens-puzzle-solvers/blob/master/src/main/java/com/github/sbugat/nqueens/solvers/bruteforce/BruteForceNQueensSolverOneDimensionArray.java).

### Benchmarks

These benchmarks are done on a [Core i5 2500K](http://ark.intel.com/products/52210/Intel-Core-i5-2500K-Processor-6M-Cache-up-to-3_70-GHz):

| chessboard size | execution time | number of runs |
| ------------- | ----------- | ----------- |
| 4 | 67.70 µs | 5000 |
| 5 | 1.82 ms | 5000 |
| 6 | 67.17 ms | 500 |
| 7 | 3.11 s | 50 |
| 8 | 2.42 m | 5 |
| 9 | 3h02m | 1 |
| 10 | too long... |
On 9x9 chessboard, the needed time to count all solutions is very long!


## Add grid constraints

### Explaination

In the N-queens-puzzle, the number of currently placed queens per column, line and diagonals can stored in arrays variables, when a queen is put on the chessboard theses variables are incremented at the correct offsets and when a queen is removed theses variables must be decremented at the same offsets.

### Implementation

This is the previous implementation with 4 new constraints for lines, columns and both diagnonals:

```java
/** Chessboard with only one dimension with all lines. */
private boolean[] chessboard;
/** Array to count queens on each column. */
private int[] columnCounts;
/** Array to count queens on each line. */
private int[] lineCounts;
/** Array to count queens on ascending diagonals, diagonal number = x + y. */
private int[] ascendingDiagonalCounts;
/** Array to count queens on descending diagonals, diagonal number = x + chessboard size - 1 - y. */
private int[] descendingDiagonalCounts;

private void solve(final int position) {

	// Recalculate X and Y coordinates
	final int y = position / chessboardSize;
	final int x = position % chessboardSize;

	// Put a queen on the current position
	chessboard[position] = true;
	lineCounts[y]++;
	columnCounts[x]++;
	final int ascendingDiagonal = x + y;
	final int descendingDiagnonal = x + chessboardSize - 1 - y;
	ascendingDiagonalCounts[ascendingDiagonal]++;
	descendingDiagonalCounts[descendingDiagonal]++;
	placedQueens++;

	// All queens are sets then a solution may be present
	if (placedQueens >= chessboardSize) {
		if (checkSolutionChessboard()) {
			solutionCount++;
			print();
		}
	}
	else {

		// End of the chessboard check
		if (position + 1 < chessboard.length) {
			solve(position + 1);
		}
	}

	// Remove the queen on the current position
	placedQueens--;
	ascendingDiagonalCounts[ascendingDiagonal]--;
	descendingDiagonalCounts[descendingDiagonal]--;
	lineCounts[y]--;
	columnCounts[x]--;
	chessboard[position] = false;

	// End of the chessboard check
	if (position + 1 < chessboard.length) {
		solve(position + 1);
	}
}
```
The complete implementation is in this source file [BruteForceNQueensSolverGridConstraits](https://github.com/Sylvain-Bugat/N-queens-puzzle-solvers/blob/master/src/main/java/com/github/sbugat/nqueens/solvers/bruteforce/BruteForceNQueensSolverGridConstraits.java).

### Benchmarks

These benchmarks are done on a [Core i5 2500K](http://ark.intel.com/products/52210/Intel-Core-i5-2500K-Processor-6M-Cache-up-to-3_70-GHz):

| chessboard size | execution time | number of runs |
| ------------- | ----------- | ----------- |
| 4 | 55,98 µs | 5000 |
| 5 | 1,75 ms | 5000 |
| 6 | 54,57 ms | 500 |
| 7 | 2,37 s | 50 |
| 8 | 1.54 m | 5 |
| 9 | 2.05 h | 1 |
| 10 | too long... |
On 9x9 chessboard, the needed time to count all solutions is very long!

## Next optimisations?

Other optimisations will be tested in the part 4, stay tuned of go to the [GitHub project](https://github.com/Sylvain-Bugat/N-queens-puzzle-solvers) to have some algorithms preview!
