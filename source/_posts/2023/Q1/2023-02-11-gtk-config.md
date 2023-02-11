---
title: 'GTK User Configuration'
excerpt: 'GTK User Configuration'
date: 2023-02-11 09:55:22
tags:
  - Linux
  - Windows
categories:
  - Operating system
---

# GTK User Configuration

+ https://developer-old.gnome.org/gtk2/stable/GtkSettings.html
+ https://docs.gtk.org/gtk3/class.Settings.html
+ https://docs.gtk.org/gtk4/class.Settings.html

## GTK 2

GIMP (2.10.32) is the only useful GUI program that still uses GTK 2.

```
# vi ~/.gtkrc-2.0
# libgtk2.0-0 (2.24.33)

gtk-icon-theme-name = Adwaita
# gtk-cursor-theme-size = 16
gtk-font-name = "sans-serif 16"
# GTK+ itself use the following named icon sizes: gtk-menu, gtk-button, gtk-small-toolbar, gtk-large-toolbar, gtk-dnd, gtk-dialog.
gtk-icon-sizes = "panel-menu=16,16:panel=16,16:gtk-menu=16,16:gtk-large-toolbar=16,16:gtk-small-toolbar=16,16:gtk-button=16,16"
# gtk-icon-theme-name = "Tangerine"
gtk-theme-name = Greybird
gtk-key-theme-name = Emacs
```

## GTK 3

Most gnome programs now use GTK 3.

```
# mkdir -p  ~/.config/gtk-3.0/ && vi ~/.config/gtk-3.0/settings.ini
# libgtk-3-0 (3.24.36)

[Settings]
gtk-cursor-theme-name = Adwaita
gtk-cursor-theme-size = 16

gtk-font-name = sans-serif 16
# gtk-font-name = Consolas 16

# GTK+ itself use the following named icon sizes: gtk-menu, gtk-button, gtk-small-toolbar, gtk-large-toolbar, gtk-dnd, gtk-dialog.
gtk-icon-sizes = panel-menu=16,16:panel=16,16:gtk-menu=16,16:gtk-large-toolbar=16,16:gtk-small-toolbar=16,16:gtk-button=16,16
gtk-icon-theme-name = Adwaita
gtk-theme-name = Greybird
gtk-key-theme-name = Emacs
```

## GTK 4

Only a few gnome programs use GTK 4 today.

```
# mkdir -p  ~/.config/gtk-4.0/ && vi ~/.config/gtk-4.0/settings.ini
# libgtk-4-1 (4.8.3)

[Settings]
gtk-cursor-theme-name = Adwaita
gtk-cursor-theme-size = 16

gtk-font-name = sans-serif 16
# gtk-font-name = Consolas 16

# GTK+ itself use the following named icon sizes: gtk-menu, gtk-button, gtk-small-toolbar, gtk-large-toolbar, gtk-dnd, gtk-dialog.
gtk-icon-sizes = panel-menu=16,16:panel=16,16:gtk-menu=16,16:gtk-large-toolbar=16,16:gtk-small-toolbar=16,16:gtk-button=16,16
gtk-icon-theme-name = Adwaita
gtk-theme-name = Greybird
gtk-key-theme-name = Emacs
```
