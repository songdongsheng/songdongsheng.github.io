---
title: Windows 命令行窗口的启动目录
description: 描述 Windows 命令行窗口的启动目录的位置，以及修改方法
date: 2018-11-24 18:46:49
tags:
  - Windows
categories: [Operating system, Windows]
permalink: cmd-starting-directory
---

# Windows 命令行窗口的启动目录

## 菜单中的命令提示符的链接文件

### 当前用户配置

```
%LOCALAPPDATA%\Microsoft\Windows\WinX\Group3\01 - Command Prompt.lnk
%LOCALAPPDATA%\Microsoft\Windows\WinX\Group3\02 - Command Prompt.lnk
%APPDATA%\Microsoft\Windows\Start Menu\Programs\System Tools\Command Prompt.lnk
```

### 默认用户配置

```
%USERPROFILE%\..\Default\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\System Tools\Command Prompt.lnk
%USERPROFILE%\..\Default\AppData\Local\Microsoft\Windows\WinX\Group3\01 - Command Prompt.lnk
%USERPROFILE%\..\Default\AppData\Local\Microsoft\Windows\WinX\Group3\02 - Command Prompt.lnk
```

## 修改方法

在对应链接文件上点击右键，修改起始位置为你需要的目录即可。
