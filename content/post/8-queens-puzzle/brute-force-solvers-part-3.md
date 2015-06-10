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

## Algorithms comparisons

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
			<li>
				<a href="#prefixsquareReadsTab" data-toggle="tab" data-identifier="prefixsquareReadsGraph">reads</a>
			</li>
			<li>
				<a href="#prefixexplicitTestsTab" data-toggle="tab" data-identifier="prefixexplicitTestsGraph">tests</a>
			</li>
			<li class="active">
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
						<span class="label" style="background-color: #A52A2A;">Array brute-force</span>
						<span class="label" style="background-color: #72A0C1;">One-dimension array brute-force</span>
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
						<span class="label" style="background-color: #A52A2A;">Array brute-force</span>
						<span class="label" style="background-color: #72A0C1;">One-dimension array brute-force</span>
					</div>
				</div>
			</div>
			<div id="prefixsquareReadsTab" class="tab-pane">
				<div class="row">
					<div class="caption">
						Square reads count
					</div>
					<div id="prefixsquareReads"></div>
					<div class="legend">
						<span class="label" style="background-color: #A52A2A;">Array brute-force</span>
						<span class="label" style="background-color: #72A0C1;">One-dimension array brute-force</span>
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
						<span class="label" style="background-color: #A52A2A;">Array brute-force</span>
						<span class="label" style="background-color: #72A0C1;">One-dimension array brute-force</span>
					</div>
				</div>
			</div>
			<div id="prefiximplicitTestsTab" class="tab-pane active">
				<div class="row">
					<div class="caption">
						Loop tests count
					</div>
					<div id="prefiximplicitTests"></div>
					<div class="legend">
						<span class="label" style="background-color: #A52A2A;">Array brute-force</span>
						<span class="label" style="background-color: #72A0C1;">One-dimension array brute-force</span>
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
	{"size": "1", "solver1queenPlacements": 0.1,  "solver1methodCalls": 0.47712125471966244,  "solver1squareReads": 0.6020599913279624,  "solver1explicitTests": 1.2041199826559248,  "solver1implicitTests": 1.2304489213782739,  "solver2queenPlacements": 0.1,  "solver2methodCalls": 0.47712125471966244,  "solver2squareReads": 0.6020599913279624,  "solver2explicitTests": 1.1139433523068367,  "solver2implicitTests": 0.9030899869919435},
	{"size": "2", "solver1queenPlacements": 1.0,  "solver1methodCalls": 1.2041199826559248,  "solver1squareReads": 1.662757831681574,  "solver1explicitTests": 2.130333768495006,  "solver1implicitTests": 2.041392685158225,  "solver2queenPlacements": 1.0,  "solver2methodCalls": 1.2041199826559248,  "solver2squareReads": 1.662757831681574,  "solver2explicitTests": 2.0644579892269186,  "solver2implicitTests": 1.5797835966168101},
	{"size": "3", "solver1queenPlacements": 2.110589710299249,  "solver1methodCalls": 2.3283796034387376,  "solver1squareReads": 2.8998205024270964,  "solver1explicitTests": 3.2390490931401916,  "solver1implicitTests": 3.1749315935284423,  "solver2queenPlacements": 2.110589710299249,  "solver2methodCalls": 2.3283796034387376,  "solver2squareReads": 2.8998205024270964,  "solver2explicitTests": 3.2013971243204513,  "solver2implicitTests": 2.61066016308988},
	{"size": "4", "solver1queenPlacements": 3.400710636773231,  "solver1methodCalls": 3.6372895476781744,  "solver1squareReads": 4.312262005983347,  "solver1explicitTests": 4.560635818678364,  "solver1implicitTests": 4.525265139380899,  "solver2queenPlacements": 3.400710636773231,  "solver2methodCalls": 3.6372895476781744,  "solver2squareReads": 4.312262005983347,  "solver2explicitTests": 4.542389669540937,  "solver2implicitTests": 3.88394519503428},
	{"size": "5", "solver1queenPlacements": 4.8350878472324945,  "solver1methodCalls": 5.084737097962795,  "solver1squareReads": 5.860187025558374,  "solver1explicitTests": 6.055365332930141,  "solver1implicitTests": 6.037897591308276,  "solver2queenPlacements": 4.8350878472324945,  "solver2methodCalls": 5.084737097962795,  "solver2squareReads": 5.860187025558374,  "solver2explicitTests": 6.045159794259578,  "solver2implicitTests": 5.331147800363139},
	{"size": "6", "solver1queenPlacements": 6.378669477211044,  "solver1methodCalls": 6.6374187756089364,  "solver1squareReads": 7.503656466686411,  "solver1explicitTests": 7.664378828076926,  "solver1implicitTests": 7.657762498472013,  "solver2queenPlacements": 6.378669477211044,  "solver2methodCalls": 6.6374187756089364,  "solver2squareReads": 7.503656466686411,  "solver2explicitTests": 7.6576997764298955,  "solver2implicitTests": 6.893889480210125},
	{"size": "7", "solver1queenPlacements": 8.008697276815294,  "solver1methodCalls": 8.273980937567645,  "solver1squareReads": 9.220314780287609,  "solver1explicitTests": 9.356563780270248,  "solver1implicitTests": 9.356790164519534,  "solver2queenPlacements": 8.008697276815294,  "solver2methodCalls": 8.273980937567645,  "solver2squareReads": 9.220314780287609,  "solver2explicitTests": 9.35174515006043,  "solver2implicitTests": 8.542375184927572},
	{"size": "8", "solver1queenPlacements": 9.710173198417113,  "solver1methodCalls": 9.980313634397985,  "solver1squareReads": 10.996915638198928,  "solver1explicitTests": 11.114798065696673,  "solver1implicitTests": 11.119446858344201,  "solver2queenPlacements": 9.710173198417113,  "solver2methodCalls": 9.980313634397985,  "solver2squareReads": 10.996915638198928,  "solver2explicitTests": 11.111146813940177,  "solver2implicitTests": 10.259782458480862}
	];
//Data
var prefixlogData = { "0.1": 1,  "4.312262005983347": 20524,  "9.710173198417113": 5130659560,  "0.6020599913279624": 4,  "11.114798065696673": 130256098465,  "6.055365332930141": 1135966,  "6.378669477211044": 2391495,  "6.045159794259578": 1109583,  "7.664378828076926": 46172015,  "2.0644579892269186": 116,  "4.525265139380899": 33517,  "3.88394519503428": 7655,  "2.8998205024270964": 794,  "1.2041199826559248": 16,  "5.084737097962795": 121545,  "8.542375184927572": 348638372,  "8.273980937567645": 187923433,  "7.6576997764298955": 45467364,  "9.980313634397985": 9556825020,  "4.8350878472324945": 68405,  "4.560635818678364": 36361,  "6.037897591308276": 1091183,  "5.860187025558374": 724748,  "3.400710636773231": 2516,  "2.3283796034387376": 213,  "7.503656466686411": 31890143,  "2.041392685158225": 110,  "0.9030899869919435": 8,  "1.662757831681574": 46,  "9.35174515006043": 2247735217,  "0.47712125471966244": 3,  "1.2304489213782739": 17,  "8.008697276815294": 102022809,  "9.356790164519534": 2273998451,  "1": 10,  "10.259782458480862": 18187895844,  "5.331147800363139": 214362,  "10.996915638198928": 99292315414,  "2.110589710299249": 129,  "9.220314780287609": 1660790226,  "1.1139433523068367": 13,  "11.119446858344201": 131657880209,  "2.130333768495006": 135,  "6.6374187756089364": 4339291,  "11.111146813940177": 129165584613,  "9.356563780270248": 2272813395,  "3.1749315935284423": 1496,  "6.893889480210125": 7832303,  "7.657762498472013": 45473931,  "2.61066016308988": 408,  "3.6372895476781744": 4338,  "3.2013971243204513": 1590,  "1.5797835966168101": 38,  "4.542389669540937": 34865,  "3.2390490931401916": 1734	};
var prefixqueenPlacementsGraph = Morris.Line({
	element: 'prefixqueenPlacements',
	hideHover: 'auto',
	data: prefixdata,
	xkey: 'size',
	ykeys: ['solver1queenPlacements', 'solver2queenPlacements'],
	labels: ['Array brute-force', 'One-dimension array brute-force'],
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
	labels: ['Array brute-force', 'One-dimension array brute-force'],
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
	labels: ['Array brute-force', 'One-dimension array brute-force'],
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
	labels: ['Array brute-force', 'One-dimension array brute-force'],
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
	labels: ['Array brute-force', 'One-dimension array brute-force'],
	resize: true,
	parseTime: false,
	lineColors: ['#A52A2A', '#72A0C1'],
	yLabelFormat: function(y) { if( prefixlogData[y] ) { return prefixlogData[y].toLocaleString(); } else { if( 0 == y ) { return 0; } return "10^" + y;} },
	xLabelFormat: function(obj) { return (obj.x + 1).toLocaleString(); },
});

</script>

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

Next optimisations are tested in the part 4, click on the link below.
