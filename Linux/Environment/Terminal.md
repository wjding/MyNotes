# Terminal Related 

## Terminfo
Terminfo is a data base describing terminals, used by screen-oriented programs
such as nvi(1), rogue(1) and libraries such as curses(3X). Terminfo describes
terminals by giving a set of capabilities which they have, by specifying how to
perform screen operations, and by specifying padding requirements and
initialization sequences. 

## Terminfo compiler - tic
Terminfo files are usually binary format, which is compiled by tic (terminfo
compiler). 

* [Terminfo Source Format](https://pubs.opengroup.org/onlinepubs/7908799/xcurses/terminfo.html)
* [Ncurses and terminfo sources](https://invisible-island.net/ncurses/)

## Ncurses library
ncurses (new curses) is a programming library providing an application
programming interface (API) that allows the programmer to write text-based user
interfaces in a terminal-independent manner. It is a toolkit for developing
"GUI-like" application software that runs under a terminal emulator. It also
optimizes screen changes, in order to reduce the latency experienced when using
remote shells. 

