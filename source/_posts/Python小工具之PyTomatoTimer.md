---
title: Python小工具之PyTomatoTimer
date: 2021-09-05 22:43:21
tags: PySimpleGUI
categories: Python
---

代码见[GitHub](https://github.com/leiz2192/PyTomato)。

利用 [PySimpleGUI](https://pysimplegui.readthedocs.io/en/latest/) 和 [pygame](https://www.pygame.org/docs/)  写的一个小工具，用于番茄计时。

![looks-like](/images/20210905T225045.549.png)

- 记录运行和中断的次数
- 倒计时可通过配置文件 "PyTomato.json" 配置，单位为秒
- 双击倒计时部分关闭
<!--more-->

利用 pygame 播放音乐，如果音乐不存在或无法打开，则跳过播放部分。

```python
def init_music(music_file):
    try:
        pygame.init()
        mixer.init()
        mixer.music.load(os.path.join(os.getcwd(), music_file))
        mixer.music.set_volume(1.0)
        mixer.music.play(-1)
        mixer.music.pause()
    except Exception as e:
        print("exception when init music", e)
        global USE_MUSIC
        USE_MUSIC = False

def music_pause():
    if not USE_MUSIC:
        return
    mixer.music.pause()

def music_unpause():
    if not USE_MUSIC:
        return
    mixer.music.unpause()
```


利用了 Frame 实现元素的多元排列。

```python
def init_layout(default_time, run_times, pause_times):
    frame_layout = [[sg.Button('Run', font=("Consolas", 16), button_color=('#FFFFFF', '#404040'), border_width=0, key='Play')],
                    [sg.Text(times_text(run_times, pause_times), font=("Consolas", 9), auto_size_text=True, key="Times")]]
    return [[sg.Text(countdown_time_text(default_time),
                     size=(6, 1),
                     font=("Consolas", 27),
                     justification='center',
                     key='text',
                     enable_events=True),
             sg.Frame("", layout=frame_layout, element_justification="center", border_width=0)]]
```
