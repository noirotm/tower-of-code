# Tower Of Code

This is a hybrid king-of-the-hill and code-golf challenge.

Your goal is to write a program that will interact with a Javascript engine
in order to pass as many tests as possible in order to reach the top level of
the Tower Of Code.

This is a turn-based challenge where all entries are running together, and
must output Javascript code to the standard output once per turn, to be executed
by the controller.  
The golfing element is due to the fact that the allowed amount of code to output
is limited by available energy, so shorter code is obviously at an advantage.

## Rules

### Victory

A program is victorious when it has reached the highest level in the Tower,
or when all other contestants have been eliminated.

### Tests

There are ten levels in the tower, each one being unlocked by calling a function
in the runtime.  
Each function's name is secret, and can only be obtained by calling the previous
function, which takes no argument and returns a string.  
You start at level 0, and win once you have called the function for the 10th level.

Every function name is the concatenation of the level to reach (eg. `level5`
to reach the 5th level) and the string returned by the previous function, to
which a specific operation must be applied.

Each level has a specific operation:

- level 1: an empty string (so the function is always named `level1`)
- level 2: the previous string reversed
- level 3: the longest word in the previous string
- level 4: a *snake_case* conversion into *CamelCase* of the previous string
- level 5: a ROT13 of the previous string
- level 6: an upper-case roman number from the integer representation of the previous string
- level 7: the Nth fibonacci number where N is the integer representation of the previous string
- level 8: a base64 decoding of the previous string
- level 9: the Nth pi digit where N is the integer representation of the previous string
- level 10: the closest prime number less than the integer representation of the previous string

For example, if `level1()` returns `"echo"`, the function to call next will be
named `level2ohce`.

Passing a test consists in finding the correct name for the next function and
calling it.

### Turns

Each turn, your program receives two numbers on its *STDIN*, separated by a space,
and followed by a single `"\n"` character.
The first number represents your available energy, that is, the number of bytes
that you are allowed to output.  
The second number is the current level your program is at.

Sending the `"\n\n"` string (two consecutive line-feed characters)
marks the end of turn.

If the program chooses to execute code, the number of bytes sent to STDOUT minus
the final two `"\n"` characters are deduced from the total energy pool.

Programs gain **1** energy point at the end of their turn.

If the program chooses not to execute code, by sending only the `"\n\n"` string,
energy is accumulated for later turns.

If a program attempts to execute more code than its current energy, code will
be truncated to the length of available energy and executed as-is, even if
invalid.

### Elimination

Executing invalid code or causing the engine to encounter an exception causes
the player at the current turn to be eliminated.

Each turn can only take a maximum duration of **5** seconds. Any program taking
longer is eliminated.

Only one test can be taken each turn. Any attempt to pass more than one test in
a given turn causes the player at the current turn to be eliminated.  
Similarly, attempting to pass a test that does not correspond to your level
results in elimination.

### Javascript API

The Javascript runtime used in this challenge is otto (https://github.com/robertkrimen/otto)
which complies to ECMAScript 5.1.

Some functions and variables are made available for players and enforced at each
turn:

- `player.energy`: the current player's energy
- `player.level`: the current player's level
- `feedback(str)`: echo a string to be received on standard input by your program

The following callback can be set by the player and will not be overwritten
by the engine:

- `player.onBeginTurn`: by default an empty function, will be called when the
   player's turn begins, before evaluating the code sent on that turn.

In addition, the following array:

    players

contains all players, without specific order.

## Tournament

To make the challenge fair for all entrants, there will be as many matches as
contestants, where the order of players is rotated by one so each program
has a chance to run first.

At the end of a match, each player scores as many points as their current level
even if they have been eliminated.  
The winner also earns a bonus of 10 points.

The overall winner will be the program with most points after all matches.

## Submissions

Programs will be run in an Ubuntu virtual machine, using the code at
https://github.com/noirotm/tower-of-code. Implementations in any language are
accepted as long as reasonable instructions to compile and/or run them are provided.

I'm providing a few test implementations in ruby in the git repository, feel
free to take inspiration from them.

## Tips

Since all code runs in a single Javascript environment, there is no resctriction
of access for variables and functions created by the contestants.
Interacting with other players is therefore encouraged.
Be creative, there is more than one way to be the winner !
