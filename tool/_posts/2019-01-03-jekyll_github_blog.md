---
title: 使用 Jekyll 和 GitHub 搭建个人博客
tags: jekyll github
---

## 一、Jekyll 环境搭建

[Jekyll 官网](https://jekyllrb.com/)、[Jekyll 中文网](http://jekyllcn.com/)

- Jekyll 介绍

  Jekyll 是一个简单的博客形态的静态站点生产机器。它有一个模版目录，其中包含原始文本格式的文档，通过一个转换器（如 [Markdown](http://daringfireball.net/projects/markdown/)）和 [Liquid](https://github.com/Shopify/liquid/wiki) 渲染器转化成一个完整的可发布的静态网站，你可以发布在任何你喜爱的服务器上。Jekyll 也可以运行在 [GitHub Page](http://pages.github.com/) 上，也就是说，你可以使用 GitHub 的服务来搭建你的项目页面、博客或者网站，而且是**完全免费**的。

- Jekyll 结构

  Jekyll 的核心其实是一个文本转换引擎。它的概念其实就是：你用你最喜欢的标记语言来写文章，可以是 Markdown, 也可以是 Textile，或者就是简单的 HTML，然后 Jekyll 就会帮你套入一个或一系列的布局中。在整个过程中你可以设置 URL 路径，你的文本在布局中的显示样式等等。这些都可以通过纯文本编辑来实现，最终生成的静态页面就是你的成品了。

  一个基本的 Jekyll 网站的目录结构一般是这样：

  ```
  .
  ├── _config.yml
  ├── _drafts
  |   ├── begin-with-the-crazy-ideas.textile
  |   └── on-simplicity-in-technology.markdown
  ├── _includes
  |   ├── footer.html
  |   └── header.html
  ├── _layouts
  |   ├── default.html
  |   └── post.html
  ├── _posts
  |   ├── 2007-10-29-why-every-programmer-should-play-nethack.textile
  |   └── 2009-04-26-barcamp-boston-4-roundup.textile
  ├── _site
  ├── .jekyll-metadata
  └── index.html
  ```

- Jekyll 安装

  1. 安装 ruby

  2. 更改 gem 源

     ```console
     $ gem source -r https://rubygems.org/
     $ gem source -a https://gems.ruby-china.com
     $ gem source
     *** CURRENT SOURCES ***
      
     https://gems.ruby-china.com
     ```

  3. 安装 Jekyll

     ```console
     $ sudo gem install jekyll bundle
     ```

  4. 新建

     ```console
     $ jekyll new myblog
     ```

  5. 运行

     ```console
     $ cd myblog
     $ jekyll serve
     Server address: http://127.0.0.1:4000/
     Server running... press ctrl-c to stop.
     ```

  6. 本地查看

     ```
     浏览器输入 http://127.0.0.1:4000/
     ```

  7. 写博文

     发表一篇新文章，需要做的就是在 `_posts` 文件夹中创建一个新的文件。

     文件名的命名非常重要。Jekyll 要求一篇文章的文件名遵循下面的格式：

     ```
     年-月-日-标题.MARKUP
     2011-12-31-new-years-eve-is-awesome.md
     2012-09-12-how-to-write-a-blog.textile
     ```

## 二、GitHub Pages

[GitHub Pages 官网](https://pages.github.com/)

- GitHub Pages 介绍

  GitHub Pages 可以为你或者你的项目提供介绍网页，它是由 GitHub 官方托管和发布的。你可以使用 GitHub 提供的页面自动生成器。也可以做个人博客，是个轻量级的博客系统，没有麻烦的配置。使用标记语言如Markdown，不需自己搭建服务器，还可以绑定自己的域名。

- GitHub Pages 使用

  GitHub Pages 使用非常简单，直接参考官网教程。

  1. GitHub 上创建新仓库，

     ```
     新仓库名为：username.github.io
     username 是自己 GitHub 用户名，如果没有使用自己的用户名，个人博客可能会无法访问。
     ```

  2. 将新建仓库克隆到本地

     ```console
     $ git clone https://github.com/username/username.github.io
     ```

  3. 在本地仓库创建 index.html 文件

     ```console
     $ cd username.github.io
     $ echo "Hello World" > index.html
     ```

  4. 把本地仓库 push 到 GitHub 上

     ```console
     $ git add .
     $ git commit -m "Initial commit"
     $ git push -u origin master
     ```

  5. 访问

     ```
     浏览器访问：https://username.github.io
     可以看到刚才写入 index.html 的 “Hello World”
     ```

## 三、搭配 Jekyll 和 GitHub

- 不使用主题样式

  直接把 Jekyll 生成文件夹下的所有内容 copy 到 `username.github.io` 的本地仓库即可。

  1. Jekyll 生成文件夹
  2. copy 文件夹下所有内容到自己的本地仓库
  3. 本地仓库 push 到 GitHub
  4. 浏览器访问

- 使用主题样式

  [Jekyll Themes](http://jekyllthemes.org/)，有各种主题样式可以下载，也可以直接在网上搜索喜欢的主题样式，都是大牛做好的。

  1. 下载或者克隆主题
  2. copy 主题文件夹下所有内容到自己的本地仓库
  3. 本地仓库 push 到 GitHub
  4. 浏览器访问

