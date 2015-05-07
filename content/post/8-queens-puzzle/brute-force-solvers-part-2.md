+++
categories = ["code","8 queens puzzle"]
series = ["8 queens puzzle"]
date = "2015-05-06T20:46:15+02:00"
description = "N queens puzzle brute-force solvers part 2"
keywords = ["8 queens puzzle","8 queens","algorithm","N queens puzzle","N queens","puzzle","chessboard","chess","blog"]
title = "8 queens puzzle brute-force solvers part 2"
draft = true

+++

## Adding a queen constraint

### Explaination

[Constraint programming](https://en.wikipedia.org/wiki/Constraint_programming) consists of use some data structure to store some contraints that can be modified and checked faster than recompute it at each algorithm step.

In the N-queens-puzzle, the number of currently placed queens can stored in a variable, when a queen is put on the chessboard this variable must be incremented and when a queen is remove this variable must be decremented.

### Implementation

This is the previous implementation with a queen counter (variable  `placedQueens`)  limit of N placed queens at the same time on the chessboard:
```java
/** Chessboard represented by a list of lists. */
private final List<List<Boolean>> chessboard;
/** Current number of queens on the chessboard. */
private int placedQueens;

private void solve(final int x, final int y) {

	// Put a queen on the current position
	chessboard.get(x).set(y, Boolean.TRUE);
	placedQueens++;

	// All queens are sets then a solution may be present
	if (placedQueens >= chessboardSize) {
		if (checkSolutionChessboard()) {
			solutionCount++;
			print();
		}
	}
	else {

		// Recursive call to the next position
		final int nextX = (x + 1) % chessboardSize;
		// Switch to the next line
		if (0 == nextX) {

			// End of the chessboard check
			if (y + 1 < chessboardSize) {
				solve(nextX, y + 1);
			}
		}
		else {
			solve(nextX, y);
		}
	}

	// Remove the queen on the current position
	placedQueens--;
	chessboard.get(x).set(y, Boolean.FALSE);

	// Recursive call to the next position
	final int nextX = (x + 1) % chessboardSize;
	// Switch to the next line
	if (0 == nextX) {

		// End of the chessboard check
		if (y + 1 < chessboardSize) {
			solve(nextX, y + 1);
		}
	}
	else {
		solve(nextX, y);
	}
}
```
The complete implementation is in this source file [BruteForceNQueensSolverWithLists](https://github.com/Sylvain-Bugat/N-queens-puzzle-solvers/blob/master/src/main/java/com/github/sbugat/nqueens/solvers/bruteforce/BruteForceNQueensSolverWithLists.java).

### Benchmarks

These benchmarks are done on a [Core i5 2500K](http://ark.intel.com/products/52210/Intel-Core-i5-2500K-Processor-6M-Cache-up-to-3_70-GHz):

| N | execution time |
| ------------- | ----------- |
| 4 | 4,46 ms |
| 5 | 6,18 ms |
| 6 | 152,93 ms |
| 7 | 6,63 s |
| 8 | 6.20 m |
| 9 | 6.10 h |
| 10 | too long... |
On 9x9 chessboard, the needed time to count all solutions is very long!

## Use a two-dimentionnal array for the chessboard

### Explaination

The data structure used to represent the chessboard can be changed from a list of lists to a two-dimentionnal array. Data access is faster because it's more direct without any method call, and the stored data can be a primitive type (boolean in this case intead of Boolean object).

### Implementation

This is the previous implementation with an two-dimentionnal array instead of a list of lists:
```java
/** Chessboard represented by a 2 dimentionnal array. */
private final boolean[][] chessboard;
/** Current number of queens on the chessboard. */
private int placedQueens;

private void solve(final int x, final int y) {

	// Put a queen on the current position
	chessboard[x][y] = true;
	placedQueens++;

	// All queens are sets on the chessboard then a solution may be present
	if (placedQueens >= chessboardSize) {
		if (checkSolutionChessboard()) {
			solutionCount++;
			print();
		}
	} else {

		// Recursive call to the next position
		final int nextX = (x + 1) % chessboardSize;
		// Switch to the next line
		if (0 == nextX) {

			// End of the chessboard check
			if (y + 1 < chessboardSize) {
				solve(nextX, y + 1);
			}
		} else {
			solve(nextX, y);
		}
	}

	// Remove the queen on the current position
	placedQueens--;
	chessboard[x][y] = false;

	// Recursive call to the next position
	final int nextX = (x + 1) % chessboardSize;
	// Switch to the next line
	if (0 == nextX) {

		// End of the chessboard check
		if (y + 1 < chessboardSize) {
			solve(nextX, y + 1);
		}
	} else {
		solve(nextX, y);
	}
}
```

The complete implementation is in this source file [BruteForceNQueensSolverArray](https://github.com/Sylvain-Bugat/N-queens-puzzle-solvers/blob/master/src/main/java/com/github/sbugat/nqueens/solvers/bruteforce/BruteForceNQueensSolverArray.java).

### Benchmarks

These benchmarks are done on a [Core i5 2500K](http://ark.intel.com/products/52210/Intel-Core-i5-2500K-Processor-6M-Cache-up-to-3_70-GHz):

| N | execution time |
| ------------- | ----------- |
| 4 |  |
| 5 |  |
| 6 | |
| 7 |  |
| 8 |  |
| 9 |  |
| 10 | too long... |
On 10x10 chessboard, the needed time to count all solutions is very long!

## Next optimisations?

Other optimisations will be tested in the part 3, stay tuned of go to the [GitHub project](https://github.com/Sylvain-Bugat/N-queens-puzzle-solvers) to have some algorithms preview!
