
- [ ] Design board class
	- [ ] piece list
		- [ ] Piece class
	- [x] meta data
		- [x] move counters
		- [x] en passant targets
		- [x] castling rights
		- [x] turn

- [ ] Basic REPL integration (UCI standard)
	- [ ] Create new game
	- [ ] get bestmove
	- [ ] set position

- [ ] Make testing framework
	- [ ] Import bit boards from fen (fen to bit board stack, metadata conversion)
	- [ ] set up Pytest tests for test suite
	- [ ] can select sf or my engine to test

- [ ] Get possible moves (psudolegal)
	- [ ] Pawn
		- [ ] White
		- [ ] Black
		- [ ] Double move
	- [ ] Knight
	- [ ] Sliding pieces
		- [ ] Bishop
		- [ ] Rook
		- [ ] Queen
	- [ ] King
	- [ ] Castling
	- [ ] En Passant
		- Check FEN string, add to fen on double move

- [ ] Create function that converts a list of squares into a bit board
- [ ] check opponents moves to check for check

- [ ] Implement Legal moves for all pieces
	- [ ] en passant
	- [ ] special castling rules
		- cannot castle in or out of check

- [ ] Create Evaluation function that will give a board a rating based on how powerful it is
	- Not based on future moves, that is left to Min Max algorithm
	- Start simple, but this eval function can ALWAYS be improved

- [ ] Implement Min Max algorithm (search algorithm)
	- Once we have functions that:
		- Get all LEGAL moves in a position
		- Can get all LEGAL moves in a position and after a move
		- Simple Eval function

- [ ] Create game manager 
	- allowing different versions of engines to play against each other and average the results

- [ ] Add additional functions while testing using testing suite
	- [ ] search: alpha beta pruning, search ordering
	- [ ] eval: piece positioning bias, board control