---
title: Vagrant使用
date: 2019-10-07 22:16:32
tags: vagrant
categories: 工程
---


## VirtualBox安装

VirtualBox是Oracle开源的虚拟化系统，它支持多个平台，所以你可以到[*官方网站*](https://www.virtualbox.org/wiki/Downloads/)下载适合你平台的VirtualBox最新版本并安装，安装过程选择默认设置即可。

## Vagrant安装

在[*官方网站*](http://www.vagrantup.com/downloads.html)下载安装包,安装过程选择默认设置即可。

尽量下载最新的程序，因为VirtualBox经常升级，升级后有些接口会变化，老的Vagrant可能无法使用。

检测安装是否成功的方法是，在终端命令行工具中输入vagrant，看看程序是不是已经可以运行了。如果不行，请检查一下`$PATH`里面是否包含vagrant所在的路径。

<!--more-->

## Vagrant配置

一个打包好的操作系统在Vagrant中称为Box，即Box是一个打包好的操作系统环境。[vagrantbox.es](http://www.vagrantbox.es/)上面有大家熟知的大多数操作系统。建议下载后安装，这样安装过程快速。

## 建立开发环境目录

根据自己的系统不同建立一个目录就可以。

## 下载box

前面讲了box是一个操作系统环境，实际上它是一个zip包，包含了Vagrant的配置信息和VirtualBox的虚拟机镜像文件.比如[*Ubuntu lucid 64*]( http://files.vagrantup.com/lucid64.box)

## 添加box

添加box的命令如下：

```bash
vagrant box add base 远端的box地址或者本地的box文件名
```

base是box的名称，可以是任意的字符串，base是默认名称，主要用来标识一下你添加的box，后面的命令都是基于这个标识来操作的。

例子：

```bash
vagrant box add base http://files.vagrantup.com/lucid64.box
vagrant box add base https://dl.dropbox.com/u/7225008/VagrantCentOS-6.3-x86_64-minimal.box
vagrant box add base CentOS-6.3-x86_64-minimal.box
vagrant box add "CentOS 6.3 x86_64 minimal" CentOS-6.3-x86_64-minimal.box
```

例如：

```bash
vagrant box add base lucid64.box
```

安装过程的信息：

```bash
Downloading or copying the box...
Extracting box...te: 47.5M/s, Estimated time remaining: --:--:--)
Successfully added box 'base' with provider 'virtualbox'!
```

box中的镜像文件在window系统中应该是放到了： C:\Users\当前用户名\.vagrant.d\boxes\目录下。

## 初始化

初始化的命令如下：

```bash
vagrant init
```

如果你添加的box名称不是base，那么需要在初始化的时候指定名称，例如

```bash
vagrant init "CentOS 6.3 x86_64 minimal"
```

初始化过程的信息：

```bash
A `Vagrantfile` has been placed in this directory.
You are now ready to `vagrant up` your first virtual environment!
Please read the comments in the Vagrantfile as well as documentation on `vagrantup.com` for more information on using Vagrant.
```

## 启动虚拟机

启动虚拟机的命令如下：

```bash
vagrant up
```

启动过程的信息:

```bash
Bringing machine 'default' up with 'virtualbox' provider...
[default] Importing base box 'base'...
[default] Matching MAC address for NAT networking...
[default] Setting the name of the VM...
[default] Clearing any previously set forwarded ports...
[default] Creating shared folders metadata...
[default] Clearing any previously set network interfaces...
[default] Preparing network interfaces based on configuration...
[default] Forwarding ports...
[default] -- 22 => 2222 (adapter 1)
[default] Booting VM...
[default] Waiting for VM to boot. This can take a few minutes.
[default] VM booted and ready for use!
[default] Mounting shared folders...
[default] -- /vagrant
```

## 连接到虚拟机

上面已经启动了虚拟机，之后我们就可以通过ssh来连接到虚拟机了。比如在我的开发机中可以像这样来连接：

```bash
vagrant ssh
```

连接到虚拟机后的信息如下：

```bash
Linux lucid64 2.6.32-38-server #83-Ubuntu SMP Wed Jan 4 11:26:59 UTC 2012 x86_64 GNU/Linux
Ubuntu 10.04.4 LTS

Welcome to the Ubuntu Server!
 * Documentation:  http://www.ubuntu.com/server/doc
New release 'precise' available.
Run 'do-release-upgrade' to upgrade to it.

Welcome to your Vagrant-built virtual machine.
Last login: Fri Sep 14 07:31:39 2012 from 10.0.2.2
```

这样我们就可以像连接到一台服务器一样进行操作了。
*Window机器不支持这样的命令，必须使用第三方客户端来进行连接，例如putty、Xshell等.*

```bash
主机地址: 127.0.0.1
端口: 2222
用户名: vagrant
密码: vagrant
```
