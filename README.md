# Lemmings

A console version of the classic Lemmings game, written in Java. Lemmings walk
on their own across a 10x10 board and you keep them alive by giving them roles
until enough of them reach the exit door.

## What it is

Second-year Programming Technology (TP1) assignment at UCM, built in late 2024.
The course hands you the skeleton of the game and asks you to grow it over three
deliveries: the walking simulation, then a command interpreter, then roles,
level files and the exception hierarchy. This repo is the final delivery.

The game runs in a terminal. Every turn you type a command (or nothing) and the
board advances one cycle: lemmings step, fall, turn around at walls and die if
they fall too far or off the board.

## How it works

- The board is 10 columns by 10 rows. Rows are letters (A to J) and columns are
  numbers (1 to 10). Cells hold walls, metal walls, the exit door and lemmings.
- A lemming walks in its current direction and turns around when it hits a wall
  or the edge. If there is nothing under it, it falls. Landing after a fall of 3
  or more cells kills it; falling off the board kills it too.
- Roles change what a lemming does: `Walker` is the default, `Parachuter` falls
  without taking damage until it lands and then goes back to walking,
  `DownCaver` digs through the wall directly below it, then falls. Metal walls
  cannot be dug.
- Three built-in levels (0, 1 and 2) are defined in code. A level can also be
  loaded from a text file: the first line is
  `cycle lemmingsInBoard dead exited lemmingsToWin`, and each following line is
  one object, for example `(4,5) ExitDoor`, `(2,4) Wall`, `(3,6) MetalWall` or
  `(2,3) Lemming RIGHT 0 Walker`.
- You win when no lemmings are left on the board and at least `lemmingsToWin`
  made it through the door. Otherwise you lose.

Commands (short form in brackets):

| Command | What it does |
|---|---|
| `[n]one` or an empty line | advance one cycle |
| `[s]et[R]ole ROLE ROW COL` | give the lemming at `ROW COL` (e.g. `sr P C 4`) the role `W`, `P` or `DC` |
| `[r]eset [level]` | restart the current level, or switch to level 0, 1 or 2 |
| `[l]oad <file>` | load a level from a text file |
| `[h]elp` | print the command list |
| `[e]xit` | quit |

## Stack

Java 17 or newer (the code uses `String.formatted`). No build tool, no
dependencies apart from JUnit 5 for the course test harness.

## Run it

From the repo root:

```
mkdir -p out
javac -d out $(find src -name '*.java' ! -name 'Tests_V3.java')
java -cp out tp1.Main 2
```

The optional argument is the starting level (0, 1 or 2; default 2). Passing a
second argument of any value switches to the plain view without ANSI colours.

`src/tp1/Tests_V3.java` is the course's output-comparison harness. It needs
JUnit 5 and a `tests/pr3+reset/` folder with the expected outputs, which is not
part of this repo, so it is excluded from the command above.

## Project layout

```
src/
  tp1/            Main, Controller, game logic, game objects and console views
  commands/       one class per command plus the CommandGenerator parser
  logic/          lemming roles (Walker, Parachuter, DownCaver)
  exceptions/     parse, execute, load and off-board exceptions
```
