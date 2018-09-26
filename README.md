# OCTAGON Chess Tournament GUI
*version 0.01 ALPHA*

>## Disclaimer
>This is early alpha software so it is undergoing aggressive feature addition and changes. It needs much refactoring, validation and error handling. Please refer to the license for terms of usage.


## What is OCTAGON

OCTAGON is a GUI for playing UCI chess engine tournaments. 

### OCTAGON philosophy:

- Functionality
    - Innovative specialized features (Lc0)
    - Cross-platform
    - Flexibility
    - Automation
    - Reporting
    - Extensibility

- User Experience
    - Clear data visualization
    - Pleasing aesthetics

## Features

### Tournament Engine
- Runs in a browser (but you have to install Python)
- Runs UCI chess engines with command line arguments and UCI options
- Round-Robin or Gauntlet modes
- Multi-gauntlet Mode: Pit several engine challengers against a field of opponents
- Automatic Tournament resume (will replay from last unfinished game)
- Pause games (currently from console)
- Per engine time controls and node limited play
- Openings: PGN format, repeated, sequential or random
- Adjudication rules for win/resign and draw
- Tournaments defined in JSON files
- Engines defined in JSON files
- Tournament and Engine files can be modified mid-tournament
- Tournament time data: Start time, elapsed time, estimated time left per game and per tournament
- Outputs verbose PGN with engine data, and compact PGN only with moves

### User Interface
- Chess piece animation and movement indicators for easy game following
- Collapsible panes
- Single screen design for video streaming
- Tournament data, configuration and rules
- Chess board
- Material difference
- Engine data: Name, UCI name, UCI authors, time controls
- Engine logos
- Clocks and move time(with 0.1s resolution)
- Engine info: Eval, depth, seldepth, nodes, nps
- Adjudication and 50-move counters
- Charts for live engine info
- Lc0 data: live NPS ratio, live A0 ratio, Lc0 raw info (more coming...)
- Last move
- Position FEN
- Move list PGN
- Elapsed time and time left data
- Current round, game, and total games to be played
- Pairings and results  
- Game termination reason: Checkmate, stalemate, 3-fold repetition, 50-move rule, or adjudication  
- Standings: Rating, live Elo performance and error, points/games, efficiency, draw rate, score, and record bar graphs

## Basic Concepts

Octagon is divided into two separate parts, the GUI which runs on your browser, and the tournament engine which runs on a console window.

Once you have python and the necessary python libraries correctly installed in your system, to run an Octagon tournament you need to follow these steps:

---
1. Create your engine JSON configuration files
2. Create a tournament JSON configuration file
3. Run the **octagon.py** engine
4. Open the **index.html** GUI in your browser
---

In the future these steps will be streamlined. Ideally you will just have to click on an icon and you will be able to configure everything from the GUI.

## Important behaviors

- If you close the browser GUI, the console will be paused and resume as soon as you open the index.html file again. 

- If you break the execution on the console ( ctrl-Break in Windows ), the current game will be halted and discarded, but the tournament will resume if you run octagon.py again, and refresh the browser GUI. Tournaments can be suspended this way and resumed at a later point. The tournament will replay the last incomplete game.

- You can also pause a game by pausing execution on the console ( Pause in windows ). Un-pausing the execution (ctrl-z in Windows ), will resume the game if the GUI is still running on the browser.

- If you make changes to the engine configuration files or the tournament file, the changes will be reflected after the next game starts. (Some changes that do not involve engine parameters or regeneration of the tournament pairings will be updated if you refresh the GUI)

- OCTAGON is **not yet capable of running multiple instances**. Make sure you do not have more than a **single Octagon GUI open** on your browser. 

## Getting Started

Here are the steps to get Octagon running:

## 1. Install Python 3

Download and install Python 3: [https://www.python.org/downloads/](https://www.python.org/downloads/)

## 2. Install Python-Chess

```
pip install python-chess
```
    
## 3. Install Tornado

```
pip install tornado==3.2.1
```

## 4. Create engine configuration files

Engine configuration files are JSON files that contain all the information you need to send Octagon to run your engine. 

The engine configuration files preferred location is inside the `tournament/engines/` folder. In the `tournament/engines/` folder you will find 3 sample configuration files. You must change the paths and other fields to match your engine software and preferences.

Here are the example templates:

---
SF8.json: 
```
{
    "name":"Stockfish 8",
    "rating":3423,
    "logo path":"logos/SF.png",
    "path":"C:/Chess Engines/Stockfish/stockfish_8_x64.exe",
    "args":"",
    "options":{
        "Threads":4
    }
}
```
---
```
{
    "name":"Fire7.1",
    "rating":3339,
    "logo path":"logos/Fire.png",
    "path":"C:/Chess Engines/Fire/Fire_7.1_x64.exe",
    "args":"",
    "options":{
        "Threads":4
    }
}
```
---
lc0-11250.json:
```
{
    "name":"Lc0 0.18 11250",
    "rating":3400,
    "logo path":"logos/lc0.png",
    "path":"C:/Chess Engines/LeelaChessZero/lc0 v0.18.0/lc0.exe",
    "args":"-w \"C:/Chess Engines/LeelaChessZero/Networks/11250.gz\"",
    "options":{
        "Threads":2,
        "Scale thinking time":3
    }
}
```
Notice how the quotes in the path and argument fields need to be escaped characters by the use of the backslash.

If the `logo path` is empty, no engine image will be loaded. In the examples, the `logo path` is relative to the folder where octagon.py resides. The `logo/` folder is the preferred location for the engine logos.

## 5. Create tournament configuration file

Tournament configuration files are also JSON files. The tournament files preferred location is in the `tournament/` folder. In this folder you will find a `tournament.json` file. This file is used as default when no arguments are entered when running `octagon.py`. The easiest way to get started is to edit this `tournaments/tournament.json` file to suit your needs. 

If you want to create your own tournament configuration files, just make sure you enter their path as an argument in the next step.


Here is a template:

```
{
    "name": "lc0 11250 vs SF8 vs Fire7.1 Gauntlet",
    "description": "GPU vs 1 CPU thread",
    "os": "Win10 x64",
    "cpu": "i7-6700HQ @2.6Ghz",
    "ram": "16Gb",
    "gpu": "GTX960M",
    "PGN path": "",
    "gauntlet": true,
    "challengers": 1,
    "rounds": 4,

    "engines": [

        { 
            "file": "tournaments/engines/lc0-11250.json",
            "nodes control": false,
            "moves": 0,
            "time": 1,
            "increment": 1,
            "nodes": 0 
        },

        { 
            "file": "tournaments/engines/SF8.json",
            "nodes control": false,
            "moves": 0,
            "time": 1,
            "increment": 0.5,
            "nodes": 0 
        },
        
        { 
            "file": "tournaments/engines/Fire7.1.json",
            "nodes control": false,
            "moves": 0,
            "time": 1,
            "increment": 0.5,
            "nodes": 0 
        }

    ],

    "openings path": "silversuite.pgn",
    "openings twice": true,
    "openings random": true,
    "openings ply": 2,

    "min win move": 0,
    "min win score": 5.5,
    "win move length": 5,

    "min draw move": 30,
    "max draw score": 0.1,
    "draw move length": 4,

    "delay between games": 5
}
```

If the `PGN path` is empty, the tournament PGN will be saved in the local PGN folder using the tournament name as filename. 

If the `openings path` is empty, no openings will be used.

The `challengers` parameter is the number of engines that will challenge the Gauntlet field. The engines are picked in the order they appear in the tournament file.

The `rounds` parameter assumes each engine will play each other both as white and as black. Octagon will generate pairings using the closest highest even number.

To disable **adjudication** set the `"win move length": 0` or `"draw move length": 0`.

## 6. Run octagon.py with path to tournament filename as parameter

If you just edited the included `tournament.json` file, just open a console window and type:

```
python octagon.py 
```
---

If you have created your own tournament configuration file then type:

```
python octagon.py <myPath/myTournament.json>
```

Ommiting the path argument, will run the `tournament/tournament.json` file by default. 

## 7. Open GUI

Wait until the console has tested the tournament file and all the UCI engines. When ready, it will display:

    All engines Tested!

    Tournament ready, waiting for GUI...

Once ready, open **index.html** in your browser, double-clicking should suffice. Remember to open only one GUI window or tab.
