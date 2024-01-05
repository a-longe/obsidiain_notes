
# Game Class Requirements
- __OVERALL JOB__:
	- Keep track of a game of chess
- keep track of 'history' of boards, ie what the current board is, and the past boards.
	- in the future we wight want to add being able to take back moves and revert to a previous board
	
- keep track of any data that pertains between boards or any data that you cant figure out just by looking at a board
	- is the game over?
		- if a player forfeits or runs out of time
		- to control the main game loop
	- black and white remaining time
		- to be displayed
		- affects main game loop
		- to be passed to engine for engine to determine how long to 'think' about a given move for
	- display surface
		- originally was contained in board class but realized that pygame is bad at switching between displaying different surfaces, therefor have one surface that is passed by reference to each board then piece

- One change I'd like to change is that the game class atm is holding whose turn it is and also the move count, however this doesn't really make sense as the board class will already need to keep track of this already

- Some methods to interact with boards
	- get current board
	- add board
	- both of these methods are actually very simple but make the code more readable


# Board Class
- __OVERALL JOB:__
	- Capture a moment in a chess game
	
- To do this, keep track of all the pieces in a dictionary, whose turn it is, move count and more data to be added later.

- Also store the game of which this board is a part of
	- This is because parts of the board my need to ask the game object to change certain variables attached to it

- Keeps track of some data nessisary other than the pieces, atm it only keeps track of whose turn it is and the number of moves since the start of the game. 
- Eventually we will need to add more pieces like castling rights and en passant.
	- This is the only place where it makes sense to store these, it doesn't really make sense to store it in the game class because the game does not affect the variables, the movement on the board do
	- And it doesn't make sense to put them in the piece class because it does not pertain to them

- Some methods to interact with the board
	- atm has the methods dealing with the surface but because we moved the surface to the game class
	- should have methods that deal with getting and setting any of it's variables
	- atm has methods that are called everytime the mouse it clicked up or down, but does this make any sense?
		- I don't think so, to me it doesn't make sense for the board to carry the mouse events, it makes more sense for the game to carry that responsibility. It makes more sense when you think about the fact that the only board we would want to affect would be the current board, which is only determinable in the game class.

# __Piece Class__
- __OVERALL JOB:__
	- 

# __Mouse Up and Mouse Down__
