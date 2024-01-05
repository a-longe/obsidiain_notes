Write out how each component is supposed to work

## [[Board]]

- The Board's main job is to handle where all the pieces are and to keep track of some extra pieces of data like remaining time, en passant target, active colour and castling rights
- Board should be the one handling anything to do with all the pieces
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
			  ```python
			 dict = {
				 'a1': Piece('w', 'P', 'a1')
				 'b3': Piece('b', 'N', 'b3')}
			 ```
			 
		 - List of piece objects
			 - simple for keeping track of each piece but makes it harder to deal with checking if a piece is at a given location
			  ```python
			  lo_pieces = [Piece('w', 'P', 'a1'), Piece('b', 'N', 'b3')] 
			```
			
		 - Piece Matrices
			 - have a list of lists that represent the rows in a chess board with any pieces in that row being in the list, no representation of an empty square
			  ```python 
			  piece_matrix_wo_empties = [
				  [Piece('w', 'P', 'a1')],
				  [],
				  [Piece('b', 'n', 'b3')],
				  [Piece('b', 'q', 'f4')],
				  [],
				  [],
				  [],
				  [],
			  ]
			```
			
			 - While this representation does abstract the rows of a chess board, we run into problems with move validation and figuring out where the pieces are in the row. To fix this we could have a piece type that acts as an empty object to "fill space" in each list. This would mean having a constant list size but feels like a bit of a hacky way of solving this issue, not necessarily meaning its the worst option.
			 ```python
			 piece_matrix_with_empties = [
				  [Piece('w', 'P', 'a1'), empty, empty, empty, empty, 
				  empty, empty, empty],
				  [empty, empty, empty, empty, empty, empty, empty],
				  [Piece('b', 'n', 'b3'), empty, empty, empty, empty,
				   empty, empty, empty],
				  [Piece('b', 'q', 'b4'), empty, empty, empty, empty,
				   empty, empty, empty],
				  [empty, empty, empty, empty, empty, empty, empty],
				  [empty, empty, empty, empty, empty, empty, empty],
				  [empty, empty, empty, empty, empty, empty, empty],
				  [empty, empty, empty, empty, empty, empty, empty]
			  ]
			```
			
			 - Have two matrices one that has a list of pieces for each row and one for each column, while abstracting more, it runs into issues with updating both lists at the same time accurately and with no real benefit above having one piece matrix I think this idea would not be the best option.
			  ```python
			rows = [
				[Piece('w', 'P', 'a1'), empty, empty, empty, empty, 
				empty, empty, empty],
				[empty, empty, empty, empty, empty, empty, empty],
				[Piece('b', 'n', 'b3'), empty, empty, empty, empty,
				 empty, empty, empty],
				[Piece('b', 'q', 'b4'), empty, empty, empty, empty,
				 empty, empty, empty],
				[empty, empty, empty, empty, empty, empty, empty],
				[empty, empty, empty, empty, empty, empty, empty],
				[empty, empty, empty, empty, empty, empty, empty],
				[empty, empty, empty, empty, empty, empty, empty]]

			columns = [
				[Piece('w', 'P', 'a1'), empty, empty, empty, empty, 
				empty, empty, empty],
				[empty, empty, Piece('b', 'n', 'b3'), 
				Piece('b', 'q', 'b4'), empty, empty, empty],
				[empty, empty, empty, empty, empty, empty, empty],
				[empty, empty, empty, empty, empty, empty, empty],
				[empty, empty, empty, empty, empty, empty, empty],
				[empty, empty, empty, empty, empty, empty, empty],
				[empty, empty, empty, empty, empty, empty, empty]
			]
			```

 - Concepts:
	 - Row
	 - Column
	 - Sliding Piece
	 - Jumping Piece
	 - Board Edges

## [[Move Representation]]
 - tuple representing the square a piece starts (0-63) and and end square (0-63)
	 - eg. (8, 16) which is the equivalent to 'a2a3'

- Lets talk through how were going to deal with moves and getting all possible moves

- move method in piece class that given a square index to move to will set itself to that key's value and then delete the instance in the old key

- get_legal_moves from piece class
-  have a list of all possible squares the piece can move to on an empty board then check if each move is valid using a secondary method, is_legal(move)
	- I've just thought of a glaring problem with this method and that is that 

- How we're generating moves:
	- have two functions:
		- Jumping_move
			- given an offset and makes the necessary checks to check if it is a *valid* move (not teleporting from one side of the board to another)
			- we are given a list of offset for each piece so in each pieces get moves method, loop over each offset and call the jumping move function and add the returned list of moves to a general list of moves
		- Sliding_move
			- is a bit more complicated than jumping piece but should be similar
			- given a list of offsets
			- loop over them, check the square
			- if that square is occupied by a piece of same colour, break out of loop
			- if its occupied by piece of opposite colour, add that square to list, then break
			- if it's empty, add to list, then apply offset again and repeat
			```python
			def get_sliding_moves(board, start:int, offset:int) -> 
													list[tuple[int, int]]
				# new_square:int = 
			```
			
	- either way, to check if a move will leave the board, convert the integer with divmod and apply offset, then check if either number is > 8 or < 0


# _Questions and To Do List_

- where does make_move method go, board or game class?
	- I think because making moves have nothing to do with how much time is left for each colour or have anything to do with multiple chess boards, the make_move method, which should return a new board to go in the list of boards contained in the game class, should be in the board class
	- when make move method is called from the game loop, by default if will to called on the object in the last index `[-1]` and will return the next board which will be appended to the list of boards
	- in the game loop the board being displayed will always be the board at index `[-1]` in the game objects list of boards
- make game class?
	- Game class needs to contain 3 variables, how much time white has left, how much time black has left and a list of boards that describe how a game progresses
- test one edge case?
	- dealing with board metadata like castling rights, move clocks or en passant targets
	- Add one of these to board class and add checks to insure legality of these are followed
	- Suggestion: Start with something super simple like move clocks to get an idea on how to build up the frame for other edge cases to 
 - need to add win/loss screen
	 - maybe to start and keep it simple we simply print what colour won and then break out of loop?

where to put blue square code?
what fow the game class need to be?
move piece update to apply to board, then seperatly apply board surface tpo display surface. (piece update to specified surface?)
use racket to make a super simple game and think about the differences between the chosen racket package vs pygame

