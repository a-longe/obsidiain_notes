Write out how each component is supposed to work

## [[Board]]

- The Board's main job is to handle where all the pieces are and to keep track of some extra pieces of data like remaining time, en passant target, active colour and castling rights
- When creating a board there is an intermediate type of data to represent the pieces
	- list of tuples, tuples contain what colour pieces is, coordinate or square and piece type?
	- en passant target 
	- castling rights
	- who's turn is it?
- The pieces will be stored in a piece matrix with a list for each row on the board
- Methods:
	- clear_board()
		- displays an empty chess board for the chess pieces to be displayed on
	- update_pieces()
		- loops over each piece in piece matrix and calls their own update method
	- is_inside_board(x, y)
		- is the given coordinate found inside the chess board
		- used to check what square the mouse is on and also for drag and drop (don't want piece being dragged outside the board)


## [[Piece]]

- base class for all chess pieces, will not be found in boards piece matrix, used for inheriting methods that all kinds of chess pieces share
- will be passed the board which it's from, what colour it is (is_white - true/false) and a Rect object used to store where the piece is in the coordinate plane (contains size controls and has useful build in methods)
- Methods:
	- update()
		- if is_dragging is true, set the center of the rectangle to the mouse cords
		- pastes the icon determined my the kind of piece onto the board that was passed in
	- snap_to_square()


## [[Game Loop]]

-  The game loop is a function that is called every frame in Pygame
- in the game loop to take input from the player, we loop over every event and change the state of the game based on those game inputs.
- you get a mouse down input, check if the mouse in on a piece, if it is, set that piece's is dragging variable to true so that  every frame that piece will be 'attached' to the cursor
- on a mouse up event, loop over every piece until you find a piece a with a is_dragging being true, check it's Rect and 'snap' it to the square where it's center is located
- at the end of every game loop call the update method on the current board


## [[Move Validation]]
 - What do I want the board representation to do?
	 - Options (types):
		 - Dictionary with location as key and piece object as value
			 - location could be square notation eg: 'a3' but that gets with dealing with changing from letters to numbers as indices, and with the number going from 8 to 1 and not 1 to 8
			 - could make it better by numbering each square from 0 to 63 and using that instead, makes getting the number easier but does not abstract the board into rows and columns (maybe do not need to do this?)
		 - List of piece objects
			 - simple for keeping track of each piece but makes it harder to deal with checking if a piece is at a given location
		 - Piece Matrices
			 - have a list of lists that represent the rows in a chess board with any pieces in that row being in the list, no representation of an empty square
			 - While this representation does abstract the rows of a chess board, we run into problems with move validation and figuring out where the pieces are in the row. To fix this we could have a piece type that acts as an empty object to "fill space" in each list. This would mean having a constant list size but feels like a bit of a hacky way of solving this issue, not necessarily meaning its the worst option.
			 - Have two matrices one that has a list of pieces for each row and one for each column, while abstracting more, it runs into issues with updating both lists at the same time accurately and with no real benefit above having one piece matrix i think this idea would not be the best option.
 - Concepts:
	 - Row
	 - Column
	 - Sliding Piece
	 - Jumping Piece
	 - Board Edges