---
title: Hexo搭建个人博客
date: 2018-05-13 21:51:31
tags: hexo
categories: 工具
---

## 准备工作

1) 安装Git. 将本地的网页和文章等内容发布或提交到GitHub上需要用到Git.
2) 安装Node.js. Hexo是基于Node.js的高效静态站点生成框架.
3) 安装Hexo. 在Node.js安装完成后, 通过控制台执行以下命令即可.

```bash
npm install hexo-cli -g
```
<!--more-->

## 创建博客

```bash
hexo init leiz2192.hexo
cd leiz2192.hexo
hexo new "YourBlogTitle" //会生成source/_posts/YourBlogTitle.md
```

然后撰写YourBlogTitle.md.

```bash
hexo g   //生成网页
hexo s   //s是server的缩写，启动服务, 通过http://localhost:4000/本地访问
```

## 发布博客

### 安装部署插件

```bash
npm install hexo-deployer-git --save
```

### 创建部署仓库

在GitHub上"Create a new repository", Repository name为***your-github-user-name***.github.io, 比如***leiz2192***.github.io

### 配置部署参数

在_config.yml中配置deploy.

```yaml
deploy:
  type: git
  repo: git@github.com:leiz2192/leiz2192.github.io.git
  branch: master
```

repo建议使用ssh方式地址, https方式在部署时会需要输入用户名和密码.

### 生成发布博客

```bash
hexo d -g  //生成网页并发布到GitHub
```

然后就可以访问[博客](https://leiz2192.github.io/).

## 维护博客

博客的网页在repo *leiz2192.github.io*中维护, 而博客的md文件可以在repo *leiz2192.hexo*中维护. 这样数据和网页分离, 便于管理.

而在电脑或系统改变了, git clone *leiz2192.hexo*后就可以继续维护博客, 而且历史文章不丢失.

另外, 利用git hook可以在每次git push时自动发布博客, 只需在.git/hooks目录中新增pre-push文件, 文件内容如下.

```bash
#!/bin/sh

hexo d -g

exit 0
```

## 修饰博客

### about

新建about page

```bash
hexo new page "about"
```

修改theme的`_config.yml`, 去注释`menu`中的`about`.

```yaml
menu:
  ......
  about: /about/ || user
```

然后修改`source/about/index.md`文件即可.

### tags

新建tags page

```bash
hexo new page "tags"
```

修改theme的`_config.yml`, 去注释`menu`中的`tags`.

```yaml
menu:
  ......
  tags: /tags/ || tags
```

然后修改`source/tags/index.md`文件, 增加`type: "tags"`

```yaml
---
title: tags
date: 2019-10-04 23:11:52
type: "tags"
---
```

### categories

新建categories page

```bash
hexo new page "categories"
```

修改theme的`_config.yml`, 去注释`menu`中的`categories`.

```yaml
menu:
  ......
  categories: /categories/ || th
```

然后修改`source/categories/index.md`文件, 增加`type: "categories"`

```yaml
---
title: categories
date: 2019-10-04 23:15:38
type: "categories"
---
```

### search

安装插件

```bash
npm install hexo-generator-searchdb --save
```

修改站点的配置文件`_config.yml`, 增加以下内容.

```yaml
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```

修改theme的`_config.yml`, 修改`local_search`的`enable`为**true**.

```yaml
local_search:
  enable: true
```

清理缓存后发布.

```bash
hexo clean
hexo d -g
```
