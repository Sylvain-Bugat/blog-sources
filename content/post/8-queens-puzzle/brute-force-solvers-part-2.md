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
			<i class="glyphicon glyphicon-stats"></i>Algorithms comparison
		</h3>
		<ul class="nav nav-tabs">
			<li>
				<a href="#prefixqueenPlacementsTab" data-toggle="tab" data-identifier="prefixqueenPlacementsGraph">moves</a>
			</li>
			<li>
				<a href="#prefixmethodCallsTab" data-toggle="tab" data-identifier="prefixmethodCallsGraph">method calls</a>
			</li>
			<li  class="active">
				<a href="#prefixsquareReadsTab" data-toggle="tab" data-identifier="prefixsquareReadsGraph">reads</a>
			</li>
			<li>
				<a href="#prefixexplicitTestsTab" data-toggle="tab" data-identifier="prefixexplicitTestsGraph">tests</a>
			</li>
			<li>
				<a href="#prefiximplicitTestsTab" data-toggle="tab" data-identifier="prefiximplicitTestsGraph">loop tests</a>
			</li>
		</ul>
	</div>
	<div class="panel-body">
		<div class="tab-content">
			<div id="prefixqueenPlacementsTab" class="tab-pane">
				<div class="row">
					<div class="caption">
						Queen placements count
					</div>
					<div id="prefixqueenPlacements"></div>
					<div class="legend">
						<span class="label" style="background-color: #A52A2A;">List brute-force</span>
						<span class="label" style="background-color: #72A0C1;">Queen constraint brute-force</span>
					</div>
				</div>
			</div>
			<div id="prefixmethodCallsTab" class="tab-pane">
				<div class="row">
					<div class="caption">
						Method calls count
					</div>
					<div id="prefixmethodCalls"></div>
					<div class="legend">
						<span class="label" style="background-color: #A52A2A;">List brute-force</span>
						<span class="label" style="background-color: #72A0C1;">Queen constraint brute-force</span>
					</div>
				</div>
			</div>
			<div id="prefixsquareReadsTab" class="tab-pane active">
				<div class="row">
					<div class="caption">
						Square reads count
					</div>
					<div id="prefixsquareReads"></div>
					<div class="legend">
						<span class="label" style="background-color: #A52A2A;">List brute-force</span>
						<span class="label" style="background-color: #72A0C1;">Queen constraint brute-force</span>
					</div>
				</div>
			</div>
			<div id="prefixexplicitTestsTab" class="tab-pane">
				<div class="row">
					<div class="caption">
						Explicit tests count
					</div>
					<div id="prefixexplicitTests"></div>
					<div class="legend">
						<span class="label" style="background-color: #A52A2A;">List brute-force</span>
						<span class="label" style="background-color: #72A0C1;">Queen constraint brute-force</span>
					</div>
				</div>
			</div>
			<div id="prefiximplicitTestsTab" class="tab-pane">
				<div class="row">
					<div class="caption">
						Loop tests count
					</div>
					<div id="prefiximplicitTests"></div>
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
var prefixdata = [
	{"size": "1", "solver1queenPlacements": 0.1,  "solver1methodCalls": 1.3617278360175928,  "solver1squareReads": 0.6989700043360189,  "solver1explicitTests": 1.2304489213782739,  "solver1implicitTests": 1.3222192947339193,  "solver2queenPlacements": 0.1,  "solver2methodCalls": 1.2787536009528289,  "solver2squareReads": 0.6020599913279624,  "solver2explicitTests": 1.2041199826559248,  "solver2implicitTests": 1.2304489213782739},
	{"size": "2", "solver1queenPlacements": 1.0,  "solver1methodCalls": 2.510545010206612,  "solver1squareReads": 1.9344984512435677,  "solver1explicitTests": 2.2430380486862944,  "solver1implicitTests": 2.3010299956639813,  "solver2queenPlacements": 1.0,  "solver2methodCalls": 2.287801729930226,  "solver2squareReads": 1.662757831681574,  "solver2explicitTests": 2.130333768495006,  "solver2implicitTests": 2.041392685158225},
	{"size": "3", "solver1queenPlacements": 2.110589710299249,  "solver1methodCalls": 3.8275631112547237,  "solver1squareReads": 3.2911467617318855,  "solver1explicitTests": 3.4616485680634552,  "solver1implicitTests": 3.5514499979728753,  "solver2queenPlacements": 2.110589710299249,  "solver2methodCalls": 3.4929000111087034,  "solver2squareReads": 2.8998205024270964,  "solver2explicitTests": 3.2390490931401916,  "solver2implicitTests": 3.1749315935284423},
	{"size": "4", "solver1queenPlacements": 3.400710636773231,  "solver1methodCalls": 5.299415766886762,  "solver1squareReads": 4.783760695743924,  "solver1explicitTests": 4.884325142831696,  "solver1implicitTests": 4.9841536143517695,  "solver2queenPlacements": 3.400710636773231,  "solver2methodCalls": 4.880664992432927,  "solver2squareReads": 4.312262005983347,  "solver2explicitTests": 4.560635818678364,  "solver2implicitTests": 4.525265139380899},
	{"size": "5", "solver1queenPlacements": 4.8350878472324945,  "solver1methodCalls": 6.8903197834110905,  "solver1squareReads": 6.386476313871969,  "solver1explicitTests": 6.4542487819626135,  "solver1implicitTests": 6.550688461391552,  "solver2queenPlacements": 4.8350878472324945,  "solver2methodCalls": 6.409833241014112,  "solver2squareReads": 5.860187025558374,  "solver2explicitTests": 6.055365332930141,  "solver2implicitTests": 6.037897591308276},
	{"size": "6", "solver1queenPlacements": 6.378669477211044,  "solver1methodCalls": 8.568493489537232,  "solver1squareReads": 8.07182297973045,  "solver1explicitTests": 8.121447677995999,  "solver1implicitTests": 8.211273254652662,  "solver2queenPlacements": 6.378669477211044,  "solver2methodCalls": 8.03971425372861,  "solver2squareReads": 7.503656466686411,  "solver2explicitTests": 7.664378828076926,  "solver2implicitTests": 7.657762498472013},
	{"size": "7", "solver1queenPlacements": 8.008697276815294,  "solver1methodCalls": 10.315503512967584,  "solver1squareReads": 9.823468221192783,  "solver1explicitTests": 9.861649751563379,  "solver1implicitTests": 9.944653307817687,  "solver2queenPlacements": 8.008697276815294,  "solver2methodCalls": 9.746508511417176,  "solver2squareReads": 9.220314780287609,  "solver2explicitTests": 9.356563780270248,  "solver2implicitTests": 9.356790164519534},
	{"size": "8", "solver1queenPlacements": 9.710173198417113,  "solver1methodCalls": 12.119972643923148,  "solver1squareReads": 11.631093073935638,  "solver1explicitTests": 11.66145138991786,  "solver1implicitTests": 11.738178869540649,  "solver2queenPlacements": 9.710173198417113,  "solver2methodCalls": 11.515816123068971,  "solver2squareReads": 10.996915638198928,  "solver2explicitTests": 11.114798065696673,  "solver2implicitTests": 11.119446858344201}
	];
//Data
var prefixlogData = { "0.1": 1,  "1.2787536009528289": 19,  "8.121447677995999": 132265835,  "4.312262005983347": 20524,  "6.386476313871969": 2434873,  "9.746508511417176": 5578385347,  "9.710173198417113": 5130659560,  "0.6020599913279624": 4,  "11.114798065696673": 130256098465,  "6.055365332930141": 1135966,  "6.378669477211044": 2391495,  "11.515816123068971": 327956409502,  "8.568493489537232": 370248655,  "8.211273254652662": 162657186,  "4.884325142831696": 76617,  "7.664378828076926": 46172015,  "4.525265139380899": 33517,  "6.550688461391552": 3553763,  "2.8998205024270964": 794,  "9.944653307817687": 8803458227,  "4.783760695743924": 60780,  "1.3222192947339193": 21,  "1.2041199826559248": 16,  "2.287801729930226": 194,  "12.119972643923148": 1318173704582,  "1.9344984512435677": 86,  "4.8350878472324945": 68405,  "4.560635818678364": 36361,  "6.037897591308276": 1091183,  "5.860187025558374": 724748,  "11.738178869540649": 547241304569,  "3.400710636773231": 2516,  "11.66145138991786": 458618310305,  "3.2911467617318855": 1955,  "3.4616485680634552": 2895,  "6.4542487819626135": 2846091,  "7.503656466686411": 31890143,  "1.3617278360175928": 23,  "2.041392685158225": 110,  "1.662757831681574": 46,  "5.299415766886762": 199258,  "1.2304489213782739": 17,  "2.510545010206612": 324,  "8.008697276815294": 102022809,  "9.356790164519534": 2273998451,  "9.861649751563379": 7271931036,  "4.880664992432927": 75974,  "1": 10,  "3.8275631112547237": 6723,  "10.996915638198928": 99292315414,  "2.110589710299249": 129,  "4.9841536143517695": 96417,  "6.409833241014112": 2569409,  "8.03971425372861": 109575700,  "9.220314780287609": 1660790226,  "10.315503512967584": 20677761079,  "11.119446858344201": 131657880209,  "2.130333768495006": 135,  "11.631093073935638": 427654527254,  "9.356563780270248": 2272813395,  "6.8903197834110905": 7768189,  "3.1749315935284423": 1496,  "7.657762498472013": 45473931,  "9.823468221192783": 6659907867,  "2.3010299956639813": 200,  "3.4929000111087034": 3111,  "0.6989700043360189": 5,  "8.07182297973045": 117983963,  "3.5514499979728753": 3560,  "2.2430380486862944": 175,  "3.2390490931401916": 1734	};
var prefixqueenPlacementsGraph = Morris.Line({
	element: 'prefixqueenPlacements',
	hideHover: 'auto',
	data: prefixdata,
	xkey: 'size',
	ykeys: ['solver1queenPlacements', 'solver2queenPlacements'],
	labels: ['List brute-force', 'Queen constraint brute-force'],
	resize: true,
	parseTime: false,
	lineColors: ['#A52A2A', '#72A0C1'],
	yLabelFormat: function(y) { if( prefixlogData[y] ) { return prefixlogData[y].toLocaleString(); } else { if( 0 == y ) { return 0; } return "10^" + y;} },
	xLabelFormat: function(obj) { return (obj.x + 1).toLocaleString(); },
});
var prefixmethodCallsGraph = Morris.Line({
	element: 'prefixmethodCalls',
	hideHover: 'auto',
	data: prefixdata,
	xkey: 'size',
	ykeys: ['solver1methodCalls', 'solver2methodCalls'],
	labels: ['List brute-force', 'Queen constraint brute-force'],
	resize: true,
	parseTime: false,
	lineColors: ['#A52A2A', '#72A0C1'],
	yLabelFormat: function(y) { if( prefixlogData[y] ) { return prefixlogData[y].toLocaleString(); } else { if( 0 == y ) { return 0; } return "10^" + y;} },
	xLabelFormat: function(obj) { return (obj.x + 1).toLocaleString(); },
});
var prefixsquareReadsGraph = Morris.Line({
	element: 'prefixsquareReads',
	hideHover: 'auto',
	data: prefixdata,
	xkey: 'size',
	ykeys: ['solver1squareReads', 'solver2squareReads'],
	labels: ['List brute-force', 'Queen constraint brute-force'],
	resize: true,
	parseTime: false,
	lineColors: ['#A52A2A', '#72A0C1'],
	yLabelFormat: function(y) { if( prefixlogData[y] ) { return prefixlogData[y].toLocaleString(); } else { if( 0 == y ) { return 0; } return "10^" + y;} },
	xLabelFormat: function(obj) { return (obj.x + 1).toLocaleString(); },
});
var prefixexplicitTestsGraph = Morris.Line({
	element: 'prefixexplicitTests',
	hideHover: 'auto',
	data: prefixdata,
	xkey: 'size',
	ykeys: ['solver1explicitTests', 'solver2explicitTests'],
	labels: ['List brute-force', 'Queen constraint brute-force'],
	resize: true,
	parseTime: false,
	lineColors: ['#A52A2A', '#72A0C1'],
	yLabelFormat: function(y) { if( prefixlogData[y] ) { return prefixlogData[y].toLocaleString(); } else { if( 0 == y ) { return 0; } return "10^" + y;} },
	xLabelFormat: function(obj) { return (obj.x + 1).toLocaleString(); },
});
var prefiximplicitTestsGraph = Morris.Line({
	element: 'prefiximplicitTests',
	hideHover: 'auto',
	data: prefixdata,
	xkey: 'size',
	ykeys: ['solver1implicitTests', 'solver2implicitTests'],
	labels: ['List brute-force', 'Queen constraint brute-force'],
	resize: true,
	parseTime: false,
	lineColors: ['#A52A2A', '#72A0C1'],
	yLabelFormat: function(y) { if( prefixlogData[y] ) { return prefixlogData[y].toLocaleString(); } else { if( 0 == y ) { return 0; } return "10^" + y;} },
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
			<i class="glyphicon glyphicon-stats"></i>Algorithms comparison
		</h3>
		<ul class="nav nav-tabs">
			<li>
				<a href="#prefix2queenPlacementsTab" data-toggle="tab" data-identifier="prefix2queenPlacementsGraph">moves</a>
			</li>
			<li class="active">
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
			<div id="prefix2queenPlacementsTab" class="tab-pane">
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
			<div id="prefix2methodCallsTab" class="tab-pane active">
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
	{"size": "1", "solver1queenPlacements": 0.1,  "solver1methodCalls": 1.2787536009528289,  "solver1squareReads": 0.6020599913279624,  "solver1explicitTests": 1.2041199826559248,  "solver1implicitTests": 1.2304489213782739,  "solver2queenPlacements": 0.1,  "solver2methodCalls": 0.47712125471966244,  "solver2squareReads": 0.6020599913279624,  "solver2explicitTests": 1.2041199826559248,  "solver2implicitTests": 1.2304489213782739},
	{"size": "2", "solver1queenPlacements": 1.0,  "solver1methodCalls": 2.287801729930226,  "solver1squareReads": 1.662757831681574,  "solver1explicitTests": 2.130333768495006,  "solver1implicitTests": 2.041392685158225,  "solver2queenPlacements": 1.0,  "solver2methodCalls": 1.2041199826559248,  "solver2squareReads": 1.662757831681574,  "solver2explicitTests": 2.130333768495006,  "solver2implicitTests": 2.041392685158225},
	{"size": "3", "solver1queenPlacements": 2.110589710299249,  "solver1methodCalls": 3.4929000111087034,  "solver1squareReads": 2.8998205024270964,  "solver1explicitTests": 3.2390490931401916,  "solver1implicitTests": 3.1749315935284423,  "solver2queenPlacements": 2.110589710299249,  "solver2methodCalls": 2.3283796034387376,  "solver2squareReads": 2.8998205024270964,  "solver2explicitTests": 3.2390490931401916,  "solver2implicitTests": 3.1749315935284423},
	{"size": "4", "solver1queenPlacements": 3.400710636773231,  "solver1methodCalls": 4.880664992432927,  "solver1squareReads": 4.312262005983347,  "solver1explicitTests": 4.560635818678364,  "solver1implicitTests": 4.525265139380899,  "solver2queenPlacements": 3.400710636773231,  "solver2methodCalls": 3.6372895476781744,  "solver2squareReads": 4.312262005983347,  "solver2explicitTests": 4.560635818678364,  "solver2implicitTests": 4.525265139380899},
	{"size": "5", "solver1queenPlacements": 4.8350878472324945,  "solver1methodCalls": 6.409833241014112,  "solver1squareReads": 5.860187025558374,  "solver1explicitTests": 6.055365332930141,  "solver1implicitTests": 6.037897591308276,  "solver2queenPlacements": 4.8350878472324945,  "solver2methodCalls": 5.084737097962795,  "solver2squareReads": 5.860187025558374,  "solver2explicitTests": 6.055365332930141,  "solver2implicitTests": 6.037897591308276},
	{"size": "6", "solver1queenPlacements": 6.378669477211044,  "solver1methodCalls": 8.03971425372861,  "solver1squareReads": 7.503656466686411,  "solver1explicitTests": 7.664378828076926,  "solver1implicitTests": 7.657762498472013,  "solver2queenPlacements": 6.378669477211044,  "solver2methodCalls": 6.6374187756089364,  "solver2squareReads": 7.503656466686411,  "solver2explicitTests": 7.664378828076926,  "solver2implicitTests": 7.657762498472013},
	{"size": "7", "solver1queenPlacements": 8.008697276815294,  "solver1methodCalls": 9.746508511417176,  "solver1squareReads": 9.220314780287609,  "solver1explicitTests": 9.356563780270248,  "solver1implicitTests": 9.356790164519534,  "solver2queenPlacements": 8.008697276815294,  "solver2methodCalls": 8.273980937567645,  "solver2squareReads": 9.220314780287609,  "solver2explicitTests": 9.356563780270248,  "solver2implicitTests": 9.356790164519534},
	{"size": "8", "solver1queenPlacements": 9.710173198417113,  "solver1methodCalls": 11.515816123068971,  "solver1squareReads": 10.996915638198928,  "solver1explicitTests": 11.114798065696673,  "solver1implicitTests": 11.119446858344201,  "solver2queenPlacements": 9.710173198417113,  "solver2methodCalls": 9.980313634397985,  "solver2squareReads": 10.996915638198928,  "solver2explicitTests": 11.114798065696673,  "solver2implicitTests": 11.119446858344201}
	];
//Data
var prefix2logData = { "8.008697276815294": 102022809,  "1.2787536009528289": 19,  "0.1": 1,  "9.356790164519534": 2273998451,  "4.880664992432927": 75974,  "4.312262005983347": 20524,  "9.746508511417176": 5578385347,  "1": 10,  "9.710173198417113": 5130659560,  "0.6020599913279624": 4,  "11.114798065696673": 130256098465,  "10.996915638198928": 99292315414,  "6.378669477211044": 2391495,  "6.055365332930141": 1135966,  "2.110589710299249": 129,  "11.515816123068971": 327956409502,  "6.409833241014112": 2569409,  "8.03971425372861": 109575700,  "9.220314780287609": 1660790226,  "7.664378828076926": 46172015,  "4.525265139380899": 33517,  "11.119446858344201": 131657880209,  "2.130333768495006": 135,  "2.8998205024270964": 794,  "1.2041199826559248": 16,  "5.084737097962795": 121545,  "8.273980937567645": 187923433,  "6.6374187756089364": 4339291,  "9.980313634397985": 9556825020,  "2.287801729930226": 194,  "9.356563780270248": 2272813395,  "3.1749315935284423": 1496,  "4.8350878472324945": 68405,  "4.560635818678364": 36361,  "6.037897591308276": 1091183,  "5.860187025558374": 724748,  "7.657762498472013": 45473931,  "3.400710636773231": 2516,  "2.3283796034387376": 213,  "3.4929000111087034": 3111,  "3.6372895476781744": 4338,  "7.503656466686411": 31890143,  "2.041392685158225": 110,  "1.662757831681574": 46,  "0.47712125471966244": 3,  "1.2304489213782739": 17,  "3.2390490931401916": 1734	};
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
	yLabelFormat: function(y) { if( prefix2logData[y] ) { return prefix2logData[y].toLocaleString(); } else { if( 0 == y ) { return 0; } return "10^" + y;} },
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
	yLabelFormat: function(y) { if( prefix2logData[y] ) { return prefix2logData[y].toLocaleString(); } else { if( 0 == y ) { return 0; } return "10^" + y;} },
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
	yLabelFormat: function(y) { if( prefix2logData[y] ) { return prefix2logData[y].toLocaleString(); } else { if( 0 == y ) { return 0; } return "10^" + y;} },
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
	yLabelFormat: function(y) { if( prefix2logData[y] ) { return prefix2logData[y].toLocaleString(); } else { if( 0 == y ) { return 0; } return "10^" + y;} },
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
	yLabelFormat: function(y) { if( prefix2logData[y] ) { return prefix2logData[y].toLocaleString(); } else { if( 0 == y ) { return 0; } return "10^" + y;} },
	xLabelFormat: function(obj) { return (obj.x + 1).toLocaleString(); },
});

</script>

The number of called methods is much lower when using an array because square reads and writes don't need method calls anymore.

## Next optimisations?

Next optimisations are tested in the part 3, click on the link below.
