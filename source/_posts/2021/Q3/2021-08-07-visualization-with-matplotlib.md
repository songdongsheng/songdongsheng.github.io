---
title: Visualization with Matplotlib
description: Visualization with Matplotlib
date: 2021-08-07 22:14:17
tags:
    - Programming
    - Python
categories: [Programming, Python]
permalink: visualization-with-matplotlib
---

# Visualization with Matplotlib

## Introduction

Matplotlib is a comprehensive library for creating static, animated, and interactive visualizations in Python.

Matplotlib makes easy things easy and hard things possible.

+ Create
    - Develop publication quality plots with just a few lines of code
    - Use interactive figures that can zoom, pan, update...
+ Customize
    - Take full control of line styles, font properties, axes properties...
    - Export and embed to a number of file formats and interactive environments
+ Extend
    - Explore tailored functionality provided by third party packages
    - Learn more about Matplotlib through the many external learning resources

## Installation

```shell
$ pip install -U Matplotlib pandas pycairo scipy wxPython
```

## Plotting

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 10, 100)

plt.plot(x, np.sin(x))
plt.plot(x, np.cos(x))

# fig.tight_layout()

plt.tight_layout()
plt.show()
```

## Saving Figures to File

```python
# rsvg-convert -d 300 -p 300 -f pdf -a --keep-image-data -o my_figure_3.pdf my_figure.svg
import matplotlib as mpl
mpl.use('wxAgg')

import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 10, 100)

plt.plot(x, np.sin(x))
plt.plot(x, np.cos(x))
plt.tight_layout()
plt.savefig('my_figure.png')
plt.savefig('my_figure.svg')
plt.savefig('my_figure.pdf')
```

## Matplotlib Backends

Matplotlib is a plotting library. It relies on some backend to actually render the plots.

If you omit the default backend parameter, the first working backend from [the following list](https://matplotlib.org/stable/tutorials/introductory/customizing.html#a-sample-matplotlibrc-file) is used:

```shell
MacOSX Qt5Agg Gtk3Agg TkAgg WxAgg Agg
```

To get backends that are interactive or integrate into your operating system GUI, you need to know what OS you're running and what kind of backends have been compiled into your matplotlib library.

There's no easy way to find out what backends are available to be used, instead you can only really find out either by trial and error, or if you have access to the compilation logs.

However you can find out what backend the matplotlib library is currently set to and you can find out the string keys that point to different backends.

This will show you what the default backend of matplotlib has been set to. If you don't have any confounding environment variables nor a matplotlibrc file.

```python
import matplotlib.pyplot as plt
print(plt.get_backend())

TkAgg

import matplotlib as mpl
print(mpl.matplotlib_fname())
```

The list of backend string keys can be acquired with:

```python
import matplotlib.rcsetup as rcsetup

print(rcsetup.interactive_bk)
print(rcsetup.non_interactive_bk)
print(rcsetup.all_backends)

['GTK3Agg', 'GTK3Cairo', 'MacOSX', 'nbAgg', 'Qt4Agg', 'Qt4Cairo', 'Qt5Agg', 'Qt5Cairo', 'TkAgg', 'TkCairo', 'WebAgg', 'WX', 'WXAgg', 'WXCairo']

['agg', 'cairo', 'pdf', 'pgf', 'ps', 'svg', 'template']

['GTK3Agg', 'GTK3Cairo', 'MacOSX', 'nbAgg', 'Qt4Agg', 'Qt4Cairo', 'Qt5Agg', 'Qt5Cairo', 'TkAgg', 'TkCairo', 'WebAgg', 'WX', 'WXAgg', 'WXCairo', 'agg', 'cairo', 'pdf', 'pgf', 'ps', 'svg', 'template']
```

You can then try to switch to it. These are the ways to do this:

The first is to use it just before importing pyplot.

```python
import matplotlib as mpl
mpl.use('wxAgg')
import matplotlib.pyplot as plt

plt.plot(range(20), range(20))
plt.tight_layout()
plt.savefig('linear.png')
plt.show()
```

The best way is to use an environment variable:

```shell
MPLBACKEND=Qt4Agg python -c 'import matplotlib.pyplot as plt; print(plt.get_backend())'
```

## Improve the resolution

### Change the dpi settings

```python
import matplotlib as mpl
import matplotlib.pyplot as plt

# figure.dpi: 100.0, savefig.dpi: figure
print("figure.dpi: {}, savefig.dpi: {}".format(plt.rcParams['figure.dpi'], plt.rcParams['savefig.dpi']))

plt.rcParams['figure.dpi'] = 300
# figure.dpi: 300.0, savefig.dpi: figure
print("figure.dpi: {}, savefig.dpi: {}".format(plt.rcParams['figure.dpi'], plt.rcParams['savefig.dpi']))
```

### Change the image format to a vector format

```python
import matplotlib as mpl
import matplotlib.pyplot as plt

# savefig.format: png
print("savefig.format: {}".format(plt.rcParams['savefig.format']))

plt.rcParams['savefig.format'] = 'svg'
# savefig.format: svg
print("savefig.format: {}".format(plt.rcParams['savefig.format']))
```

## Reference

+ [User's Guide](https://matplotlib.org/stable/users/index.html)
+ [Matplotlib's Gallery](https://matplotlib.org/stable/gallery/index.html)
+ [Pyplot function overview](https://matplotlib.org/stable/api/pyplot_summary.html)
