---
title: Editor Config
description: Editor Config
date: 2019-10-27 08:00:15
tags:
  - IDE
categories: [Programming, IDE]
permalink: editorconfig
---

# EditorConfig
- https://EditorConfig.org/

## What is EditorConfig?
EditorConfig helps maintain consistent coding styles for multiple developers working on the same project across various editors and IDEs. The EditorConfig project consists of a file format for defining coding styles and a collection of text editor plugins that enable editors to read the file format and adhere to defined styles. EditorConfig files are easily readable and they work nicely with version control systems.

## EditorConfig Specification
- https://editorconfig-specification.readthedocs.io/en/latest/

## Plugin Guidelines
- https://github.com/editorconfig/editorconfig/wiki/Plugin-Guidelines

## Example file

```
# top-most EditorConfig file
root = true

# default settings
[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true

indent_style = space
indent_size = 4
tab_width = 4

[Makefile]
indent_style = tab
tab_width = 4

[*.{bat,cmd}]
end_of_line = crlf

[*.{json,xml,xsd,yaml,yml}]
indent_style = space
indent_size = 2
```
