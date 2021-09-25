---
title: Linux Home and End Keys
description: Linux Home and End Keys
date: 2021-09-25 23:26:33
tags:
  - Operating system
  - Linux
categories:
  - Operating system
  - Linux
permalink: linux-home-and-end-keys
---

# Linux Home and End Keys

## Update ~/.profile

```bash
export TERM=xterm-256color
export LC_ALL=C.UTF-8

if [ $(id -u) -eq 0 ]; then # you are root, set red colour prompt
  PS1="\\[$(tput setaf 1)\\][\\u@\\h:\\w] \\$ \\[$(tput sgr0)\\]"
else # normal
  PS1="\\[$(tput setaf 2)\\][\\u@\\h:\\w] \\$ \\[$(tput sgr0)\\]"
fi
```

## Print home and end Keys

```bash
$ tput cols
80

$ tput khome | cat -v; echo
^[OH

$ tput kend | cat -v; echo
^[OF

$ cat -v
^[[5~^[[F^C
```

## Bind in ~/.bashrc

```bash
bind '"\e[1~":"\eOH"'
bind '"\e[4~":"\eOF"'
```

## Bind in ~/.tmux.conf

```bash
bind-key -n Home send Escape "OH"
bind-key -n End send Escape "OF"
```

## tput utility

### Color capabilities

+ tput setab [1-7] – Set a background color using ANSI escape
+ tput setb [1-7] – Set a background color
+ tput setaf [1-7] – Set a foreground color using ANSI escape
+ tput setf [1-7] – Set a foreground color

### Text mode capabilities

+ tput bold – Set bold mode
+ tput dim – turn on half-bright mode
+ tput smul – begin underline mode
+ tput rmul – exit underline mode
+ tput rev – Turn on reverse mode
+ tput smso – Enter standout mode (bold on rxvt)
+ tput rmso – Exit standout mode
+ tput sgr0 – Turn off all attributes

### Color code for tput

+ 0 – Black
+ 1 – Red
+ 2 – Green
+ 3 – Yellow
+ 4 – Blue
+ 5 – Magenta
+ 6 – Cyan
+ 7 – White

### Print 256 colors

```bash
printf '\e[48;5;%dm ' {0..255}; printf '\e[0m \n'
```

```bash
color(){
    for c; do
        printf '\e[48;5;%dm%03d' $c $c
    done
    printf '\e[0m \n'
}

IFS=$' \t\n'
# color {0..15}
for ((i=0;i<16;i++)); do
    color $(seq $((i*16)) $((i*16+15)))
done
```

![256 colors of xterm](xterm-256colors.png)
