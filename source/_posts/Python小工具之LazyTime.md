---
title: Python小工具之LazyTime
date: 2021-09-05 00:40:04
tags: PySimpleGUI
categories: Python
---

代码见[GitHub](https://github.com/leiz2192/lazytime)。

利用 [PySimpleGUI](https://pysimplegui.readthedocs.io/en/latest/) 和 [Borax.Calendars](https://gitee.com/kinegratii/borax)  写的一个小工具，用于显示日期、时间、星期和农历信息。

Win10运行参考界面如下，可拖拽，双击鼠标关闭。
![win10](/images/20210905T004807.858.png)


时间信息获取方式：

```python
import datetime
from borax.calendars.lunardate import LunarDate

def get_lazy_time():
    prefix = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S %A")
    today = LunarDate.today()
    suffix = "{}月{} {}{}年".format(today.cn_month, today.cn_day, today.gz_year, today.animal)
    return "{} {}".format(prefix, suffix)
```
<!--more-->
通过多线程更新界面时间信息，每秒更新一次，默认 Thread 没有被动停止方法，所以继承 Thread 自定义实现。

```python
import time
import threading
import PySimpleGUI as sg

class UpdateLazyTime(threading.Thread):
    def __init__(self, window):
        super(UpdateLazyTime, self).__init__()
        self._stop = False
        self._window = window

    def run(self) -> None:
        while True:
            time.sleep(1)
            if self._stop:
                break
            self._window["lazy_time"].update(get_lazy_time())

    def stop(self):
        self._stop = True

sg.theme('Reddit')
layout = [[sg.Text(get_lazy_time(), font=("Microsoft YaHei", 10), key="lazy_time", enable_events=True)]]
window = sg.Window('', layout, no_titlebar=True, grab_anywhere=True, keep_on_top=True, margins=(0,0), alpha_channel=0.75)
uthread = UpdateLazyTime(window)
uthread.start()
```

双击关闭通过检查两次点击间隔时间小于 1s 实现，前提是 sg.Text 的 enable_events 设置为 True，否则无法接收到事件。

```python
last_time = None
while True:
    event, _ = window.read()
    if event != "lazy_time":
        continue
    if last_time and (datetime.datetime.now() - last_time).seconds < 1:
        break
    last_time = datetime.datetime.now()
```
