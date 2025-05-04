---
title: Hexo换主题
date: 2025-05-04 13:45:56
tags: hexo
categories: 工具
---

本人的[个人博客](https://leiz2192.github.io/)之前使用的主题是 [NexT](https://github.com/next-theme/hexo-theme-next)，现在切换到主题 [Sea](https://github.com/hai-zou/hexo-theme-sea)，过程记录下，要是下次还要切换主题，可以参考下。

# npm、pnpm 和 cnpm

依赖包管理我改用了 pnpm。这里简单介绍下 npm 和 pnpm 的区别。

* npm 是 Node.js 默认的包管理工具，与大多数 Node.js 项目和工具具有良好的兼容性，有着庞大的社区支持和丰富的软件包资源，但在国内访问官方仓库比较慢，再加上下载依赖包时可能会重复下载，也会多占用磁盘空间。
* pnpm 采用了内容可寻址存储（CAS）和硬链接 / 符号链接相结合的方式，避免了依赖包的重复安装，可以节省磁盘空间，下载也会快些，并且能更精确地管理依赖版本。但在一些特殊情况下，可能会遇到与某些工具或脚本不兼容的问题，因为它的 `node_modules` 结构与 npm 不同。

```bash
# 通过 npm 全局安装
npm install -g pnpm
# 安装完成查询版本
pnpm version
```

* cnpm 是淘宝团队推出的 npm 镜像工具，主要是为了解决国内访问 npm 官方仓库速度慢的问题。cnpm 保持了与 npm 的高度兼容性，几乎所有 npm 的命令都可以在 cnpm 中直接使用。

```bash
# 通过 npm 全局安装
npm install -g cnpm --registry=https://registry.npmmirror.com
# 安装完成查询版本
cnpm version
```

# 换主题

换主题过程主要是参考 [Sea](https://github.com/hai-zou/hexo-theme-sea) 主页，主要是安装和配置。

先是安装，直接通过 pnpm 安装。之前我是将主题放到 themes 目录下的，这样不仅要维护主题文件，后续更新也比较麻烦，还是直接安装简单些。

```bash
pnpm install hexo-theme-sea
```

再是配置，这里涉及到两个配置文件，分别是 \_config.yml 和 \_config.sea.yml，还是参考主页中说明配置的。

\_config.sea.yml 是基于主题的 [_config.yml](about:blank) 修改的，主要配置了 menu 和 socialLink 两部分。

```yaml
menu:
  - name: 首页
    url: /
  - name: 归档
    url: /archives/
  - name: 标签
    url: /tags/
  - name: 分类
    url: /categories/
  - name: 关于
    url: /about/

socialLink:
  - name: Email
    link: mailto:zhanglei2192@hotmail.com
  - name: GitHub
    link: https://github.com/leiz2192
```

\_config.yml 主要修改添加了下面几个配置

```yaml
language: zh
syntax_highlighter: prismjs
highlight:
  enable: false
prismjs:
  enable: true
  preprocess: true
  line_number: true
  line_threshold: 0
  tab_replace: ''
theme: sea
```

配置完成后先本地生成看看

```yaml
hexo clean
hexo g
hexo s
```

# 升级 hexo

过程中发现使用的 hexo 版本有点老了，就参考[博客升级 Hexo 版本记录](https://note.weizwz.com/hexo/basic/hexo-update)进行了下升级。

用 npm-check 检查是否需要升级

```yaml
pnpm install -g npm-check
npm-check
```

用 npm-upgrade 更新 package.json

```yaml
pnpm install -g npm-upgrade
npm-upgrade
```

然后升级系统项

```yaml
pnpm update --save
```

# 图床

之前我是用 hexo-asset-image 将本地图片插入博客的。但升级 hexo 过程中发现之前使用的 [hexo-asset-image](https://github.com/xcodebuild/hexo-asset-image) 已经很久没有更新了，GitHub 仓库已经归档。

所以参考[网上资料](https://zhuanlan.zhihu.com/p/532669042)将博客图片换成了图床形式。使用 GitHub 作为图床，借助 PicGo 将博客中的图片换成了图床形式，然后 remove 了 hexo-asset-image。

```bash
pnpm remove hexo-asset-image
```
