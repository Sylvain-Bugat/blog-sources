+++
categories = ["code","8 queens puzzle"]
series = ["8 queens puzzle"]
date = "2015-05-09T00:12:54+02:00"
description = "N queens puzzle brute-force solvers part 2"
keywords = ["8 queens puzzle","8 queens","algorithm","N queens puzzle","N queens","puzzle","chessboard","chess","blog"]
title = "8 queens puzzle brute-force solvers part 3"
draft = true

+++


## Use a one-dimentionnal array for the chessboard

### Explaination

The data structure used to represent the chessboard can be changed from a two-dimentionnal array to a one-dimentionnal array. Recursive calls are faster because there all N x N positions are sequentially acceded.

### Implementation

This is the previous implementation with a one-dimention array to represent the chessboard, his size is equal to N x N:

```java
/** Chessboard with only one dimension with all lines. */
private final boolean[] chessboard;
/** Current number of placedQueens */
private int placedQueens;

private void solve(final int i) {

	// Put a queen on the current position
	chessboard[i] = true;
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
		if (i + 1 < chessboard.length) {
			solve(i + 1);
		}
	}

	// Remove the queen on the current position
	placedQueens--;
	chessboard[i] = false;

	// End of the chessboard check
	if (i + 1 < chessboard.length) {
		solve(i + 1);
	}
}
```
The complete implementation is in this source file [BruteForceNQueensSolverOneDimension](https://github.com/Sylvain-Bugat/N-queens-puzzle-solvers/blob/master/src/main/java/com/github/sbugat/nqueens/solvers/bruteforce/BruteForceNQueensSolverOneDimension.java).

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


## Next optimisations?

Other optimisations will be tested in the part 4, stay tuned of go to the [GitHub project](https://github.com/Sylvain-Bugat/N-queens-puzzle-solvers) to have some algorithms preview!
