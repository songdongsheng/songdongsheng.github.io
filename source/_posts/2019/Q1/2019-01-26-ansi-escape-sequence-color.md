---
title: ANSI Escape Sequences Colors
description: ANSI Escape Sequences Colors
date: 2019-01-26 23:49:26
tags:
  - Linux
categories: [Operating system, Linux]
permalink: ansi-escape-sequence-color
---

# ANSI Escape Sequences Colors

[ANSI escape sequences](https://en.wikipedia.org/wiki/ANSI_escape_code) are non-printed text that is interpreted to change the format of some text. These can be used to beautify terminal prompts, text, or anything else that understands ANSI escape sequences.

## Add colors to the prompt

Sequences have different lengths. All sequences start with ESC (27 / hex 0x1B / oct 033), followed by a second byte in the range 0x40–0x5F (ASCII @A–Z[\]^_).
Colors must be followed by a m. The next part is the 1 preceding the actual color code. This indicates whether or not the color should be bold or not (0 for normal, 1 for bold).

To add colors to the shell prompt use the following export command syntax:

    '\e[x;ym $PS1 \e[m'

Where,

1. \e[ : Start color scheme.
2. x;y : Color pair to use (x;y)
3. $PS1 : Your shell prompt variable.
4. \e[m : Stop color scheme.

To set a red color prompt, type the following command:

    export PS1="\e[0;31m[\u@\h: \w]\$ \e[m "

You can also use tput command to set terminal and modify the prompt settings. For example, to display RED color prompt using a tput:

    export PS1="\[$(tput setaf 1)\][\u@\h:\w] $ \[$(tput sgr0)\]"

## Basic color codes listing

    Black      0;30       Dark Gray    1;30
    Red        0;31       Bold Red     1;31
    Green      0;32       Bold Green   1;32
    Yellow     0;33       Bold Yellow  1;33
    Blue       0;34       Bold Blue    1;34
    Purple     0;35       Bold Purple  1;35
    Cyan       0;36       Bold Cyan    1;36
    Light Gray 0;37       White        1;37

## A list of handy tput command line options

- tput bold – Bold effect
- tput rev – Display inverse colors
- tput sgr0 – Reset everything
- tput setaf {CODE}– Set foreground color, see color {CODE} table below for more information.
- tput setab {CODE}– Set background color, see color {CODE} table below for more information.

## Basic color codes for the tput command

Color {code}    | Color
----------------|--------
0               | Black
1               | Red
2               | Green
3               | Yellow
4               | Blue
5               | Magenta
6               | Cyan
7               | White

## Color variables

    export COLOR_NC='\e[0m' # No Color
    export COLOR_WHITE='\e[1;37m'
    export COLOR_BLACK='\e[0;30m'
    export COLOR_BLUE='\e[0;34m'
    export COLOR_LIGHT_BLUE='\e[1;34m'
    export COLOR_GREEN='\e[0;32m'
    export COLOR_LIGHT_GREEN='\e[1;32m'
    export COLOR_CYAN='\e[0;36m'
    export COLOR_LIGHT_CYAN='\e[1;36m'
    export COLOR_RED='\e[0;31m'
    export COLOR_LIGHT_RED='\e[1;31m'
    export COLOR_PURPLE='\e[0;35m'
    export COLOR_LIGHT_PURPLE='\e[1;35m'
    export COLOR_BROWN='\e[0;33m'
    export COLOR_YELLOW='\e[1;33m'
    export COLOR_GRAY='\e[0;30m'
    export COLOR_LIGHT_GRAY='\e[0;37m'
