---
title: 常见的 Windows 启动程序位置
description: 如何从常见的 Windows 启动程序位置找到并且删除项目，或者增加启动程序
date: 2018-11-25 18:46:49
tags:
  - programming
  - windows
categories: [编程, windows]
permalink: windows-boot-startup
---

# 常见的 Windows 启动程序位置

## Windows 启动程序位置
### 注册表

    HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run

### 硬盘位置

在开始菜单的运行输入框中，输入 “shell:startup” 或者 “shell:common startup”，也可以打开以下对应的位置：

    %APPDATA%\Microsoft\Windows\Start Menu\Programs\Startup\
    %ProgramData%\Microsoft\Windows\Start Menu\Programs\Startup\

## 禁用、删除或者增加启动程序

### 禁用或启用启动程序

打开任务管理器，点击“启动”页面，找到启动项目，点击右键，选择“禁用”或“启用”。

### 删除启动程序

+ 尽量不要删除项目，强烈建议通过任务管理器来禁用它。
+ 要删除启动项目，你只需在找到的位置删除即可。
+ 备份后再删除，并且使用重启系统来验证。

### 增加启动程序

要增加启动项目，不建议修改注册表，请在硬盘位置增加快捷方式即可。
