---
title: Hexo博客添加图片
date: 2019-11-08 22:49:58
tags: hexo
categories: 工具
---

> Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页

对于在 Hexo 博客中添加图片,  按照 [Hexo文档](https://hexo.io/zh-cn/docs/) 推荐的方法, 操作大致如下:

- 设置站点配置`_config.yml`: 将`post_asset_folder: false`改为`post_asset_folder: true`.
- 安装插件: `npm install hexo-asset-image --save`.
- 生成博客: 运行`hexo n "your-title"`, 生成`your-title.md`博文时就会在`/source/_posts`目录下生成`your-title`的文件夹, 将你想在`your-title`博文中插入的照片放置到这个同名文件夹中即可, 图片的命名随意.
- 添加图片: 在需要添加图片的位置写入图片的链接地址: `![your-image-desc](your-image-name.your-image-format)`.

<!--more-->

另外一种图片存放路径是`source/images`. 但此时访问图片的方式是`![your-image-desc](/images/your-image-name.your-image-format)`

但如果这样, 当需要在博文中插入一张截图时, 还需要先将截图保存为此目录下的文件, 再在博文中手工添加图片的链接地址.

有没有更快捷的方法?

答案是**有**: 在截图工具将截图复制到剪切板后, 在博文中粘贴一下, 图片文件自动生成, 图片链接地址也自动生成.

如果 Hexo 博文是在`VS Code`中完成的, 通过对`Paste Image`扩展进行一定的配置就可以实现图片的方便插入.

> 安装`Paste Image`在`VS Code`的应用商店中搜索安装即可. 另外 Linux 环境可能会提示安装`xclip`, 比如`sudo aptitude install xclip`.

- 配置基本路径. 图片的链接路径中不体现的路径信息. 比如 VS Code 打开是 Hexo 的工程, 图片是存在`source/images`中, 则配置`${projectRoot}/source`.
![base-path](/images/20191109T004150.933.png)

- 配置图片文件名格式. 名称是由时间组成的, 但格式可自定义.
![file-name](/images/20191109T005128.154.png)

- 配置图片保存路径.
![paste-path](/images/20191109T005753.953.png)

- 配置图片链接前缀. 如果图片保存在`source/images`中, 图片链接路径最前面需要`/`.
![paste-prefix](/images/20191109T005926.235.png)

按照上述配置以后, 截图保存到剪贴板后, 直接在博文中`Ctrl+Alt+V`(不同系统快捷键可能存在差异), 图片就自动保存到指定路径, 博文中自动生成了图片的访问地址.
