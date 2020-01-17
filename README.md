# Overview

Denon sells nice hardware, but not so great software... Denon's Engine Prime (EP) still has a lot to improve when it comes to beat detection and beat grid.

Although EP allows you to import from Serator, Rekordbox or Traktor libraries, the beat grids get erased... Pretty annoying.

`PlzFixEP` is an open-source project which goal is to provide utils to export beat grids to Engine Prime.

Supported imports:
- Traktor -> Engine Prime
- Rekordbox -> Engine Prime

# Issues
This is a quick and dirty first draft. There's a lot of issues with this code.

- The current version of the code works on Engine Prime on computers, but fails on the players. (Last tested on `EP 1.3.3.c27c25d`)
- This code has unexpected results on tracks with duplicate filenames

Feel free to PR to fix it, I'm not actively coding on this projet at the moment, but I'll definitely review your PRs ;)

# Usage

First of all, BACKUP YOUR Engine Prime files ! (Location: Music/Engine Library)

0. See [installation](#installation)
1. Import your tracks normally in Engine Prime.
2. Analyse them in Engine Prime.
3. Close Engine Prime
4. Run the `main.py` script:
```
# Rekordbox -> Engine Prime
python3 main.py rekordbox
# Traktor -> Engine Prime
python3 main.py traktor

# Advanced: specify the locations.json path:
python3 main.py traktor /home/my_user_name/some_custom_file.json
```

5. Re-open Engine Prime. Should be good to go

# Installation
- install python3
- install the required packages:
```
pip3 install -r requirements.txt
```
- configure the `locations.json` file
Note: you can ignore `traktor` or `rekordbox` if you're not using one of them.

# Notes

## Exporting rekordbox collection to xml

Go to rekordbox, click `file-> Export collection to xml format`.


# Contributing

You're very welcome to contribute if you want to pull request improvements / bugfixes, there's still a lot to do ;)

Here's a few quick tips to get started:
- Engine Prime uses SQLite, and the database files are located in `~/Music/Engine Library/`.
- `m.db` contains the overall information on your library (track lists etc.)
- `p.db` contains an interesting table for this project: `PerformanceData`, which itself has a column `beatData`. That's where the beat information is stored, and that's a binary that required quite a bit of reverse-engineering.
- Engine Prime uses a very weird serialization for `beatData` (see `beat_data.py`); EP stores a bunch of `BeatAnchors`, which are used to define the beat grid & BPM.
- Engine Prime tends to write to its databases upon being closed.

At this point, there's still a few unknowns in `beat_data.py`. Feel free to explore and offer smarter interpretations `beatData`'s binary representation.

# Credits & Licensing

Original author: Quentin PIERRE (student at Mines Paristech)

PlzFixEP is an open-source helper to import data into Engine Prime
Copyright (C) 2020  Quentin PIERRE

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <https://www.gnu.org/licenses/>.

## Thanks

I used `Ghidra` quite intensively to reverse engineer EP.
That's a great piece of software, do check it out ;)

Ghidra homepage: https://ghidra-sre.org/