# chess-vm-files

I'm proud to announce Jack CHESS, written in Jack.  

It's you against the computer.  

Copy the .vm files to a folder, and open the folder in the VMEmulator. Set the VMEmulator speed to "Fast" and set animation to "No animation", then run the app. 

Choose White or Black, then choose the level of play:

Level 1: computer plays random but legal moves.  Plays instantly. 

Level 2: computer has a fixed shallow search depth.  Plays almost instantly.

Level 3: computer will vary its search depth. [on my 2017 Mac, moves vary between 10 and 40 seconds]

Level 4: computer will vary its search depth. [on my 2017 Mac, moves vary between 45 seconds and 3 min]

Level 5: computer will vary its search depth. [on my 2017 Mac, moves vary between 90 seconds and 6 min]

Jack CHESS plays 100% legal chess, including castling, en passant, promotions, under-promotions, draw by stalemate, draw by 50 move rule, and draw by repetition.  

Jack CHESS has a small opening book.  This is so you get varied play - otherwise it would always play the same game and be very boring.  

I couldn't find anyone else that has written a chess app in Jack, so I'm happy to make this contribution to the nand2tetris community. 

Notes:

(1) To enter a move, type 4 characters using square coordinates, e.g. E2E4.  I considered using arrow keys, but arrow keys would be very annoying.  Imagine right-right-right-right-up-up-Enter-right-right-up-Enter just to select a Knight and move it.  Note that algebraic notation, e.g. e4, Nf3, O-O, Nxe5, Rdf8, R1a3, or Qh4xe1 is not supported.

(2) Despite going to great lengths to speed up the engine, Jack CHESS is still very slow, because running VM code in the VM Emulator is not exactly a blazing fast environment.  On my 2017 Mac, Jack CHESS is analyzing approximately 550 positions per second, which sounds fast but it isn't.  Also, real chess engines save time by using of gobs of heap (a hash table) to store evaluations of positions it has already searched, and the 14k heap that I was working with is just too small for that (I am using much of the heap for other stuff).  So the end result is Jack CHESS in the VMEmulator is not going to search very deep, but it still plays strong chess, and beats me regularly!

(3) Jack CHESS adheres to the Hack machine's heap and stack size limits and runs in the VMEmulator, but because Jack CHESS was written with performance (not program size) in mind, the program would not fit within the 32k Hack ROM (and the Hack ROM must also contain the Jack OS).  So I will never have the satisfaction of running Jack CHESS in the CPUEmulator. On the other hand, one could theoretically write a chess app that WOULD fit in the 32k ROM, but it would have to be optimized for program size, not performance. Meaning, the Jack code would need a total re-write, and you would need a VM Translator that outputs highly optimized Hack Assembly code.

(4) During development, sometimes the VMEmulator would give me a Jack-code crashing error such as "Out of segment space in Piece.getValue.3". Any time I received an error like this, it meant I had a null pointer, e.g. I was calling getValue() on a Piece object but the object was null.  The VMEmulator shows the Call Stack, which makes finding and fixing these crashing bugs much easier.  I eventually created my own Debug class so I could sprinkle my code with asserts, which helped a lot.  At present I have played many full games with zero crashes of any kind.

(5) It helped that I had written a chess engine 20 years ago. There are millions of articles on the internet related to chess engine programming, and many were very useful to me.  In particular I want to give credit to Bruce Moreland, whose clear writings on chess programming were very helpful.  For those of you who have written game engines, Jack CHESS is a relatively simple chess engine featuring (a) 0x88 board representation, (b) piece lists, (c) alpha-beta with quiescence search, (d) null move forward pruning, (e) iterative deepening with aspiration windows, (f) piece-square tables, and (g) move order: PV move, captures in MVV-LVA order, non-capture killer moves.  Since my move ordering is not ideal, I didn't implement LMR (late move reduction), which would have allowed for more pruning and deeper searches. Also, I did not implement pondering.  Finally, the draw by repetition detection only catches the shortest case where the intervening moves are the same, for performance reasons.  That covers most draw by repetitions and perpetual checks, so it's ok.  My goal was to create an engine that could beat me (with modest thinking time) when run in the VMEmulator, and I met that goal.

