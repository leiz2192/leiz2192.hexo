---
title: Vagrant box保存路径修改
date: 2019-10-07 21:57:12
tags: vagrant
categories:  工程
---

vagrant add box时默认保存在目录`~/.vagrant.d`. 可以通过设置`VAGRANT_HOME`环境变量改变默认位置.

>VAGRANT_HOME can be set to change the directory where Vagrant stores global state. By default, this is set to ~/.vagrant.d. The Vagrant home directory is where things such as boxes are stored, so it can actually become quite large on disk.

<!--more-->

- Windows：

  用户变量

  ```bash
  setx VAGRANT_HOME"X:/your/path"
  ```

  系统变量

  ```bash
  setx VAGRANT_HOME"X:/your/path"/M
  ```

- Linux

  ```bash
  export VAGRANT_HOME='/path/to/vagrant_home'
  ```
