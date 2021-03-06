## Leela Analysis Script

This script is a modified version of scripts originally from:
https://github.com/lightvector/leela-analysis

Currently, it's designed to work with **Leela** and **Leela Zero**, no guarantees about compatibility with any past or future versions. 
It runs on Python 3.

**WARNING:** It is not uncommon for Leela to mess up on tactical situations and give poor suggestions, particularly when it hasn't
realized the life or death of a group yet that is actually already alive or dead. Like all MC bots, it also has a somewhat different
notion than humans of how to wrap up a won game and what moves (despite still being winning) are "mistakes". Take the analysis with
many grains of salt.

### How to Use
First, download and install the commandline/GTP engine version of Leela or Leela Zero from:

    https://sjeng.org/leela.html
    http://zero.sjeng.org/

Download or Clone this repository to a local directory:

    git clone https://github.com/jumpman24/sgf-analyzer
    cd sgf-analyzer

Then run the script to analyze a game, providing the command with arguments:
* file name of game to analyze 
* used configuration (default is stored in config.yaml:bots:default) 
   
      sgfanalyze.py my_game.sgf --bot leela-zero

Some of available options in config:

    stop_on_winrate: 0.80       # Stops analysis on this winrate drop(default=0.80)
    analyze_time: 60            # How many seconds to use per game moves analysis (default=60)
    analyze_threshold: 0.05     # Display analysis on moves losing at least this much of win rate (default=0.05)
    variations_threshold: 0.10  # Explore variations on moves losing at least this much of win rate (default=0.05)
    variations_time: 30         # How many seconds to use per variations analysis (default=30)
    variations_depth: 5         # Number of nodes to explore (depth) in each variation tree (default=5)
    num_to_show: 10             # Number of suggested perfect moves to show(default=10)

By default, Leela will go through every position in the provided game and find what it considers to be all the mistakes by both players,
producing an SGF file where it highlights those mistakes and provides alternative variations it would have expected. It will probably take
an hour or two to run.

### TODO list:

   - [x] clean-up suggested variations with low visits rate
   - [x] mark by A-B alternatives which has low difference
   - [ ] support Ray bot (in progress) 
   - [ ] code refactoring (in progress) 
   - [ ] add documentation (in progress) 
   - [x] show even branches
   - [x] add params to stop analysis if win rate drops > ~80%
   - [x] add logger
   - [x] tune performance between leela calls
   - [x] support/clean-up non english characters (bug)
   - [x] update pdf graph output to have better look
   - [x] write to file during analysis
   - [x] write to file with Python 3 instead of console
   - [x] add limitation to show suggested moves
   - [x] add config file
   - [x] divided time for move and variations analysis

___

### Troubleshooting

If you get an "OSError: [Errno 2] No such file or directory" error or you get an "OSError: [Errno 8] Exec format error" originating from "subprocess.py",
check to make sure the command you provided for running Leela is correct. The former usually happens if you provided the wrong path, the latter if
you provided the wrong Leela executable for your OS.

If get an error like "WARNING: analysis stats missing data" that causes the analysis to consistently fail at a particular spot in a given sgf file and only
output partial results, there is probably a bug in the script that causes it not to be able to parse a particular output by Leela in that position. Feel
free to open an issue and provide the SGF file that causes the failure. You can also run with "-v 3" to enable super-verbose output and see exactly what
Leela is outputting on that position.
