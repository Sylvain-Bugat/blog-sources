+++
categories = ["code","8 queens puzzle"]
series = ["8 queens puzzle"]
date = "2015-05-06T20:46:15+02:00"
description = "N queens puzzle brute-force solvers part 2"
keywords = ["8 queens puzzle","8 queens","algorithm","N queens puzzle","N queens","puzzle","chessboard","chess","blog"]
title = "8 queens puzzle brute-force solvers part 2"

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

| chessboard size | execution time | number of runs |
| ------------- | ----------- | ----------- |
| 4 | 146.68 µs | 5000 |
| 5 | 4.24 ms| 5000 |
| 6 | 162.88 ms | 500 |
| 7 | 7.49 s | 50 |
| 8 | 6.20 m | 5 |
| 9 | 6.10 h | 1 |
| 10 | too long... | |
On 9x9 chessboard, the needed time to count all solutions is very long!

## Algorithms comparisons

Comparison of the previous and this algorithm:

<div class="panel panel-default tab-box">
	<div class="panel-heading">
		<h3 class="panel-title">
			<i class="fa fa-signal"></i>Algorithms comparison
		</h3>
		<ul class="nav nav-tabs">
			<li>
				<a href="#prefix1queenPlacementsTab" data-toggle="tab" data-identifier="prefix1queenPlacementsGraph">moves</a>
			</li>
			<li class="active">
				<a href="#prefix1methodCallsTab" data-toggle="tab" data-identifier="prefix1methodCallsGraph">method calls</a>
			</li>
			<li>
				<a href="#prefix1squareReadsTab" data-toggle="tab" data-identifier="prefix1squareReadsGraph">reads</a>
			</li>
			<li>
				<a href="#prefix1explicitTestsTab" data-toggle="tab" data-identifier="prefix1explicitTestsGraph">tests</a>
			</li>
			<li>
				<a href="#prefix1implicitTestsTab" data-toggle="tab" data-identifier="prefix1implicitTestsGraph">loop tests</a>
			</li>
		</ul>
	</div>
	<div class="panel-body">
		<div class="tab-content">
			<div id="prefix1queenPlacementsTab" class="tab-pane">
				<div class="row">
					<div class="caption">
						Queen placements count
					</div>
					<div id="prefix1queenPlacements"></div>
					<div class="legend">
						<span class="label" style="background-color: #A52A2A;">List brute-force</span>
						<span class="label" style="background-color: #72A0C1;">Queen constraint brute-force</span>
					</div>
				</div>
			</div>
			<div id="prefix1methodCallsTab" class="tab-pane active">
				<div class="row">
					<div class="caption">
						Method calls count
					</div>
					<div id="prefix1methodCalls"></div>
					<div class="legend">
						<span class="label" style="background-color: #A52A2A;">List brute-force</span>
						<span class="label" style="background-color: #72A0C1;">Queen constraint brute-force</span>
					</div>
				</div>
			</div>
			<div id="prefix1squareReadsTab" class="tab-pane">
				<div class="row">
					<div class="caption">
						Square reads count
					</div>
					<div id="prefix1squareReads"></div>
					<div class="legend">
						<span class="label" style="background-color: #A52A2A;">List brute-force</span>
						<span class="label" style="background-color: #72A0C1;">Queen constraint brute-force</span>
					</div>
				</div>
			</div>
			<div id="prefix1explicitTestsTab" class="tab-pane">
				<div class="row">
					<div class="caption">
						Explicit tests count
					</div>
					<div id="prefix1explicitTests"></div>
					<div class="legend">
						<span class="label" style="background-color: #A52A2A;">List brute-force</span>
						<span class="label" style="background-color: #72A0C1;">Queen constraint brute-force</span>
					</div>
				</div>
			</div>
			<div id="prefix1implicitTestsTab" class="tab-pane">
				<div class="row">
					<div class="caption">
						Loop tests count
					</div>
					<div id="prefix1implicitTests"></div>
					<div class="legend">
						<span class="label" style="background-color: #A52A2A;">List brute-force</span>
						<span class="label" style="background-color: #72A0C1;">Queen constraint brute-force</span>
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
var prefix1data = [
	{"size": "1", "solver1queenPlacements": 1,  "solver1methodCalls": 23,  "solver1squareReads": 5,  "solver1explicitTests": 17,  "solver1implicitTests": 21,  "solver2queenPlacements": 1,  "solver2methodCalls": 19,  "solver2squareReads": 4,  "solver2explicitTests": 16,  "solver2implicitTests": 17},
	{"size": "2", "solver1queenPlacements": 10,  "solver1methodCalls": 324,  "solver1squareReads": 86,  "solver1explicitTests": 175,  "solver1implicitTests": 200,  "solver2queenPlacements": 10,  "solver2methodCalls": 194,  "solver2squareReads": 46,  "solver2explicitTests": 135,  "solver2implicitTests": 110},
	{"size": "3", "solver1queenPlacements": 129,  "solver1methodCalls": 6723,  "solver1squareReads": 1955,  "solver1explicitTests": 2895,  "solver1implicitTests": 3560,  "solver2queenPlacements": 129,  "solver2methodCalls": 3111,  "solver2squareReads": 794,  "solver2explicitTests": 1734,  "solver2implicitTests": 1496},
	{"size": "4", "solver1queenPlacements": 2516,  "solver1methodCalls": 199258,  "solver1squareReads": 60780,  "solver1explicitTests": 76617,  "solver1implicitTests": 96417,  "solver2queenPlacements": 2516,  "solver2methodCalls": 75974,  "solver2squareReads": 20524,  "solver2explicitTests": 36361,  "solver2implicitTests": 33517},
	{"size": "5", "solver1queenPlacements": 68405,  "solver1methodCalls": 7768189,  "solver1squareReads": 2434873,  "solver1explicitTests": 2846091,  "solver1implicitTests": 3553763,  "solver2queenPlacements": 68405,  "solver2methodCalls": 2569409,  "solver2squareReads": 724748,  "solver2explicitTests": 1135966,  "solver2implicitTests": 1091183},
	{"size": "6", "solver1queenPlacements": 2391495,  "solver1methodCalls": 370248655,  "solver1squareReads": 117983963,  "solver1explicitTests": 132265835,  "solver1implicitTests": 162657186,  "solver2queenPlacements": 2391495,  "solver2methodCalls": 109575700,  "solver2squareReads": 31890143,  "solver2explicitTests": 46172015,  "solver2implicitTests": 45473931},
	{"size": "7", "solver1queenPlacements": 102022809,  "solver1methodCalls": 20677761079,  "solver1squareReads": 6659907867,  "solver1explicitTests": 7271931036,  "solver1implicitTests": 8803458227,  "solver2queenPlacements": 102022809,  "solver2methodCalls": 5578385347,  "solver2squareReads": 1660790226,  "solver2explicitTests": 2272813395,  "solver2implicitTests": 2273998451},
	{"size": "8", "solver1queenPlacements": 5130659560,  "solver1methodCalls": 1318173704582,  "solver1squareReads": 427654527254,  "solver1explicitTests": 458618310305,  "solver1implicitTests": 547241304569,  "solver2queenPlacements": 5130659560,  "solver2methodCalls": 327956409502,  "solver2squareReads": 99292315414,  "solver2explicitTests": 130256098465,  "solver2implicitTests": 131657880209}
	];
var prefix1queenPlacementsGraph = Morris.Line({
	element: 'prefix1queenPlacements',
	hideHover: 'auto',
	data: prefix1data,
	xkey: 'size',
	ykeys: ['solver1queenPlacements', 'solver2queenPlacements'],
	labels: ['List brute-force', 'Queen constraint brute-force'],
	resize: true,
	parseTime: false,
	lineColors: ['#A52A2A', '#72A0C1'],
	yLabelFormat: function(y) { return y.toLocaleString(); },
	xLabelFormat: function(obj) { return (obj.x + 1).toLocaleString(); },
});
var prefix1methodCallsGraph = Morris.Line({
	element: 'prefix1methodCalls',
	hideHover: 'auto',
	data: prefix1data,
	xkey: 'size',
	ykeys: ['solver1methodCalls', 'solver2methodCalls'],
	labels: ['List brute-force', 'Queen constraint brute-force'],
	resize: true,
	parseTime: false,
	lineColors: ['#A52A2A', '#72A0C1'],
	yLabelFormat: function(y) { return y.toLocaleString(); },
	xLabelFormat: function(obj) { return (obj.x + 1).toLocaleString(); },
});
var prefix1squareReadsGraph = Morris.Line({
	element: 'prefix1squareReads',
	hideHover: 'auto',
	data: prefix1data,
	xkey: 'size',
	ykeys: ['solver1squareReads', 'solver2squareReads'],
	labels: ['List brute-force', 'Queen constraint brute-force'],
	resize: true,
	parseTime: false,
	lineColors: ['#A52A2A', '#72A0C1'],
	yLabelFormat: function(y) { return y.toLocaleString(); },
	xLabelFormat: function(obj) { return (obj.x + 1).toLocaleString(); },
});
var prefix1explicitTestsGraph = Morris.Line({
	element: 'prefix1explicitTests',
	hideHover: 'auto',
	data: prefix1data,
	xkey: 'size',
	ykeys: ['solver1explicitTests', 'solver2explicitTests'],
	labels: ['List brute-force', 'Queen constraint brute-force'],
	resize: true,
	parseTime: false,
	lineColors: ['#A52A2A', '#72A0C1'],
	yLabelFormat: function(y) { return y.toLocaleString(); },
	xLabelFormat: function(obj) { return (obj.x + 1).toLocaleString(); },
});
var prefix1implicitTestsGraph = Morris.Line({
	element: 'prefix1implicitTests',
	hideHover: 'auto',
	data: prefix1data,
	xkey: 'size',
	ykeys: ['solver1implicitTests', 'solver2implicitTests'],
	labels: ['List brute-force', 'Queen constraint brute-force'],
	resize: true,
	parseTime: false,
	lineColors: ['#A52A2A', '#72A0C1'],
	yLabelFormat: function(y) { return y.toLocaleString(); },
	xLabelFormat: function(obj) { return (obj.x + 1).toLocaleString(); },
});

</script>

The number of read square is lower with the queen constraint because the chessboard don't need to be read to check the number of placed queens.

## Use a two-dimensional array for the chessboard

### Explaination

The data structure used to represent the chessboard can be changed from a list of lists to a two-dimensional array. Data access is faster because it's more direct without any method call, and the stored data can be a primitive type (boolean in this case intead of Boolean object).

### Implementation

This is the previous implementation with an two-dimensional array instead of a list of lists:

```java
/** Chessboard represented by a 2 dimensional array. */
private final boolean[][] chessboard;
/** Current number of queens on the chessboard. */
private int placedQueens;

private void solve(final int x, final int y) {

	// Put a queen on the current position
	chessboard[x][y] = true;
	placedQueens++;

	// All queens on the chessboard then a solution may be present
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

| chessboard size | execution time | number of runs
| ------------- | ----------- | ----------- |
| 4 | 84.20 µs | 5000 |
| 5 | 2.89 ms | 5000 | 
| 6 | 94.52 ms | 500 |
| 7 | 4.12 s | 50 |
| 8 | 3.50 m | 5 |
| 9 | too long... | |
On 8x8 chessboard, the needed time to count all solutions is long!

## Algorithms comparisons

Comparison of theses 2 algorithms:

<div class="panel panel-default tab-box">
	<div class="panel-heading">
		<h3 class="panel-title">
			<i class="fa fa-signal"></i>Algorithms comparison
		</h3>
		<ul class="nav nav-tabs">
			<li class="active">
				<a href="#prefix2queenPlacementsTab" data-toggle="tab" data-identifier="prefix2queenPlacementsGraph">moves</a>
			</li>
			<li>
				<a href="#prefix2methodCallsTab" data-toggle="tab" data-identifier="prefix2methodCallsGraph">method calls</a>
			</li>
			<li>
				<a href="#prefix2squareReadsTab" data-toggle="tab" data-identifier="prefix2squareReadsGraph">reads</a>
			</li>
			<li>
				<a href="#prefix2explicitTestsTab" data-toggle="tab" data-identifier="prefix2explicitTestsGraph">tests</a>
			</li>
			<li>
				<a href="#prefix2implicitTestsTab" data-toggle="tab" data-identifier="prefix2implicitTestsGraph">loop tests</a>
			</li>
		</ul>
	</div>
	<div class="panel-body">
		<div class="tab-content">
			<div id="prefix2queenPlacementsTab" class="tab-pane active">
				<div class="row">
					<div class="caption">
						Queen placements count
					</div>
					<div id="prefix2queenPlacements"></div>
					<div class="legend">
						<span class="label" style="background-color: #A52A2A;">Queen constraint brute-force</span>
						<span class="label" style="background-color: #72A0C1;">Array brute-force</span>
					</div>
				</div>
			</div>
			<div id="prefix2methodCallsTab" class="tab-pane">
				<div class="row">
					<div class="caption">
						Method calls count
					</div>
					<div id="prefix2methodCalls"></div>
					<div class="legend">
						<span class="label" style="background-color: #A52A2A;">Queen constraint brute-force</span>
						<span class="label" style="background-color: #72A0C1;">Array brute-force</span>
					</div>
				</div>
			</div>
			<div id="prefix2squareReadsTab" class="tab-pane">
				<div class="row">
					<div class="caption">
						Square reads count
					</div>
					<div id="prefix2squareReads"></div>
					<div class="legend">
						<span class="label" style="background-color: #A52A2A;">Queen constraint brute-force</span>
						<span class="label" style="background-color: #72A0C1;">Array brute-force</span>
					</div>
				</div>
			</div>
			<div id="prefix2explicitTestsTab" class="tab-pane">
				<div class="row">
					<div class="caption">
						Explicit tests count
					</div>
					<div id="prefix2explicitTests"></div>
					<div class="legend">
						<span class="label" style="background-color: #A52A2A;">Queen constraint brute-force</span>
						<span class="label" style="background-color: #72A0C1;">Array brute-force</span>
					</div>
				</div>
			</div>
			<div id="prefix2implicitTestsTab" class="tab-pane">
				<div class="row">
					<div class="caption">
						Loop tests count
					</div>
					<div id="prefix2implicitTests"></div>
					<div class="legend">
						<span class="label" style="background-color: #A52A2A;">Queen constraint brute-force</span>
						<span class="label" style="background-color: #72A0C1;">Array brute-force</span>
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
var prefix2data = [
	{"size": "1", "solver1queenPlacements": 0.0,  "solver1methodCalls": 1.2787536009528289,  "solver1squareReads": 0.6020599913279624,  "solver1explicitTests": 1.2041199826559248,  "solver1implicitTests": 1.2304489213782739,  "solver2queenPlacements": 0.0,  "solver2methodCalls": 0.47712125471966244,  "solver2squareReads": 0.6020599913279624,  "solver2explicitTests": 1.2041199826559248,  "solver2implicitTests": 1.2304489213782739},
	{"size": "2", "solver1queenPlacements": 1.0,  "solver1methodCalls": 2.287801729930226,  "solver1squareReads": 1.662757831681574,  "solver1explicitTests": 2.130333768495006,  "solver1implicitTests": 2.041392685158225,  "solver2queenPlacements": 1.0,  "solver2methodCalls": 1.2041199826559248,  "solver2squareReads": 1.662757831681574,  "solver2explicitTests": 2.130333768495006,  "solver2implicitTests": 2.041392685158225},
	{"size": "3", "solver1queenPlacements": 2.110589710299249,  "solver1methodCalls": 3.4929000111087034,  "solver1squareReads": 2.8998205024270964,  "solver1explicitTests": 3.2390490931401916,  "solver1implicitTests": 3.1749315935284423,  "solver2queenPlacements": 2.110589710299249,  "solver2methodCalls": 2.3283796034387376,  "solver2squareReads": 2.8998205024270964,  "solver2explicitTests": 3.2390490931401916,  "solver2implicitTests": 3.1749315935284423},
	{"size": "4", "solver1queenPlacements": 3.400710636773231,  "solver1methodCalls": 4.880664992432927,  "solver1squareReads": 4.312262005983347,  "solver1explicitTests": 4.560635818678364,  "solver1implicitTests": 4.525265139380899,  "solver2queenPlacements": 3.400710636773231,  "solver2methodCalls": 3.6372895476781744,  "solver2squareReads": 4.312262005983347,  "solver2explicitTests": 4.560635818678364,  "solver2implicitTests": 4.525265139380899},
	{"size": "5", "solver1queenPlacements": 4.8350878472324945,  "solver1methodCalls": 6.409833241014112,  "solver1squareReads": 5.860187025558374,  "solver1explicitTests": 6.055365332930141,  "solver1implicitTests": 6.037897591308276,  "solver2queenPlacements": 4.8350878472324945,  "solver2methodCalls": 5.084737097962795,  "solver2squareReads": 5.860187025558374,  "solver2explicitTests": 6.055365332930141,  "solver2implicitTests": 6.037897591308276},
	{"size": "6", "solver1queenPlacements": 6.378669477211044,  "solver1methodCalls": 8.03971425372861,  "solver1squareReads": 7.503656466686411,  "solver1explicitTests": 7.664378828076926,  "solver1implicitTests": 7.657762498472013,  "solver2queenPlacements": 6.378669477211044,  "solver2methodCalls": 6.6374187756089364,  "solver2squareReads": 7.503656466686411,  "solver2explicitTests": 7.664378828076926,  "solver2implicitTests": 7.657762498472013},
	{"size": "7", "solver1queenPlacements": 8.008697276815294,  "solver1methodCalls": 9.746508511417176,  "solver1squareReads": 9.220314780287609,  "solver1explicitTests": 9.356563780270248,  "solver1implicitTests": 9.356790164519534,  "solver2queenPlacements": 8.008697276815294,  "solver2methodCalls": 8.273980937567645,  "solver2squareReads": 9.220314780287609,  "solver2explicitTests": 9.356563780270248,  "solver2implicitTests": 9.356790164519534}
	];
//Data
var prefix2logData = { "8.008697276815294": 102022809,  "1.2787536009528289": 19,  "0.0": 1,  "9.356790164519534": 2273998451,  "4.880664992432927": 75974,  "4.312262005983347": 20524,  "9.746508511417176": 5578385347,  "1.0": 10,  "0.6020599913279624": 4,  "6.378669477211044": 2391495,  "6.055365332930141": 1135966,  "2.110589710299249": 129,  "6.409833241014112": 2569409,  "8.03971425372861": 109575700,  "9.220314780287609": 1660790226,  "7.664378828076926": 46172015,  "4.525265139380899": 33517,  "2.130333768495006": 135,  "2.8998205024270964": 794,  "1.2041199826559248": 16,  "5.084737097962795": 121545,  "8.273980937567645": 187923433,  "6.6374187756089364": 4339291,  "2.287801729930226": 194,  "9.356563780270248": 2272813395,  "3.1749315935284423": 1496,  "4.8350878472324945": 68405,  "4.560635818678364": 36361,  "6.037897591308276": 1091183,  "5.860187025558374": 724748,  "7.657762498472013": 45473931,  "3.400710636773231": 2516,  "2.3283796034387376": 213,  "3.4929000111087034": 3111,  "3.6372895476781744": 4338,  "7.503656466686411": 31890143,  "2.041392685158225": 110,  "1.662757831681574": 46,  "0.47712125471966244": 3,  "1.2304489213782739": 17,  "3.2390490931401916": 1734	};
var prefix2queenPlacementsGraph = Morris.Line({
	element: 'prefix2queenPlacements',
	hideHover: 'auto',
	data: prefix2data,
	xkey: 'size',
	ykeys: ['solver1queenPlacements', 'solver2queenPlacements'],
	labels: ['Queen constraint brute-force', 'Array brute-force'],
	resize: true,
	parseTime: false,
	lineColors: ['#A52A2A', '#72A0C1'],
	yLabelFormat: function(y) { if( prefix2logData[y] ) { return prefix2logData[y].toLocaleString(); } else { if( 0 == y ) { return 0; } var n=1;for (var i = 0; i < y; i++) { n*=10; } return n.toLocaleString();} },
	xLabelFormat: function(obj) { return (obj.x + 1).toLocaleString(); },
});
var prefix2methodCallsGraph = Morris.Line({
	element: 'prefix2methodCalls',
	hideHover: 'auto',
	data: prefix2data,
	xkey: 'size',
	ykeys: ['solver1methodCalls', 'solver2methodCalls'],
	labels: ['Queen constraint brute-force', 'Array brute-force'],
	resize: true,
	parseTime: false,
	lineColors: ['#A52A2A', '#72A0C1'],
	yLabelFormat: function(y) { if( prefix2logData[y] ) { return prefix2logData[y].toLocaleString(); } else { if( 0 == y ) { return 0; } var n=1;for (var i = 0; i < y; i++) { n*=10; } return n.toLocaleString();} },
	xLabelFormat: function(obj) { return (obj.x + 1).toLocaleString(); },
});
var prefix2squareReadsGraph = Morris.Line({
	element: 'prefix2squareReads',
	hideHover: 'auto',
	data: prefix2data,
	xkey: 'size',
	ykeys: ['solver1squareReads', 'solver2squareReads'],
	labels: ['Queen constraint brute-force', 'Array brute-force'],
	resize: true,
	parseTime: false,
	lineColors: ['#A52A2A', '#72A0C1'],
	yLabelFormat: function(y) { if( prefix2logData[y] ) { return prefix2logData[y].toLocaleString(); } else { if( 0 == y ) { return 0; } var n=1;for (var i = 0; i < y; i++) { n*=10; } return n.toLocaleString();} },
	xLabelFormat: function(obj) { return (obj.x + 1).toLocaleString(); },
});
var prefix2explicitTestsGraph = Morris.Line({
	element: 'prefix2explicitTests',
	hideHover: 'auto',
	data: prefix2data,
	xkey: 'size',
	ykeys: ['solver1explicitTests', 'solver2explicitTests'],
	labels: ['Queen constraint brute-force', 'Array brute-force'],
	resize: true,
	parseTime: false,
	lineColors: ['#A52A2A', '#72A0C1'],
	yLabelFormat: function(y) { if( prefix2logData[y] ) { return prefix2logData[y].toLocaleString(); } else { if( 0 == y ) { return 0; } var n=1;for (var i = 0; i < y; i++) { n*=10; } return n.toLocaleString();} },
	xLabelFormat: function(obj) { return (obj.x + 1).toLocaleString(); },
});
var prefix2implicitTestsGraph = Morris.Line({
	element: 'prefix2implicitTests',
	hideHover: 'auto',
	data: prefix2data,
	xkey: 'size',
	ykeys: ['solver1implicitTests', 'solver2implicitTests'],
	labels: ['Queen constraint brute-force', 'Array brute-force'],
	resize: true,
	parseTime: false,
	lineColors: ['#A52A2A', '#72A0C1'],
	yLabelFormat: function(y) { if( prefix2logData[y] ) { return prefix2logData[y].toLocaleString(); } else { if( 0 == y ) { return 0; } var n=1;for (var i = 0; i < y; i++) { n*=10; } return n.toLocaleString();} },
	xLabelFormat: function(obj) { return (obj.x + 1).toLocaleString(); },
});

</script>

The number of called methods is much lower when using an array because square reads and writes don't need method calls anymore.

## Next optimisations?

Next optimisations are tested in the part 3, click on the link below.
