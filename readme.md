# Coding Challenge: Colour Battle!

![](images/banner.png)

The goal is the fill as much as possible of the canvas with your bot's colour. You determine in which direction your bot should move. *Be weary of other bots who might want to paint over your hard work!*

[View the already submitted bots battle online here!](https://nobleans-playground.github.io/coding-challenge-colour-battle/)

You will write your bot's logic in a Python class. In each iteration of the game a specific function in your bot will be called (with relevant information as arguments), and your bot will need to decide in which direction to turn.

Here you can see two example bots. Do you have what it takes to out-paint _Rambo the Rando?!_
- [Rambo the Rando](https://github.com/nobleans-playground/coding-challenge-bot-template/blob/main/rambo_the_rando.py)
- [Short Sighted Steve](https://github.com/nobleans-playground/coding-challenge-colour-battle/blob/master/robots/short_sighted_steve.py)

## Some Concepts

### The Grid

This is the square canvas on which your bot will paint. It's a 2D grid of integers, where the integer contains the `id` of the bot who's colour is painted there, or `0` if it's unpainted. The size of the grid will be scaled with the amount of bots in on the grid, with the following equation:

``` Python
cells_per_bot = 8 * 8    # The amount of space every bot will have on average
grid_side_length = math.ceil(math.sqrt(cells_per_bot * number_of_bots))
grid = np.zeros(grid_side_length, grid_side_length, dtype=np.int16)
```

### Your Bot

Your bot will move around on the grid (or canvas). Each round you can choose if it will go `UP`, `DOWN`, `LEFT`, `RIGHT` or `STAY` where it is currently. After every move it will attempt to paint the tile beneath it, but if it's successfull depends on its randomly assigned `id` (starting at `1`). The following equation is used:

``` Python
def determine_new_tile_colour(self, floor_colour, bot_colour):
        if floor_colour == 0: return bot_colour # Bot always paints a white floor tile
        return [floor_colour, 0, bot_colour][abs(bot_colour - floor_colour) % 3]
```

The goal of this equation is to allow smarter bots to perform better by possibly seeking the tiles that are easier to paint. 

**Note:** If two robots occupy the same tile then the tile will be painted white, regardless of bot `id`.

As mentioned your bot will be a Python class where you will write the logic the bot will use to decide what to do next. How the bot determines what to do next is up to you. You could train an AI, do weighted path-planning, or simply hunt the highest scoring opponent. As long as it won't fry my laptop trying to run it, it's fine.

Each round your bot will execute the following function that will receive these arguments:

``` Python

# This function is called every round. It contains
#   self
#       id                  Your randomly assigned ID
#       position            Your current position on the grid
#   grid                    A np.array((N, N), dtype=np.int16) containing
#                           the current colours of all the grid tiles. As
#                           mentioned the colours are basically the id
#                           of the bot who painted it. 0 means unpainted, white.
#   enemies[]               A list of all the bots on the grid, including you
#       id                  Id of this enemy (or you)
#       position            Position of this enemy (or you) on the grid
#   game_info               General info about this game
#       current_round       The current round starting at 1
#       number_of_rounds    The current round starting at 1
#       grid_size           Width (or height) of the grid.
def determine_next_move(self, grid, enemies, game_info):
    # Your logic here
    return Move.UP  # This bot will always go up. An invalid move will be treated as Move.STAY.
```
**Note:** Any moves to outside the grid limits will simply be ignored.

## Final Scoring
The final standings will be determined in an epic tournament consisting of 1000 games consisting of a 1000 rounds each where all bots will be placed in a single arena. After each game the total the amount of painted tiles for each bot will be counted, and at the end all the tournament all the game's scores will be added together.

**Note:** The amount of games or rounds might be adjusted depending on the amount of bots submitted, or if it's found that 1000 is not long enough for an accurate representation. If the amount of bots is too large then a Round-Robin-ish implementation might be implemented.

## Submitting

Your bot will live as a git submodule inside the main challenge repository. This means you will need to create your own GitHub account and create a new repository based on a template. You can follow the following steps to create your own bot.  

**Note:** You are allowed to submit a maximum of two bots.

**Steps to submit a bot:**
1. Create a GitHub account if you don't have one already. Using a personal account is fine.
2. Ask Tim Clephas to add you to the [Nobleans Playground](https://github.com/nobleans-playground) Organisation. **Note:** This is optional. You will only need this if you want to use a repository template for Step 3.
3. Create a personal repository where your bot will live. It needs to look like this [template](https://github.com/nobleans-playground/coding-challenge-bot-template). If you joined the Playground Organisation you can select the `template-bot` as template for your new repository.
4.  Give your bot a custom name, add your name as contributor, and some custom logic (can be done later as well).
5. Notify a organizer (Hein Wessels) to add your bot to the challenge as a submodule in the main repository. **Note:** Your bot doesn't have to be complete to be added. It simply needs to run and return a valid move. You can update/change/refactor your bot at any point during the challenge.
6. After your bot has been added you observe it fighting for colour squares [online](https://nobleans-playground.github.io/coding-challenge-colour-battle/)!

**Running the challenge on your own machine**
1. Clone _this_ repository using `git clone --recursive [URL]`. The `--recursive` is to pull in all submodule bots.
2. If your bot's submodule has been added as a submodule then you can develop your bot in your folder. Remember to `add/commit/push` your changes once you're done so that the online simulation can be updated with your newest bot. (Will still need a manual refresh by an organiser)
3. **If your bot has not been **added to the main repository you can temporarily add it to `robots/`, similar to `ShortSightedSteve`. Remember to add ot tp `bot_list.py` as well.
4. Run `main.py` to watch the current bots battle for the canvas with a nice GUI (if using VSCode press `F5` to run it in `debug` mode). Or run `tournament.py` to run it without a GUI and simply print out the end rankings.

**Updating your local repository with the newest changes**
Over the course of the challenge your local repository might be out-of-date with all the other bots. You will need to run `git submodule update --remote` to pull in all the newest bot changes. It might also be that some challenge-engine code was altered, which you will also have to `pull`.

## Running the bot on your machine
You can develop and test your bot on your local machine, and should be doable on either Windows or Linux. All you need is the following Python packages on your machine, `pygame` and `numpy`.

You can run the game in two modes by running one of the following two files:
- `main.py`: Regular game with GUI.
- `tournament.py`: Run a test tournament without a GUI. After it's done it will print out the rankings. Use `--games 100` or `--rounds 100` to alter the length of the tournament. Defaults to one game of 1000 rounds.

## Rules
- Targetting a specific other bot is not allowed, although you may target the tactics of a general class of bot. You may target a specific bot ID, beause it's impossible to know which bot it belongs to. This mostly means you may not reverse engineer someone's bot and then create a bot that effectively neutralizes it because you know it's next move. 
- Bots may work together but may not communicate with each other and will not know each others IDs (will be randomized anyway). The wins will be awarded individually rather than as a team.
- You may not attempt to alter other bot's internal state, or any other internal game variables, f.i. `id`, `position`, the `grid`, etc.
- Python libraries that will simplify this challenge may **not**. An exception can be made for lightweight-ish neural networks if you trained the bot yourself.
- Your bot may not intentionally crash. During the tournament any crashes of your bot will be treated as `Move.STAY` command.