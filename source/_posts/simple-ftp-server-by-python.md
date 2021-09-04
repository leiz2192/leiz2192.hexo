---
title: 基于Python的简单FTP服务器
date: 2020-11-21 18:20:46
tags: PySimpleGUI
categories: Python
---

代码见[GitHub](https://github.com/leiz2192/Python/tree/master/PyFtpServer)。

UI使用基于tk的PySimpleGUI，FTP服务器使用pyftpdlib，使用pyinstaller进行打包。

- Win10运行参考界面
![win界面](/images/20201129T195324.321.png)

- Ubuntu20.04运行参考界面
![ubunt界面](/images/20201129T203840.135.png)

- 打包命令

    ```bash
    PyInstaller -F -w py_ftp_server.py
    ```
