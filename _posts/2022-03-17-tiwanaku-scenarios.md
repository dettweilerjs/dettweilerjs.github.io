---
layout: post
title: "Tiwanaku Scenarios"
date: 2022-04-10
category: posts
tags: [board games, games]
---

<div id="grid-holder"></div>
<script src="/public/p5.min.js"></script>
<script type="text/javascript">

	var grid;
	var farms = new Array();
	var cols = 9; //can be 5 or 9
	var rows = 5;
	var tries = 0;
	var numHints = cols;
	var hints = new Array();
	var desertTilesRemaining = 0;
	var forestTilesRemaining = 0;
	var mountainTilesRemaining = 0;
	var valleyTilesRemaining = 0;
	var size = 81; //height and width of a cell, in pixels
	var inBetween = 3; //buffer in between cells

	var bkgSmall;
	var bkgLarge;
	var desert;
	var forest;
	var mountain;
	var valley;
	var cropOne;
	var cropTwo;
	var cropThree;
	var cropFour;
	var cropFive;

	function preload()
	{
		bkgSmall = loadImage("/public/images/tiwanaku/boardSmall.png");
		bkgLarge = loadImage("/public/images/tiwanaku/boardLarge.png");

		desert = loadImage("/public/images/tiwanaku/desert.png");
		forest = loadImage("/public/images/tiwanaku/forest.png");
		mountain = loadImage("/public/images/tiwanaku/mountain.png");
		valley = loadImage("/public/images/tiwanaku/valley.png");

		cropOne = loadImage("/public/images/tiwanaku/1sweetpotato.png");
		cropTwo = loadImage("/public/images/tiwanaku/2cocaleaf.png");
		cropThree = loadImage("/public/images/tiwanaku/3chili.png");
		cropFour = loadImage("/public/images/tiwanaku/4corn.png");
		cropFive = loadImage("/public/images/tiwanaku/5quinoa.png");
	}

	function Cell(i, j, h)
	{
		this.i = i;
		this.j = j;
		this.x = i * size + ((i + 1) * inBetween);
		this.y = j * size + ((j + 1) * inBetween);
		this.h = h;
		this.isInFarm = false;
		this.land = 'none';
		this.crop = 0;
		this.solutions = new Array(); //array of possible crops that could be on this Cell; used in solveGrid()
		this.solved = 0;
		this.explored = false;
		this.planted = false;
	}

	Cell.prototype.show = function()
	{
		var crops = [cropOne, cropTwo, cropThree, cropFour, cropFive];
		if (this.explored)
		{
			if (this.land == 'desert')
			{
				image(desert, this.x, this.y, this.h, this.h);
			}
			else if (this.land == 'forest')
			{
				image(forest, this.x, this.y, this.h, this.h);
			}
			else if (this.land == 'mountain')
			{
				image(mountain, this.x, this.y, this.h, this.h);
			}
			else if (this.land == 'valley')
			{
				image(valley, this.x, this.y, this.h, this.h);
			}

			if (this.planted)
			{
				image(crops[this.crop - 1], this.x + 2, this.y + 2, this.h - 4, this.h - 4);
			}
		}
	}

	function Farm(cells)
	{
		this.contains = new Array(); //array of Cell objects that make up this farm
		this.crops = new Array(); //array of crops currently on this farm
		this.cells = cells; //number of cells this farm should be
		this.finished = false; //whether or not this farm is complete yet
	}

	function make2DArray(cols, rows)
	{
		var arr = new Array(cols);
		for (var i = 0; i < arr.length; i++)
		{
			arr[i] = new Array(rows);
		}
		return arr;
	}

	function setup()
	{
		var canvas = createCanvas((cols * size + ((cols + 1) * inBetween) + size * 3), (rows * size + ((rows + 1) * inBetween) + (inBetween * 3) + size));
		canvas.parent('grid-holder');

		grid = make2DArray(cols, rows);
		for (var i = 0; i < cols; i++)
		{
			for (var j = 0; j < rows; j++)
			{
				grid[i][j] = new Cell(i, j, size);
			}
		}
		numHints = numHints + (floor(random(5)) - 2); //up to 2 less or 2 more than cols
		plantGrid();
		makeFarms();
		colorFarms();
		solveGrid();
		stackTiles();
	}

	function plantGrid()
	{
		var prevCount = rows * cols;
		var nextCount = 0;
		for (var k = 1; k < 6; k++)
		{
			var possible = true;
			while (possible)
			{
				for (var i = 0; i < cols; i++)
				{
					for (var j = 0; j < rows; j++)
					{
						if (grid[i][j].crop == 0 && random(1) > 0.5)
						{
							if (plantCell(i, j, k))
							{
								prevCount--;
								nextCount++;
							}
						}
					}
				}
				possible = !isFull(k);
			}
			if (prevCount < 0)
			{
				eraseGrid();
				plantGrid();
				break;
			}
			prevCount = nextCount;
			nextCount = 0;
		}
		var startAgain = false;
		for (var i = 0; i < cols; i++)
		{
			for (var j = 0; j < rows; j++)
			{
				if (grid[i][j].crop == 0)
				{
					startAgain = true;
					eraseGrid();
					plantGrid();
					break;
				}
			}
			if (startAgain)
			{
				break;
			}
		}
	}

	function eraseGrid()
	{
		for (var i = 0; i < cols; i++)
		{
			for (var j = 0; j < rows; j++)
			{
				grid[i][j].crop = 0;
			}
		}
	}

	function eraseFarms()
	{
		farms = new Array();
		for (var i = 0; i < cols; i++)
		{
			for (var j = 0; j < rows; j++)
			{
				grid[i][j].isInFarm = false;
			}
		}
	}

	function clearColors()
	{
		for (var i = 0; i < cols; i++)
		{
			for (var j = 0; j < rows; j++)
			{
				grid[i][j].land = 'none';
			}
		}
	}

	function clearSolutions()
	{
		for (var i = 0; i < cols; i++)
		{
			for (var j = 0; j < rows; j++)
			{
				grid[i][j].solved = 0;
			}
		}
	}

	//makes new farms from crops that are not part of farms yet
	function makeFarms()
	{
		//establish new farms starting with all 5 crops, then all available 4 crops, and so on
		for (var k = 5; k > 0; k--)
		{
			for (var i = 0; i < cols; i++)
			{
				for (var j = 0; j < rows; j++)
				{
					var cell = grid[i][j];
					if (cell.crop == k && !cell.isInFarm)
					{
						cell.isInFarm = true;
						var farm = new Farm(k);
						farm.contains.push(cell);
						farm.crops.push(k);
						farms.push(farm);
					}
				}
			}
			buildFarms(k);
		}
		//if every farm is not finished (i.e. doesn't contain 1 of every crop up to its size), erase and restart
		var done = true;
		for (var i = 0; i < farms.length; i++)
		{
			if (!farms[i].finished)
			{
				done = false;
			}
		}
		if (!done)
		{
			tries++;
			if (tries == cols)
			{
				tries = 0;
				eraseGrid();
				plantGrid();
			}
			eraseFarms();
			makeFarms();
		}
		tries = 0; //reset tries after makeFarms completes successfully
	}

	//finishes any incomplete farms by adding one cell to each farm at a time, 'repeats' times
	function buildFarms(repeats)
	{
		for (var k = 0; k < repeats; k++)
		{
			// for each farm in farms
			for (var i = 0; i < farms.length; i++)
			{
				var active = farms[i];
				if (active.cells == active.contains.length)
				{
					active.finished = true;
				}
				if (!active.finished)
				{
					var options = getAllFarmlessNeighbors(active);
					options = shuffle(options);
					//try adding all farmless neighbors until one works
					for (var j = 0; j < options.length; j++)
					{
						var chosen = options[j];
						if (!active.crops.includes(chosen.crop))
						{
							chosen.isInFarm = true;
							if (isOkay())
							{
								active.contains.push(chosen);
								active.crops.push(chosen.crop);
								break;
							}
							else // not okay
							{
								chosen.isInFarm = false;
							}
						}
					}
				}
			}
		}
	}

	//checks each cell on the grid that isn't yet in a farm to make sure it is not closed off from becoming a farm
	function isOkay()
	{
		for (var i = 0; i < cols; i++)
		{
			for (var j = 0; j < rows; j++)
			{
				//checks crop > 1 because if a cell has a crop of 1, it can be its own farm without needing any neighbors
				if ((!grid[i][j].isInFarm) && grid[i][j].crop > 1 && getFarmlessNeighbors(i, j).length == 0)
				{
					return false;
				}
			}
		}
		return true;
	}

	function colorFarms()
	{
		for (var i = 0; i < farms.length; i++)
		{
			var terrain = ['desert','forest','mountain','valley'];
			var active = farms[i];
			var neighbors = getAllAdjacent(active);
			for (var j = 0; j < neighbors.length; j++)
			{
				if (terrain.includes(neighbors[j].land))
				{
					terrain.splice(terrain.indexOf(neighbors[j].land), 1);
				}
			}
			terrain = shuffle(terrain);
			for (var j = 0; j < active.contains.length; j++)
			{
				active.contains[j].land = terrain[0];
			}
			if (terrain.length == 0)
			{
				clearColors();
				colorFarms();
				break;
			}
		}
	}

	//given a cell at grid coordinates i, j and a crop, attempt to place that crop in that cell. returns false if illegal
	function plantCell(i, j, crop)
	{
		var adj = getAdjacentCrops(i, j);
		if (adj.includes(crop))
		{
			return false;
		}
		else
		{
			grid[i][j].crop = crop;
			return true;
		}
	}

	//returns false if it is possible to legally place any more of crop on the grid; otherwise, returns true
	function isFull(crop)
	{
		for (var i = 0; i < cols; i++)
		{
			for (var j = 0; j < rows; j++)
			{
				if (grid[i][j].crop == 0 && !(getAdjacentCrops(i, j).includes(crop)))
				{
					return false;
				}
			}
		}
		return true;
	}

	//for a given farm, return an array of cells that surround that farm (including diagonally)
	function getAllAdjacent(farm)
	{
		var adjacent = new Array();
		var cells = farm.contains
		for (var k = 0; k < cells.length; k++)
		{
			adjacent = adjacent.concat(getAdjacent(cells[k].i, cells[k].j));
		}
		return [...new Set(adjacent)]; //casting to a Set and back removes duplicate cells
	}

	//for a cell at given grid coordinates, return an array of cells that surround that cell (including diagonally)
	function getAdjacent(i, j)
	{
		var x;
		var y;
		var adj = new Array();
		for (var xOff = -1; xOff < 2; xOff++)
		{
			for (var yOff = -1; yOff < 2; yOff++)
			{
				x = i + xOff;
				y = j + yOff;
				if (x > -1 && x < cols && y > -1 && y < rows && !(xOff == 0 && yOff == 0))
				{
					adj.push(grid[x][y]);
				}
			}
		}
		return adj;
	}

	//for a given farm, return orthogonally adjacent cells that are not part of any farm yet
	function getAllFarmlessNeighbors(farm)
	{
		var neighbors = new Array();
		var cells = farm.contains
		for (var k = 0; k < cells.length; k++)
		{
			neighbors = neighbors.concat(getFarmlessNeighbors(cells[k].i, cells[k].j));
		}
		return [...new Set(neighbors)]; //remove duplicate cells
	}

	//for a cell at given grid coordinates, return orthogonally adjacent cells that are not part of any farm yet
	function getFarmlessNeighbors(i, j)
	{
		var list = [i - 1, j, i + 1, j, i, j - 1, i, j + 1]; //the four orthogonal offsets
		var neighbors = new Array();
		while (list.length > 0)
		{
			if (list[0] > -1 && list[0] < cols && list[1] > -1 && list[1] < rows)
			{
				if (!grid[list[0]][list[1]].isInFarm)
				{
					neighbors.push(grid[list[0]][list[1]]);
				}
			}
			list.splice(0, 2); //after checking, remove from the list
		}
		return neighbors;
	}

	function getAdjacentCrops(i, j)
	{
		var x;
		var y;
		var adj = new Array();
		for (var xOff = -1; xOff < 2; xOff++)
		{
			for (var yOff = -1; yOff < 2; yOff++)
			{
				x = i + xOff;
				y = j + yOff;
				if (x > -1 && x < cols && y > -1 && y < rows && grid[x][y].crop > 0)
				{
					adj.push(grid[x][y].crop);
				}
			}
		}
		return [...new Set(adj)]; //remove duplicate crops
	}

	function solveGrid()
	{
		farms.reverse();
		for (var i = 0; i < farms.length; i++)
		{
			var active = farms[i];
			for (var j = 0; j < active.contains.length; j++)
			{
				var cell = active.contains[j];
				cell.solutions = active.crops.slice(0).sort(); //initialize possible solution in each cell as the crops of that cell's farm
			}
		}
		if (hints.length == 0)
		{
			makeHints();
		}
		//mark the hint cells as solved
		for (var i = 0; i < hints.length; i++)
		{
			solveCell(hints[i], hints[i].crop);
			hints[i].explored = true;
			hints[i].planted = true;
		}

		// the three-step solving process
		for (var z = 0; z < cols; z++) //try the process cols times
		{
			//step 1: remove possible solutions that are already planted somewhere else on the same farm
			for (var i = 0; i < farms.length; i++)
			{
				var farm = farms[i].contains;
				for (var j = 0; j < farm.length; j++)
				{
					var cell = farm[j];
					if (cell.solved > 0)
					{
						for (var k = 0; k < farm.length; k++)
						{
							if ((k != j) && farm[k].solutions.includes(cell.solved))
							{
								farm[k].solutions.splice(farm[k].solutions.indexOf(cell.solved), 1);
							}
						}
					}
				}
			}

			//step 2: for each cell, if it only has one possible solution, solve that cell
			for (var i = 0; i < cols; i++)
			{
				for (var j = 0; j < rows; j++)
				{
					var cell = grid[i][j];
					if (cell.solutions.length == 1 && cell.solved == 0) //if there's only one possible solution for the cell
					{
						solveCell(cell, cell.solutions[0]);
					}
				}
			}

			//step 3: for each cell on a farm, if it contains the only possibility for any of that farm's crops, solve that cell
			for (var i = 0; i < farms.length; i++)
			{
				var farm = farms[i].contains;
				for (var j = 0; j < farm.length; j++)
				{
					var cell = farm[j];
					for (var k = 0; k < cell.solutions.length; k++)
					{
						var only = true;
						for (var m = 0; m < farm.length; m++)
						{
							if (farm[m].solutions.includes(cell.solutions[k]) && (m != j))
							{
								only = false;
							}
						}
						if (only && cell.solved == 0)
						{
							solveCell(cell, cell.solutions[k]);
						}
					}
				}
			}
		}

		//after attempting to solve, check if the grid is solved
		var done = true;
		for (var i = 0; i < cols; i++)
		{
			for (var j = 0; j < rows; j++)
			{
				if (grid[i][j].solved == 0)
				{
					done = false;
					break;
				}
			}
			if (!done)
			{
				break;
			}
		}

		//if not, replace random hints with some cells that aren't solved, then try to solve again
		if (!done)
		{
			hints = shuffle(hints);
			for (var i = 0; i < farms.length; i++)
			{
				var farm = farms[i].contains;
				farm = shuffle(farm);
				for (var j = 0; j < farm.length; j++)
				{
					if (farm[j].solved == 0)
					{
						var removedHint = hints.pop(); //remove a hint from the end of the list
						removedHint.explored = false;
						removedHint.planted = false;
						hints.splice(0, 0, farm[j]); //add the new hint to the beginning of the list
						break;
					}
				}
			}

			tries++;
			if (tries == cols) //nuke the grid and start again :)
			{
				tries = 0;
				for (var i = 0; i < hints.length; i++)
				{
					hints[i].explored = false;
					hints[i].planted = false;
				}
				hints = new Array();
				eraseGrid();
				eraseFarms();
				clearColors();
				plantGrid();
				makeFarms();
				colorFarms();
			}
			clearSolutions();
			solveGrid();
		}
	}

	function solveCell(cell, solution)
	{
		if (cell.solved == 0) //if cell is not already solved
		{
			cell.solved = solution;
			cell.solutions = [solution];
			var adj = getAdjacent(cell.i, cell.j);
			for (var k = 0; k < adj.length; k++)
			{
				if (adj[k].solutions.includes(cell.solved))
				{
					adj[k].solutions.splice(adj[k].solutions.indexOf(cell.solved), 1);
				}
			}
		}
	}

	function makeHints()
	{
		for (var i = 0; i < numHints; i++)
		{
			var next = grid[floor(random(cols))][floor(random(rows))];
			while (next.solved > 0)
			{
				next = grid[floor(random(cols))][floor(random(rows))];
			}
			hints.push(next);
		}
	}

	function stackTiles()
	{
		desertTilesRemaining = 0;
		forestTilesRemaining = 0;
		mountainTilesRemaining = 0;
		valleyTilesRemaining = 0;
		for (var i = 0; i < cols; i++)
		{
			for (var j = 0; j < rows; j++)
			{
				var cell = grid[i][j];
				if (!cell.explored)
				{
					if (cell.land == 'desert')
					{
						desertTilesRemaining++;
					}
					else if (cell.land == 'forest')
					{
						forestTilesRemaining++;
					}
					else if (cell.land == 'mountain')
					{
						mountainTilesRemaining++;
					}
					else if (cell.land == 'valley')
					{
						valleyTilesRemaining++;
					}
				}
			}
		}
	}

	//randomly arrange a given array
	function shuffle(arr)
	{
		var counter = arr.length;
		while (counter > 0)
		{
			var index = floor(random(counter)); //pick a random index between 0 and counter-1
			counter--;
			//swap arr[index] and arr[counter]
			var temp = arr[counter];
			arr[counter] = arr[index];
			arr[index] = temp;
		}
		return arr;
	}

	//extends the p5.js function mousePressed to check when a cell is clicked on
	function mousePressed()
	{
		var cell = grid[floor(mouseX / (size + inBetween))][floor(mouseY / (size + inBetween))];
		if (!cell.explored)
		{
			cell.explored = true;
			stackTiles();
			clear();
		}
		else
		{
			if (!cell.planted)
			{
				cell.planted = true;
			}
		}
	}

	function newLargeBoard()
	{
		cols = 9;
		numHints = cols;
		setup();
	}

	function newSmallBoard()
	{
		cols = 5;
		numHints = cols;
		setup();
	}

	function drawRemaining(tile, remaining, overlap, offset)
	{
		image(tile, offset, ((size * rows) + ((rows + 4) * inBetween)), size, size);
		for (var i = 1; i < remaining; i++)
		{
			image(tile, offset + (i * overlap), ((size * rows) + ((rows + 4) * inBetween)), size, size);
		}
		if (remaining == 0)
		{
			offset = offset + overlap;
		}

		textSize(floor(size / 4));
		textStyle(BOLD);
		textAlign(CENTER, CENTER);
		text(remaining, (offset + ((remaining - 1) * overlap) + (floor(size / 2))), ((size * rows) + ((rows + 4) * inBetween) + floor(size / 2)));
		return offset + ((remaining - 1) * overlap) + size + (inBetween * 3);
	}

	function drawBkg()
	{
		if (cols == 5)
		{
			image(bkgSmall, 0, 0, (cols * size + ((cols + 1) * inBetween)), (rows * size + ((rows + 1) * inBetween)));
		}
		else if (cols == 9)
		{
			image(bkgLarge, 0, 0, (cols * size + ((cols + 1) * inBetween)), (rows * size + ((rows + 1) * inBetween)));
		}
	}
	
	function draw()
	{
		drawBkg();

		for (var i = 0; i < cols; i++)
		{
			for (var j = 0; j < rows; j++)
			{
				grid[i][j].show();
			}
		}

		var overlap = floor(size / 5);
		var offset = 0;

		offset = drawRemaining(desert, desertTilesRemaining, overlap, offset);
		offset = drawRemaining(forest, forestTilesRemaining, overlap, offset);
		offset = drawRemaining(mountain, mountainTilesRemaining, overlap, offset);
		offset = drawRemaining(valley, valleyTilesRemaining, overlap, offset);
	}

</script>
<button type="button" onclick="newLargeBoard()">New Large Board</button>
<button type="button" onclick="newSmallBoard()">New Small Board</button>

[Tiwanaku](https://boardgamegeek.com/boardgame/267979/tiwanaku) is a puzzle-style competitive deduction game that recently funded on Kickstarter. It is played on a grid of square tiles, either the small 5 by 5 or the large 5 by 9. I will not explain all the rules here, since you can find them yourself if you're interested, but it is important that when you play Tiwananku, you are given a hidden answer key that tells you the correct answer when you attempt to deduce a tile. Above is my implementation of that answer key.

So let me explain how the grid is laid out. There are four types of square terrain tile, represented by four colors (and edge shapes on the tile): yellow deserts, green forests, blue mountains, and red valleys. These tiles are grouped into regions on the grid. A region is a group of 1-5 terrain tiles of the same type, orthogonally adjacent to each other. A region will never be larger than 5 tiles, nor will it touch, orthogonally or diagonally, another region of the same color.

There are crops to be planted in each region, represented by the circular tiles of different sizes on top of the terrain tiles. Each region will have a different crop on each tile, from 1 up to the size of the region - so a size 4 region will have a 1, 2, 3, and 4 crop on it, in a random arrangement. Furthermore, no two of the same crop will ever be adjacent orthogonally or diagonally.

Lastly, how to use the answer key. The board is laid out with a number of pre-filled "hints", where both the terrain and the crop are revealed. There are also stacks of tiles below the board, showing how many of each type will be used when the board is full. You can click on any grid space to reveal what terrain tile should be there, and click on any placed terrain tile to reveal what crop should be there. The grid is set up in such a way that there will only be one possible layout of crops once all of them are planted.

Good luck, and have fun!