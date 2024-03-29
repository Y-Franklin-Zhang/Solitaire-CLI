# Solitaire-CLI 

This was created as part of the coding challenge for the KP Engineering 
Fellows Program (Summer 2020). 

If you're not familiar with the rules for Solitaire, you can read up on 
them [here.](https://bicyclecards.com/how-to-play/solitaire/)

## Setup 
First, make sure that you have Python 3 installed. Now, to start the game, run the 
following in your local terminal:
```bash
cd <this-directory>
python solitaire.py
```

## How to Play
Upon startup, you will see a welcome screen followed by a prompt to enter a move command. For the best experience, make sure your terminal window size is wide enough to accomodate the entire game board. The move commands
are as follows: 
```bash
\sw - Moves a card from Stock to Waste.
\wf <suit> - Moves a card from Waste to the <suit> Foundation. Suit must be one of: clubs/diamonds/hearts/spades.
\wt <tableau_num> - Moves a card from Waste to the <tableau_num> Tableau. <tableau_num> must be between 1 and 7, inclusive.
\tf <tableau_num> <suit> - Moves a card from the <tableau_num> Tableau to the <suit> foundation. Same input rules as above.
\tt <num_1> <num_2> - Moves all face-up cards from <num_1> Tableau to <num_2> Tableau. Same input rules as above.
\help - Displays all possible moves.
\quit - Quit the game.
```
In general, the commands follow the structure of (source) -> (desination). So, e.g. `\sw` moves from a card from the (s)tock to (w)aste, and `\tf 2 hearts` moves a card from (t)ableau (2) to (f)oundation (hearts). 

At any point in the game, you can enter the command `\help` to re-display the list of all valid commands. You will only 
be allowed to execute commands that are legal moves under the Solitaire rules linked above. If you attempt to execute a
move that is not legal, you'll see an error message and you'll be prompted to re-enter a valid move. 

Note that because I couldn't find a universal way to display the color of cards across all terminals (I had originally opted
for UTF-8 symbols, but had trouble getting them to display properly across all possible consoles), each card's string
representation is only going to tell you its rank and suit. But, in a typical playing card deck, it's always the case that hearts and diamonds are one color, and clubs and spades are another color, so the task of matching opposite colors is the same as matching a heart/diamond with a club/spade. 

## Design Choices
I chose to go with a fairly OOP-heavy approach, as I feel that the design of Solitaire lends itself really nicely to 
OOP. At the core of my implementation lies the `CardStack` abstract base class, which is inherited by 
the `CardSequence`, `Stock`, `Waste`, `Foundation`, and `Tableau` classes. The `CardStack` abstract class is basically just a normal LIFO stack, but with a little extra details for handling flipping cards up and down. Because Solitaire is really just a game
of managing different stacks, it made sense to me to have everything inherit from `CardStack`.

I also have the `Card` class represent a typical playing card. It has all the standard fields you'd expect: rank, suit,
color, value (the numerical version of its rank), and whether it's flipped up or down. Cards that are flipped up will 
display their rank and suit, but cards that are flipped down will have their rank and suit hidden. 

To handle moving cards around the board, I chose to implement the `CardSequence` class, which I used to determine 
whether the sequence of cards was valid to be moved in the first place. The inspiration for this class came from the 
observation that the only valid sequences that could be moved were those that were "oscillating" in color and 
"descending" in value.

Because of time constraints, I was a little loose and fast with designing my APIs for my different classes, and don't 
always have explicit getters/setters. Going forward, I'd really like to clean up my external methods for each class, and
have everything nicely encapsulated and functional. 

Finally, I have all the game logic abstracted away in the `Game` class, which handles executing all the valid moves of 
the game, and I have all the driver code and input handling in the `main()` method.

## Libraries and Tools
I chose to implement this in Python, because it's clean, functional, and simple, and it had everything I needed (inheritance, classes, dynamic lists, etc.) right out of the box, without too much complication. It's also my most comfortable language, so it's usually my go-to. 

As far as Python packages go, I used the `random` module to assist with shuffling the deck, and that's it. Again, 
because of time constraints, I opted not to write a full collection of unit tests, and instead just tested my code 
manually. However, if I were to come back to this, I'd implement the unit tests for each of my Classes via `pytest`. 
