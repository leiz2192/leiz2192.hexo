---
title: Windows Terminal集成第三方Bash
date: 2020-06-07 23:06:34
tags: shell
categories: 工具
---

将Git Bash和Cygwin集成到Windows Terminal中。

各参数说明参见[Windows 终端中的配置文件设置](https://docs.microsoft.com/zh-cn/windows/terminal/customize-settings/profile-settings)。

我的配置如下。其中guid自定义，确保其唯一性和格式正确行即可；commandline修改为实际安装路径，其中我的Cygwin64配置是以zsh启动。
<!--more-->
Git Bash设置：

```json
{
    "guid" : "{0caa0dad-35be-5f56-a8ff-afceeeaa6109}",
    "commandline" : "C:\\Program Files\\Git\\bin\\bash.exe --login -i",
    "icon" : "C:\\ProgramData\\Microsoft\\Windows\\Start Menu\\Programs\\Git\\gwindows_logo.png",
    "name" : "Git Bash",
    "startingDirectory" : "%USERPROFILE%"
}
```

Cygwin64设置：

```json
{
    "guid" : "{0caa0dad-35be-5f56-35be-0a998ec441b8}",
    "commandline" : "C:\\cygwin64\\bin\\zsh.exe --login -i",
    "icon" : "C:\\cygwin64\\Cygwin.ico",
    "name" : "Cygwin64",
    "startingDirectory" : "%USERPROFILE%"
}
```

应用于所有配置文件的设置：

```json
{
    // Put settings here that you want to apply to all profiles.
    "acrylicOpacity" : 0.8,
    "closeOnExit" : true,
    "colorScheme": "Solarized Dark",
    "padding" : "0, 0, 0, 0",
    "cursorShape" : "bar",
    "fontSize" : 12,
    "snapOnInput" : true,
    "historySize" : 9001,
    "useAcrylic" : true
}
```
