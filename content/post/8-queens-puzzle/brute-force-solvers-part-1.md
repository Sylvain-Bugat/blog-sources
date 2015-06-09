+++
categories = ["code","8 queens puzzle"]
series = ["8 queens puzzle"]
date = "2015-05-02T12:38:15+02:00"
description = "N queens puzzle brute-force solvers part 1"
keywords = ["8 queens puzzle","8 queens","algorithm","N queens puzzle","N queens","puzzle","chessboard","chess","blog"]
title = "8 queens puzzle brute-force solvers part 1"

+++

## Brute-force

### Explanation

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

## Uber-brute-force

### Explanation

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

This tested chessboard is just insane, only a brute-force program can test this as a solution. But this algorithm is a floor value to test speed-ups and optimisations.

### Implementation

This is the slowest implementation I have done to resolve this puzzle:

```java
/** Chessboard represented by a list of lists. */
private final List<List<Boolean>> chessboard;

private void solve(final int x, final int y) {

	// Put a queen on the current position
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

	// Remove the queen on the current position
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

The complete implementation is in this source file [SlowBruteForceNQueensSolverWithListsNoQueensLimit](https://github.com/Sylvain-Bugat/N-queens-puzzle-solvers/blob/master/src/main/java/com/github/sbugat/nqueens/solvers/bruteforce/SlowBruteForceNQueensSolverWithListsNoQueensLimit.java).

This implementation has a lot of weakness:

* The algorithm don't stop after placing N queens
* The solution is checked by analyzing the full chessboard after placing each queen
* The chessboard is represented by list of lists

The first point is a complexity overkill because it greatly increases the number of moves required to test all possible solutions on the NxN chessboard. It's a waste of time to test any combination with over N queens, a back-track is needed to test another untested combination.

### Benchmarks

These benchmarks are done on a [Core i5 2500K](http://ark.intel.com/products/52210/Intel-Core-i5-2500K-Processor-6M-Cache-up-to-3_70-GHz):

| chessboard size | execution time |
| ------------- | ----------- |
| 4 | 4.57 ms |
| 5 | 2.47 s |
| 6 | 2h09m |
| 7 | too long... |

Even on 6x6 chessboard, the time needed to count all solutions is very long!

## Brute-force

### Implementation

This is just the previous implementation with a maximum limit of N placed queens at the same time on the chessboard:

```java
/** Chessboard represented by a list of lists. */
private final List<List<Boolean>> chessboard;

private void solve(final int x, final int y) {

	// Put a queen on the current position
	chessboard.get(x).set(y, Boolean.TRUE);

	// All queens are sets then a solution may be present
	if (getPlacedQueens() >= chessboardSize) {
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

The complete implementation is in this source file [SlowBruteForceNQueensSolverWithLists](https://github.com/Sylvain-Bugat/N-queens-puzzle-solvers/blob/master/src/main/java/com/github/sbugat/nqueens/solvers/bruteforce/SlowBruteForceNQueensSolverWithLists.java).

This implementation is still very unefficient but is a lot faster because a lot of dead combination are not tested. This optimisation is the first little step to implement a back-tracking algorithm.

### Benchmarks

These benchmarks are done on a [Core i5 2500K](http://ark.intel.com/products/52210/Intel-Core-i5-2500K-Processor-6M-Cache-up-to-3_70-GHz):

| chessboard size | execution time |
| ------------- | ----------- |
| 4 | 3.50 ms |
| 5 | 21.95 ms |
| 6 | 432.14 ms |
| 7 | 20.90 s |
| 8 | 19.50 m |
| 9 | too long... |

On 8x8 chessboard, the time needed to count all solutions is quite long!

## Algorithms comparisons

Comparison of theses 2 algorithms:

<div class="panel panel-default tab-box">
	<div class="panel-heading">
		<h3 class="panel-title">
			<i class="glyphicon glyphicon-stats"></i>Algorithms comparison
		</h3>
		<ul class="nav nav-tabs">
			<li class="active">
				<a href="#queenPlacementsTab" data-toggle="tab" data-identifier="queenPlacementsGraph">moves</a>
			</li>
			<li>
				<a href="#methodCallsTab" data-toggle="tab" data-identifier="methodCallsGraph">method calls</a>
			</li>
			<li>
				<a href="#squareReadsTab" data-toggle="tab" data-identifier="squareReadsGraph">reads</a>
			</li>
			<li>
				<a href="#explicitTestsTab" data-toggle="tab" data-identifier="explicitTestsGraph">tests</a>
			</li>
			<li>
				<a href="#implicitTestsTab" data-toggle="tab" data-identifier="implicitTestsGraph">loop tests</a>
			</li>
		</ul>
	</div>
	<div class="panel-body">
		<div class="tab-content">
			<div id="queenPlacementsTab" class="tab-pane active">
				<div class="row">
					<div class="caption">
						Queen placements count
					</div>
					<div id="queenPlacements"></div>
					<div class="legend">
						<span class="label" style="background-color: #A52A2A;">Uber brute-force</span>
						<span class="label" style="background-color: #72A0C1;">List brute-force</span>
					</div>
				</div>
			</div>
			<div id="methodCallsTab" class="tab-pane">
				<div class="row">
					<div class="caption">
						Method calls count
					</div>
					<div id="methodCalls"></div>
					<div class="legend">
						<span class="label" style="background-color: #A52A2A;">Uber brute-force</span>
						<span class="label" style="background-color: #72A0C1;">List brute-force</span>
					</div>
				</div>
			</div>
			<div id="squareReadsTab" class="tab-pane">
				<div class="row">
					<div class="caption">
						Square reads count
					</div>
					<div id="squareReads"></div>
					<div class="legend">
						<span class="label" style="background-color: #A52A2A;">Uber brute-force</span>
						<span class="label" style="background-color: #72A0C1;">List brute-force</span>
					</div>
				</div>
			</div>
			<div id="explicitTestsTab" class="tab-pane">
				<div class="row">
					<div class="caption">
						Explicit tests count
					</div>
					<div id="explicitTests"></div>
					<div class="legend">
						<span class="label" style="background-color: #A52A2A;">Uber brute-force</span>
						<span class="label" style="background-color: #72A0C1;">List brute-force</span>
					</div>
				</div>
			</div>
			<div id="implicitTestsTab" class="tab-pane">
				<div class="row">
					<div class="caption">
						Loop tests count
					</div>
					<div id="implicitTests"></div>
					<div class="legend">
						<span class="label" style="background-color: #A52A2A;">Uber brute-force</span>
						<span class="label" style="background-color: #72A0C1;">List brute-force</span>
					</div>
				</div>
			</div>
		</div>
	</div>
</div>

<script>

$('ul.nav a').on('shown.bs.tab', function (e) {
	var types = $(this).attr("data-identifier");
	var typesArray = types.split(",");
	$.each(typesArray, function (key, value) {
		eval(value + ".redraw()");
		eval(value + ".resizeHandler()");
	})
});

//Data
var data = [
	{"size": "1", "solver1queenPlacements": 0.1,  "solver1methodCalls": 1.3424226808222062,  "solver1squareReads": 0.6989700043360189,  "solver1explicitTests": 1.2041199826559248,  "solver1implicitTests": 1.3010299956639813,  "solver2queenPlacements": 0.1,  "solver2methodCalls": 1.3617278360175928,  "solver2squareReads": 0.6989700043360189,  "solver2explicitTests": 1.2304489213782739,  "solver2implicitTests": 1.3222192947339193},
	{"size": "2", "solver1queenPlacements": 1.1760912590556813,  "solver1methodCalls": 2.61066016308988,  "solver1squareReads": 2.0253058652647704,  "solver1explicitTests": 2.3521825181113627,  "solver1implicitTests": 2.3729120029701067,  "solver2queenPlacements": 1.0,  "solver2methodCalls": 2.510545010206612,  "solver2squareReads": 1.9344984512435677,  "solver2explicitTests": 2.2430380486862944,  "solver2implicitTests": 2.3010299956639813},
	{"size": "3", "solver1queenPlacements": 2.708420900134713,  "solver1methodCalls": 4.28431791543061,  "solver1squareReads": 3.7318304202881625,  "solver1explicitTests": 3.917872955198848,  "solver1implicitTests": 3.9792751475910233,  "solver2queenPlacements": 2.110589710299249,  "solver2methodCalls": 3.8275631112547237,  "solver2squareReads": 3.2911467617318855,  "solver2explicitTests": 3.4616485680634552,  "solver2implicitTests": 3.5514499979728753},
	{"size": "4", "solver1queenPlacements": 4.8164467953202195,  "solver1methodCalls": 6.556332417801133,  "solver1squareReads": 6.028985830380328,  "solver1explicitTests": 6.147003496919546,  "solver1implicitTests": 6.222352759797032,  "solver2queenPlacements": 3.400710636773231,  "solver2methodCalls": 5.299415766886762,  "solver2squareReads": 4.783760695743924,  "solver2explicitTests": 4.884325142831696,  "solver2implicitTests": 4.9841536143517695},
	{"size": "5", "solver1queenPlacements": 7.525749205620827,  "solver1methodCalls": 9.434581510046916,  "solver1squareReads": 8.924064268966557,  "solver1explicitTests": 9.00365643185312,  "solver1implicitTests": 9.082408408565364,  "solver2queenPlacements": 4.8350878472324945,  "solver2methodCalls": 6.8903197834110905,  "solver2squareReads": 6.386476313871969,  "solver2explicitTests": 6.4542487819626135,  "solver2implicitTests": 6.550688461391552}
	];
//Data
var logData = { "9.00365643185312": 1008454787,  "0.1": 1,  "9.082408408565364": 1208950192,  "6.386476313871969": 2434873,  "1.3424226808222062": 22,  "1": 10,  "2.0253058652647704": 106,  "2.3729120029701067": 236,  "1.1760912590556813": 15,  "3.8275631112547237": 6723,  "2.3521825181113627": 225,  "2.110589710299249": 129,  "4.9841536143517695": 96417,  "4.884325142831696": 76617,  "6.147003496919546": 1402825,  "3.917872955198848": 8277,  "8.924064268966557": 839584223,  "6.556332417801133": 3600248,  "3.9792751475910233": 9534,  "6.550688461391552": 3553763,  "7.525749205620827": 33554379,  "4.783760695743924": 60780,  "1.3222192947339193": 21,  "1.2041199826559248": 16,  "3.7318304202881625": 5393,  "6.028985830380328": 1069020,  "6.8903197834110905": 7768189,  "1.9344984512435677": 86,  "2.708420900134713": 511,  "4.8350878472324945": 68405,  "4.28431791543061": 19245,  "9.434581510046916": 2720078953,  "6.222352759797032": 1668602,  "1.3010299956639813": 20,  "3.400710636773231": 2516,  "2.61066016308988": 408,  "4.8164467953202195": 65531,  "3.2911467617318855": 1955,  "2.3010299956639813": 200,  "6.4542487819626135": 2846091,  "3.4616485680634552": 2895,  "0.6989700043360189": 5,  "1.3617278360175928": 23,  "5.299415766886762": 199258,  "3.5514499979728753": 3560,  "1.2304489213782739": 17,  "2.510545010206612": 324,  "2.2430380486862944": 175	};
var queenPlacementsGraph = Morris.Line({
	element: 'queenPlacements',
	hideHover: 'auto',
	data: data,
	xkey: 'size',
	ykeys: ['solver1queenPlacements', 'solver2queenPlacements'],
	labels: ['Uber brute-force', 'List brute-force'],
	resize: true,
	parseTime: false,
	lineColors: ['#A52A2A', '#72A0C1'],
	yLabelFormat: function(y) { if( logData[y] ) { return logData[y].toLocaleString(); } else { if( 0 == y ) { return 0; } return "10^" + y;} },
	xLabelFormat: function(obj) { return (obj.x + 1).toLocaleString(); },
});
var methodCallsGraph = Morris.Line({
	element: 'methodCalls',
	hideHover: 'auto',
	data: data,
	xkey: 'size',
	ykeys: ['solver1methodCalls', 'solver2methodCalls'],
	labels: ['Uber brute-force', 'List brute-force'],
	resize: true,
	parseTime: false,
	lineColors: ['#A52A2A', '#72A0C1'],
	yLabelFormat: function(y) { if( logData[y] ) { return logData[y].toLocaleString(); } else { if( 0 == y ) { return 0; } return "10^" + y;} },
	xLabelFormat: function(obj) { return (obj.x + 1).toLocaleString(); },
});
var squareReadsGraph = Morris.Line({
	element: 'squareReads',
	hideHover: 'auto',
	data: data,
	xkey: 'size',
	ykeys: ['solver1squareReads', 'solver2squareReads'],
	labels: ['Uber brute-force', 'List brute-force'],
	resize: true,
	parseTime: false,
	lineColors: ['#A52A2A', '#72A0C1'],
	yLabelFormat: function(y) { if( logData[y] ) { return logData[y].toLocaleString(); } else { if( 0 == y ) { return 0; } return "10^" + y;} },
	xLabelFormat: function(obj) { return (obj.x + 1).toLocaleString(); },
});
var explicitTestsGraph = Morris.Line({
	element: 'explicitTests',
	hideHover: 'auto',
	data: data,
	xkey: 'size',
	ykeys: ['solver1explicitTests', 'solver2explicitTests'],
	labels: ['Uber brute-force', 'List brute-force'],
	resize: true,
	parseTime: false,
	lineColors: ['#A52A2A', '#72A0C1'],
	yLabelFormat: function(y) { if( logData[y] ) { return logData[y].toLocaleString(); } else { if( 0 == y ) { return 0; } return "10^" + y;} },
	xLabelFormat: function(obj) { return (obj.x + 1).toLocaleString(); },
});
var implicitTestsGraph = Morris.Line({
	element: 'implicitTests',
	hideHover: 'auto',
	data: data,
	xkey: 'size',
	ykeys: ['solver1implicitTests', 'solver2implicitTests'],
	labels: ['Uber brute-force', 'List brute-force'],
	resize: true,
	parseTime: false,
	lineColors: ['#A52A2A', '#72A0C1'],
	yLabelFormat: function(y) { if( logData[y] ) { return logData[y].toLocaleString(); } else { if( 0 == y ) { return 0; } return "10^" + y;} },
	xLabelFormat: function(obj) { return (obj.x + 1).toLocaleString(); },
});

</script>

All benchmarked operations with the uber-brute-force algorithm became huge very quickly and the speed up of only placing a maximum of N queens on a NxN chessboard is amazing!

## Next optimisations?

Next optimisations are tested in the part 2, click on the link below.
