---
layout: post
title: "Tobago Board Generator"
date: 2020-12-16
category: posts
tags: [board games, games]
---

<script type="text/javascript" src="/public/jsgl.min.js"></script> 
<div id="panel" style="width: 640px; height: 480px"></div>
<script type="text/javascript">
	var myPanel = new jsgl.Panel(document.getElementById("panel"));
	var Volcano = false;

	function generateBoard()
	{
		myPanel.clear();
		
		var lowerA = { name: "a", board: [
			{ x: 0,  y: 6, z: -6, isShore: 1, type: 'mountains' },
			{ x: -1, y: 6, z: -5, isShore: 1, type: 'beach'     },
			{ x: -2, y: 6, z: -4, isShore: 1, type: 'beach'     },
			{ x: 1,  y: 5, z: -6, isShore: 1, type: 'beach'     },
			{ x: 0,  y: 5, z: -5, isShore: 0, type: 'jungle'    },
			{ x: -1, y: 5, z: -4, isShore: 0, type: 'jungle'    },
			{ x: -2, y: 5, z: -3, isShore: 1, type: 'jungle'    },
			{ x: -3, y: 5, z: -2, isShore: 1, type: 'beach'     },
			{ x: 2,  y: 4, z: -6, isShore: 1, type: 'beach'     },
			{ x: 1,  y: 4, z: -5, isShore: 1, type: 'jungle'    },
			{ x: 0,  y: 4, z: -4, isShore: 1, type: 'jungle'    },
			{ x: -1, y: 4, z: -3, isShore: 1, type: 'beach'     },
			{ x: 0,  y: 3, z: -3, isShore: 1, type: 'jungle'    },
			{ x: -1, y: 3, z: -2, isShore: 1, type: 'beach'     },
			{ x: -2, y: 3, z: -1, isShore: 1, type: 'beach'     },
			{ x: -3, y: 3, z: 0,  isShore: 1, type: 'scrubland' },
			{ x: 2,  y: 2, z: -4, isShore: 1, type: 'river'     },
			{ x: 1,  y: 2, z: -3, isShore: 1, type: 'mountains' },
			{ x: 0,  y: 2, z: -2, isShore: 0, type: 'mountains' },
			{ x: -1, y: 2, z: -1, isShore: 0, type: 'mountains' },
			{ x: -2, y: 2, z: 0,  isShore: 0, type: 'scrubland' },
			{ x: 2,  y: 1, z: -3, isShore: 1, type: 'river'     },
			{ x: 1,  y: 1, z: -2, isShore: 0, type: 'river'     },
			{ x: 0,  y: 1, z: -1, isShore: 0, type: 'lake'      },
			{ x: -1, y: 1, z: 0,  isShore: 0, type: 'scrubland' },
			{ x: 3,  y: 0, z: -3, isShore: 1, type: 'river'     },
			{ x: 2,  y: 0, z: -2, isShore: 0, type: 'river'     },
			{ x: 1,  y: 0, z: -1, isShore: 0, type: 'lake'      },
			{ x: 0,  y: 0, z: 0,  isShore: 0, type: 'lake'      }
			] };
			
		var upperA = { name: "A", board: [
			{ x: 0,  y: 6, z: -6, isShore: 1, type: 'mountains' },
			{ x: -1, y: 6, z: -5, isShore: 1, type: 'mountains' },
			{ x: 1,  y: 5, z: -6, isShore: 1, type: 'mountains' },
			{ x: 0,  y: 5, z: -5, isShore: 0, type: 'mountains' },
			{ x: -1, y: 5, z: -4, isShore: 1, type: 'jungle'    },
			{ x: -2, y: 5, z: -3, isShore: 1, type: 'jungle'    },
			{ x: -3, y: 5, z: -2, isShore: 1, type: 'beach'     },
			{ x: 2,  y: 4, z: -6, isShore: 1, type: 'mountains' },
			{ x: 1,  y: 4, z: -5, isShore: 0, type: 'mountains' },
			{ x: 0,  y: 4, z: -4, isShore: 0, type: 'jungle'    },
			{ x: -1, y: 4, z: -3, isShore: 0, type: 'jungle'    },
			{ x: -2, y: 4, z: -2, isShore: 0, type: 'jungle'    },
			{ x: -3, y: 4, z: -1, isShore: 1, type: 'beach'     },
			{ x: -4, y: 4, z: 0,  isShore: 1, type: 'scrubland' },
			{ x: 2,  y: 3, z: -5, isShore: 1, type: 'mountains' },
			{ x: 1,  y: 3, z: -4, isShore: 0, type: 'jungle'    },
			{ x: 0,  y: 3, z: -3, isShore: 0, type: 'lake'      },
			{ x: -1, y: 3, z: -2, isShore: 0, type: 'lake'      },
			{ x: -2, y: 3, z: -1, isShore: 0, type: 'jungle'    },
			{ x: -3, y: 3, z: 0,  isShore: 3, type: 'scrubland' },
			{ x: 3,  y: 2, z: -5, isShore: 1, type: 'mountains' },
			{ x: 2,  y: 2, z: -4, isShore: 1, type: 'jungle'    },
			{ x: 1,  y: 2, z: -3, isShore: 0, type: 'jungle'    },
			{ x: 0,  y: 2, z: -2, isShore: 0, type: 'lake'      },
			{ x: -1, y: 2, z: -1, isShore: 0, type: 'lake'      },
			{ x: -2, y: 2, z: 0,  isShore: 0, type: 'mountains' },
			{ x: 2,  y: 1, z: -3, isShore: 1, type: 'river'     },
			{ x: 1,  y: 1, z: -2, isShore: 0, type: 'river'     },
			{ x: 0,  y: 1, z: -1, isShore: 0, type: 'lake'      },
			{ x: -1, y: 1, z: 0,  isShore: 0, type: 'mountains' },
			{ x: 3,  y: 0, z: -3, isShore: 1, type: 'scrubland' },
			{ x: 2,  y: 0, z: -2, isShore: 0, type: 'scrubland' },
			{ x: 1,  y: 0, z: -1, isShore: 0, type: 'mountains' },
			{ x: 0,  y: 0, z: 0,  isShore: 0, type: 'mountains' }
			] };

		var lowerB = { name: "b", board: [
			{ x: 3,  y: 0, z: -3, isShore: 1, type: 'scrubland' },
			{ x: 3,  y: 2, z: -5, isShore: 1, type: 'beach'     },
			{ x: 2,  y: 0, z: -2, isShore: 1, type: 'scrubland' },
			{ x: 2,  y: 2, z: -4, isShore: 1, type: 'scrubland' },
			{ x: 2,  y: 3, z: -5, isShore: 1, type: 'beach'     },
			{ x: 1,  y: 0, z: -1, isShore: 0, type: 'lake'      },
			{ x: 1,  y: 1, z: -2, isShore: 1, type: 'scrubland' },
			{ x: 1,  y: 2, z: -3, isShore: 1, type: 'scrubland' },
			{ x: 1,  y: 3, z: -4, isShore: 1, type: 'scrubland' },
			{ x: 1,  y: 4, z: -5, isShore: 1, type: 'beach'     },
			{ x: 0,  y: 0, z: 0,  isShore: 0, type: 'lake'      },
			{ x: 0,  y: 1, z: -1, isShore: 0, type: 'lake'      },
			{ x: 0,  y: 2, z: -2, isShore: 0, type: 'river'     },
			{ x: 0,  y: 3, z: -3, isShore: 1, type: 'river'     },
			{ x: -1, y: 1, z: 0,  isShore: 0, type: 'lake'      },
			{ x: -1, y: 2, z: -1, isShore: 0, type: 'mountains' },
			{ x: -1, y: 3, z: -2, isShore: 0, type: 'jungle'    },
			{ x: -1, y: 4, z: -3, isShore: 1, type: 'jungle'    },
			{ x: -1, y: 5, z: -4, isShore: 1, type: 'jungle'    },
			{ x: -1, y: 6, z: -5, isShore: 1, type: 'jungle'    },
			{ x: -1, y: 7, z: -6, isShore: 1, type: 'jungle'    },
			{ x: -2, y: 2, z: 0,  isShore: 0, type: 'mountains' },
			{ x: -2, y: 3, z: -1, isShore: 0, type: 'mountains' },
			{ x: -2, y: 4, z: -2, isShore: 1, type: 'mountains' },
			{ x: -2, y: 5, z: -3, isShore: 1, type: 'mountains' },
			{ x: -2, y: 6, z: -4, isShore: 1, type: 'jungle'    },
			{ x: -2, y: 7, z: -5, isShore: 1, type: 'jungle'    },
			{ x: -3, y: 3, z: 0,  isShore: 3, type: 'scrubland' },
			{ x: -3, y: 4, z: -1, isShore: 1, type: 'scrubland' },
			{ x: -4, y: 4, z: 0,  isShore: 1, type: 'scrubland' },
			{ x: -4, y: 5, z: -1, isShore: 1, type: 'scrubland' }
			] };
			
		var upperB = { name: "B", board: [
			{ x: 4,  y: 0, z: -4, isShore: 1, type: 'scrubland' },
			{ x: 4,  y: 1, z: -5, isShore: 1, type: 'beach'     },
			{ x: 3,  y: 0, z: -3, isShore: 2, type: 'scrubland' },
			{ x: 3,  y: 1, z: -4, isShore: 0, type: 'scrubland' },
			{ x: 3,  y: 2, z: -5, isShore: 1, type: 'beach'     },
			{ x: 2,  y: 0, z: -2, isShore: 0, type: 'lake'      },
			{ x: 2,  y: 1, z: -3, isShore: 0, type: 'scrubland' },
			{ x: 2,  y: 2, z: -4, isShore: 0, type: 'scrubland' },
			{ x: 2,  y: 3, z: -5, isShore: 1, type: 'river'     },
			{ x: 1,  y: 0, z: -1, isShore: 0, type: 'lake'      },
			{ x: 1,  y: 1, z: -2, isShore: 0, type: 'lake'      },
			{ x: 1,  y: 2, z: -3, isShore: 0, type: 'river'     },
			{ x: 1,  y: 3, z: -4, isShore: 0, type: 'river'     },
			{ x: 1,  y: 4, z: -5, isShore: 1, type: 'jungle'    },
			{ x: 1,  y: 5, z: -6, isShore: 1, type: 'jungle'    },
			{ x: 0,  y: 0, z: 0,  isShore: 0, type: 'lake'      },
			{ x: 0,  y: 1, z: -1, isShore: 0, type: 'lake'      },
			{ x: 0,  y: 2, z: -2, isShore: 0, type: 'lake'      },
			{ x: 0,  y: 3, z: -3, isShore: 0, type: 'jungle'    },
			{ x: 0,  y: 4, z: -4, isShore: 0, type: 'jungle'    },
			{ x: 0,  y: 5, z: -5, isShore: 1, type: 'jungle'    },
			{ x: -1, y: 1, z: 0,  isShore: 0, type: 'mountains' },
			{ x: -1, y: 2, z: -1, isShore: 0, type: 'scrubland' },
			{ x: -1, y: 3, z: -2, isShore: 0, type: 'scrubland' },
			{ x: -1, y: 4, z: -3, isShore: 0, type: 'scrubland' },
			{ x: -1, y: 5, z: -4, isShore: 1, type: 'beach'     },
			{ x: -2, y: 2, z: 0,  isShore: 0, type: 'mountains' },
			{ x: -2, y: 3, z: -1, isShore: 0, type: 'scrubland' },
			{ x: -2, y: 4, z: -2, isShore: 0, type: 'scrubland' },
			{ x: -2, y: 5, z: -3, isShore: 1, type: 'beach'     },
			{ x: -3, y: 3, z: 0,  isShore: 1, type: 'mountains' },
			{ x: -3, y: 4, z: -1, isShore: 1, type: 'beach'     },
			{ x: -3, y: 5, z: -2, isShore: 1, type: 'beach'     }
			] };
			
		var lowerC = { name: "c", board: [
			{ x: 4, y: 0, z: -4, isShore: 1, type: 'river' },
			{ x: 3, y: 0, z: -3, isShore: 2, type: 'river' },
			{ x: 3, y: 1, z: -4, isShore: 1, type: 'beach' },
			{ x: 2, y: 0, z: -2, isShore: 0, type: 'river' },
			{ x: 2, y: 1, z: -3, isShore: 1, type: 'beach' },
			{ x: 2, y: 3, z: -5, isShore: 1, type: 'beach' },
			{ x: 1, y: 0, z: -1, isShore: 0, type: 'mountains' },
			{ x: 1, y: 1, z: -2, isShore: 0, type: 'river' },
			{ x: 1, y: 2, z: -3, isShore: 1, type: 'beach' },
			{ x: 1, y: 3, z: -4, isShore: 1, type: 'beach' },
			{ x: 1, y: 4, z: -5, isShore: 1, type: 'scrubland' },
			{ x: 1, y: 5, z: -6, isShore: 1, type: 'scrubland' },
			{ x: 0, y: 0, z: 0, isShore: 0, type: 'mountains' },
			{ x: 0, y: 1, z: -1, isShore: 0, type: 'scrubland' },
			{ x: 0, y: 2, z: -2, isShore: 0, type: 'lake' },
			{ x: 0, y: 3, z: -3, isShore: 0, type: 'lake' },
			{ x: 0, y: 4, z: -4, isShore: 1, type: 'scrubland' },
			{ x: -1, y: 1, z: 0, isShore: 0, type: 'scrubland' },
			{ x: -1, y: 2, z: -1, isShore: 0, type: 'mountains' },
			{ x: -1, y: 3, z: -2, isShore: 0, type: 'mountains' },
			{ x: -1, y: 4, z: -3, isShore: 1, type: 'beach' },
			{ x: -2, y: 2, z: 0, isShore: 0, type: 'scrubland' },
			{ x: -2, y: 3, z: -1, isShore: 1, type: 'mountains' },
			{ x: -2, y: 4, z: -2, isShore: 1, type: 'mountains' },
			{ x: -2, y: 5, z: -3, isShore: 1, type: 'beach' },
			{ x: -2, y: 6, z: -4, isShore: 1, type: 'mountains' },
			{ x: -3, y: 3, z: 0, isShore: 1, type: 'scrubland' }
			] };
			
		var upperC = { name: "C", board: [
			{ x: 4, y: 0, z: -4, isShore: 1, type: 'mountains' },
			{ x: 3, y: 0, z: -3, isShore: 2, type: 'mountains' },
			{ x: 3, y: 1, z: -4, isShore: 1, type: 'beach' },
			{ x: 3, y: 2, z: -5, isShore: 1, type: 'beach' },
			{ x: 3, y: 3, z: -6, isShore: 1, type: 'beach' },
			{ x: 2, y: 0, z: -2, isShore: 0, type: 'jungle' },
			{ x: 2, y: 1, z: -3, isShore: 0, type: 'jungle' },
			{ x: 2, y: 2, z: -4, isShore: 0, type: 'jungle' },
			{ x: 2, y: 3, z: -5, isShore: 0, type: 'jungle' },
			{ x: 2, y: 4, z: -6, isShore: 1, type: 'jungle' },
			{ x: 2, y: 5, z: -7, isShore: 1, type: 'jungle' },
			{ x: 1, y: 0, z: -1, isShore: 0, type: 'jungle' },
			{ x: 1, y: 1, z: -2, isShore: 0, type: 'jungle' },
			{ x: 1, y: 2, z: -3, isShore: 0, type: 'lake' },
			{ x: 1, y: 3, z: -4, isShore: 0, type: 'mountains' },
			{ x: 1, y: 4, z: -5, isShore: 0, type: 'mountains' },
			{ x: 1, y: 5, z: -6, isShore: 1, type: 'mountains' },
			{ x: 0, y: 0, z: 0, isShore: 0, type: 'jungle' },
			{ x: 0, y: 1, z: -1, isShore: 0, type: 'jungle' },
			{ x: 0, y: 2, z: -2, isShore: 0, type: 'lake' },
			{ x: 0, y: 3, z: -3, isShore: 0, type: 'lake' },
			{ x: 0, y: 4, z: -4, isShore: 0, type: 'mountains' },
			{ x: 0, y: 5, z: -5, isShore: 1, type: 'mountains' },
			{ x: -1, y: 1, z: 0, isShore: 0, type: 'scrubland' },
			{ x: -1, y: 2, z: -1, isShore: 0, type: 'scrubland' },
			{ x: -1, y: 3, z: -2, isShore: 0, type: 'river' },
			{ x: -1, y: 4, z: -3, isShore: 1, type: 'beach' },
			{ x: -1, y: 5, z: -4, isShore: 1, type: 'beach' },
			{ x: -1, y: 6, z: -5, isShore: 1, type: 'mountains' },
			{ x: -2, y: 2, z: 0, isShore: 0, type: 'scrubland' },
			{ x: -2, y: 3, z: -1, isShore: 0, type: 'river' },
			{ x: -2, y: 4, z: -2, isShore: 1, type: 'beach' },
			{ x: -2, y: 6, z: -4, isShore: 1, type: 'beach' },
			{ x: -3, y: 3, z: 0, isShore: 1, type: 'scrubland' },
			{ x: -3, y: 4, z: -1, isShore: 1, type: 'river' },
			{ x: -3, y: 5, z: -2, isShore: 1, type: 'beach' },
			{ x: -4, y: 5, z: -1, isShore: 1, type: 'river' }
			] };
		
		var boardB = pickBetween(lowerB, upperB);
		var boardC = pickBetween(lowerC, upperC);		
		var boardOrder = pickBetween([boardB, boardC], [boardC, boardB]);
		var board1 = pickBetween(lowerA, upperA);
		var board2 = boardOrder[0];
		var board3 = boardOrder[1];

		//Record the three middle hexes for volcano placement
		for (var i = 0; i < board1.board.length; i++)
		{
			if ((board1.board[i].x == 0) && (board1.board[i].y == 0) && (board1.board[i].z == 0))
				board1.board[i].middle = true;
		}
		for (var i = 0; i < board2.board.length; i++)
		{
			if ((board2.board[i].x == 0) && (board2.board[i].y == 0) && (board2.board[i].z == 0))
				board2.board[i].middle = true;
		}
		for (var i = 0; i < board3.board.length; i++)
		{
			if ((board3.board[i].x == 0) && (board3.board[i].y == 0) && (board3.board[i].z == 0))
				board3.board[i].middle = true;
		}
		
		rotateBoard(board2, 1);
		rotateBoard(board3, 2);
		
		var boardShift = Math.floor(Math.random() * 2);
						
		if (boardShift == 0)
		{
			shiftBoard(board2, 1, -1, 0);
			shiftBoard(board3, 0, -1, 1);
			
			drawLabel(board2.name, 234 + 30*8, 200 - 17*7);
			drawLabel(board3.name, 232 + 30, 200 + 17*16);
		}
		else
		{
			shiftBoard(board2, 0, -1, 1);
			shiftBoard(board3, -1, 0, 1);
			
			drawLabel(board2.name, 234 + 30*8, 200 - 17*5);
			drawLabel(board3.name, 232, 200 + 17*15);
		}
		
		drawLabel(board1.name, 230 - 30*7, 200 - 17*7);
		
		/* determine which hexes are shore pieces based on board configuration */
		determineShoreline(board1, board2, board3, boardShift);
		determineShoreline(board2, board3, board1, boardShift);
		determineShoreline(board3, board1, board2, boardShift);
		
		placeBoard(board1, 230, 200);
		placeBoard(board2, 234, 200);
		placeBoard(board3, 232, 203.46);
		
		drawBoard(board1);
		drawBoard(board2);
		drawBoard(board3);
		
		var wholeBoard = board1.board.concat(board2.board, board3.board);
		
		var innerBoard = [];
		for (var i = 0; i < wholeBoard.length; i++)
		{
			if (wholeBoard[i].isShore == 0)
			{
				innerBoard.push(wholeBoard[i]);
			}
		}
		
		if (Volcano)
		{
			var volCenter = placeVolcano(innerBoard, wholeBoard);
			drawVolcano(volCenter);
		}

		var huts = placeObjects(wholeBoard, 4);
		drawHuts(huts);
		
		var trees = placeObjects(wholeBoard, 3);
		drawTrees(trees);

		var statues = placeObjects(innerBoard, 3);
		drawStatues(statues);
	}
	
	function drawLabel(name, x, y)
	{
		var label = myPanel.createLabel();
		
		label.setText(name);
		label.setLocationXY(x, y);		
		label.setHorizontalAnchor(jsgl.HorizontalAnchor.CENTER);
		label.setVerticalAnchor(jsgl.VerticalAnchor.CENTER);
		label.setBold(true);
		label.setFontFamily("sans-serif");
		label.setFontSize(24);

		myPanel.addElement(label);
	}

	function placeVolcano(inner, board)
	{
		var volCenter = randomHex(inner);
		if (document.getElementById("centered").checked)
		{
			var centerHexes = [];
			for (var i = 0; i < inner.length; i++)
			{
				if (inner[i].middle)
					centerHexes.push(inner[i]);
			}
			volCenter = randomHex(centerHexes);
		}
		var hexes = [];
		for (var i = 0; i < board.length; i++)
		{
			if (distance(volCenter, board[i]) <= 1)
				hexes.push(board[i]);
		}
		for (var i = 0; i < hexes.length; i++)
		{
			board.splice(board.indexOf(hexes[i]), 1);
			inner.splice(inner.indexOf(hexes[i]), 1);
			drawHex(hexes[i].xLoc, hexes[i].yLoc, 'volcano')
		}
		return volCenter;
	}
	
	function placeObjects(board, num)
	{
		var finished = false;
		var hexes;
		
		while (!finished)
		{
			finished = true;
		
			hexes = [];
			for (var i = 0; i < num; i++)
			{
				hexes.push(randomHex(board));
			}
			
			for (var i = 0; i < num; i++)
			{
				for (var j = i + 1; j < num; j++)
				{ // must be 4 or more spaces apart
					if (distance(hexes[i], hexes[j]) <= 3)
						finished = false;
				}
			}
				
			if (!finished)
				console.log("Object placement failed.");
		}
		
		/* remove selected hexes from board */
		for (var i = 0; i < num; i++)
		{
			board.splice(board.indexOf(hexes[i]), 1);
		}
			
		return hexes;
	}
	
	function randomHex(board)
	{
		return board[Math.floor(Math.random() * board.length)];
	}
	
	function distance(hex1, hex2)
	{
		return (Math.abs(hex1.x - hex2.x) + 
		       Math.abs(hex1.y - hex2.y) + 
			   Math.abs(hex1.z - hex2.z)) / 2;
	}

	function drawVolcano(hex)
	{
		var x = hex.xLoc;
		var y = hex.yLoc;

		var mtn = [0, 2, 6, 10];
		for (var i = 0; i < mtn.length; i++)
		{
			var vol = myPanel.createPolygon();

			vol.addPointXY(x - 40 + mtn[i], y);
			vol.addPointXY(x - 50 + mtn[i], y + 17 - mtn[i]);
			vol.addPointXY(x - 40 + mtn[i], y + 34 - mtn[i]);
			vol.addPointXY(x - 20 + mtn[i], y + 34 - mtn[i]);
			vol.addPointXY(x - 10 + mtn[i], y + 51 - mtn[i]);
			vol.addPointXY(x + 10 - mtn[i], y + 51 - mtn[i]);
			vol.addPointXY(x + 20 - mtn[i], y + 34 - mtn[i]);
			vol.addPointXY(x + 40 - mtn[i], y + 34 - mtn[i]);
			vol.addPointXY(x + 50 - mtn[i], y + 17 - mtn[i]);
			vol.addPointXY(x + 40 - mtn[i], y);
			vol.addPointXY(x + 50 - mtn[i], y - 17 + mtn[i]);
			vol.addPointXY(x + 40 - mtn[i], y - 34 + mtn[i]);
			vol.addPointXY(x + 20 - mtn[i], y - 34 + mtn[i]);
			vol.addPointXY(x + 10 - mtn[i], y - 51 + mtn[i]);
			vol.addPointXY(x - 10 + mtn[i], y - 51 + mtn[i]);
			vol.addPointXY(x - 20 + mtn[i], y - 34 + mtn[i]);
			vol.addPointXY(x - 40 + mtn[i], y - 34 + mtn[i]);
			vol.addPointXY(x - 50 + mtn[i], y - 17 + mtn[i]);

			vol.getFill().setColor("rgb(46,41,58)");
			myPanel.addElement(vol);
		}

		/* This creates a solid fill in the center

		var mid = myPanel.createPolygon();

		mid.addPointXY(x - 10, y - 17);
		mid.addPointXY(x + 10, y - 17);
		mid.addPointXY(x + 20, y);
		mid.addPointXY(x + 10, y + 17);
		mid.addPointXY(x - 10, y + 17);
		mid.addPointXY(x - 20, y);

		mid.getFill().setColor("rgb(207,18,31)");
		myPanel.addElement(mid);
		*/

		// This creates a center containing a lava image
		var mid = myPanel.createImage();

		mid.setUrl("/public/images/lava-small.png");
		mid.setHorizontalAnchor(jsgl.HorizontalAnchor.CENTER);
		mid.setVerticalAnchor(jsgl.VerticalAnchor.MIDDLE);
		mid.setLocationXY(x, y);

		myPanel.addElement(mid);
	}
	
	function drawHuts(hexes)
	{
		for (var i = 0; i < hexes.length; i++)
		{
			drawObject(hexes[i], "rgb(188,90,82)");
		}
	}
	
	function drawTrees(hexes)
	{
		for (var i = 0; i < hexes.length; i++)
		{
			drawObject(hexes[i], "rgb(86,226,102)");
		}
	}
	
	function drawStatues(hexes)
	{
		for (var i = 0; i < hexes.length; i++)
		{
			var hex = hexes[i];
		
			drawObject(hex, "rgb(180,200,224)");
			
			var marker = myPanel.createCircle();
			
			var heading = Math.floor(Math.random() * 6);
			switch (heading)
			{
			case 0:
				marker.setCenterLocationXY(hex.xLoc, hex.yLoc + 10);
				break;
			case 1:
				marker.setCenterLocationXY(hex.xLoc + 8.66, hex.yLoc + 5);
				break;
			case 2:
				marker.setCenterLocationXY(hex.xLoc + 8.66, hex.yLoc - 5);
				break;
			case 3:
				marker.setCenterLocationXY(hex.xLoc, hex.yLoc - 10);
				break;
			case 4:
				marker.setCenterLocationXY(hex.xLoc - 8.66, hex.yLoc - 5);
				break;
			case 5:
				marker.setCenterLocationXY(hex.xLoc - 8.66, hex.yLoc + 5);
				break;
			}
			
			marker.setRadius(3);
			marker.getFill().setColor("rgb(0,0,0)");
			
			myPanel.addElement(marker);
		}
	}
	
	function drawObject(hex, color)
	{
		var obj = myPanel.createCircle();
		
		obj.setCenterLocationXY(hex.xLoc, hex.yLoc);
		obj.setRadius(10);
		
		obj.getFill().setColor(color);
		
		myPanel.addElement(obj);
	}
	
	function pickBetween(obj1, obj2)
	{
		if (Math.random() > 0.5)
			return obj2;
		else
			return obj1;
	}
	
	function rotateBoard(boardObj, rotate)
	{
		var board = boardObj.board;
	
		for (i = 0; i < board.length; i++)
		{
			switch (rotate)
			{
			case 1:
				var temp = board[i].x;
				board[i].x = board[i].y;
				board[i].y = board[i].z;
				board[i].z = temp;
				break;
			case 2:
				var temp = board[i].x;
				board[i].x = board[i].z;
				board[i].z = board[i].y;
				board[i].y = temp;
				break;
			default:
				break;
			}
		}
	}
	
	function shiftBoard(boardObj, x, y, z)
	{
		var board = boardObj.board;
	
		for (i = 0; i < board.length; i++)
		{
			board[i].x = board[i].x + x;
			board[i].y = board[i].y + y;
			board[i].z = board[i].z + z;
		}
	}
	
	function placeBoard(boardObj, xOffset, yOffset)
	{
		var board = boardObj.board;
	
		for (var i = 0; i < board.length; i++)
		{					
			board[i].xLoc = -board[i].y * 30 + xOffset;
			board[i].yLoc = (board[i].z - board[i].x) * 17 + yOffset;
		}
	}
	
	function drawBoard(boardObj)
	{
		var board = boardObj.board;
	
		for (var i = 0; i < board.length; i++)
		{					
			drawHex(board[i].xLoc, board[i].yLoc, board[i].type);
		}
	}
	
	function drawHex(x, y, type)
	{
		var hex = myPanel.createPolygon();
						
		hex.addPointXY(x - 10, y - 17);
		hex.addPointXY(x + 10, y - 17);
		hex.addPointXY(x + 20, y);
		hex.addPointXY(x + 10, y + 17);
		hex.addPointXY(x - 10, y + 17);
		hex.addPointXY(x - 20, y);
		
		switch (type)
		{
		case 'mountains':
			hex.getFill().setColor("rgb(97,96,102)");
			break;
		case 'beach':
			hex.getFill().setColor("rgb(207,167,116)");
			break;
		case 'jungle':
			hex.getFill().setColor("rgb(58,113,71)");
			break;
		case 'scrubland':
			hex.getFill().setColor("rgb(175,172,75)");
			break;
		case 'river':
			hex.getFill().setColor("rgb(227,200,205)");
			break;
		case 'lake':
			hex.getFill().setColor("rgb(38,122,168)");
			break;
		case 'volcano':
			hex.getFill().setColor("rgb(46,41,58)");
			break;
		default:
			hex.getFill().setColor("rgb(255,255,255)");
			break;
		}
		
		myPanel.addElement(hex);
	}
	
	function determineShoreline(main, left, right, shift)
	{
		var mainBoard = main.board;
	
		for (var i = 0; i < mainBoard.length; i++)
		{
			if (mainBoard[i].isShore == 2)
			{
				if ((shift == 1) &&
						(left.name == "a" || left.name == "B" || 
						 left.name == "c" || left.name == "C"))
				{
					mainBoard[i].isShore = 1;
				}
				else
				{
					mainBoard[i].isShore = 0;
				}
			}
			if (mainBoard[i].isShore == 3)
			{
				if ((shift == 0) &&
						(right.name == "a" || right.name == "A" || 
						 right.name == "b"))
				{
					mainBoard[i].isShore = 1;
				}
				else
				{
					mainBoard[i].isShore = 0;
				}
			}
		}
	}

	function makeVolcano()
	{
		Volcano = true;
		generateBoard();
		Volcano = false;
	}

	generateBoard()
</script>
<button type="button" onclick="generateBoard()">New Board</button>
<button type="button" onclick="makeVolcano()">New Board With Volcano</button>
<label for="centered" id="buttonlabel"><input type="checkbox" id="centered">Volcano Centered</label>

[Tobago](https://boardgamegeek.com/boardgame/42215/tobago) is one of my all-time favorite board games. It has 3 board sections which fit together in multiple ways as well as palm trees and huts placed randomly to make a different island map every time.

It's the "placed randomly" where things get interesting. As humans, it's hard to do things truly at random - our brains aren't built for it. I was delighted, therefore, when I came across [this little doodad](http://tobagogenerator.biz.ly) by [@TodayIsOkay](https://boardgamegeek.com/thread/1010166/random-board-generator) on BGG. It's a quite clever JavaScript applet that generates and displays a random Tobago setup. I've used it for every game since.

Well, this year the first expansion for Tobago came out, 9 years after the game's debut. The expansion adds a volcano(!) and lava that covers up parts of the island. The volcano is also placed on the board randomly. So as a challenge to myself, I copied the original code, then modified it to optionally place a volcano. The volcano can either be placed anywhere on the board or in the middle. The keyword here is "copied": I did not come up with this idea and I would not have known how to implement it. I do, however, know enough to understand it (more or less), and even to expand on it slightly with a little help from a search engine.