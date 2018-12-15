---
title: How to reverse a list of words in a shell string?
description: How to reverse a list of words in a shell string?
date: 2019-11-30 20:03:42
tags:
  - Linux
categories: [Operating system, Linux]
permalink: reverse-words-shell
---

# How to reverse a list of words in a shell string?

## awk for loop

    echo "$str" | awk '{for (i=NF; i>1; i--) printf("%s ", $i); print $1}'

    echo "$str" | awk '{for (i=NF; i>0; i--) printf("%s%s", $i, (i>1?OFS:ORS))}'

## awk do loop

    echo "$str" | awk '{do printf("%s%s", $NF, (NF>1?FS:RS)); while(--NF)}'

## tac and tr

    echo $str | tr ' ' '\n' | tac | tr '\n' ' '; echo

    echo -n "$str" | tac -s ' '

    tac -s ' ' <<< "$str" | xargs

## string operator

    rwo(){for((i=${#*};i>0;i--)){echo ${!i};};}
    echo $(rwo $str)
