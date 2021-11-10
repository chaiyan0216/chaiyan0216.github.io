---
title: LaTex 介绍及安装
tags: latex
typora-root-url: ../../..
---

[LaTex](https://www.latex-project.org) 是基于 [Tex](https://zh.wikipedia.org/zh-cn/TeX) 的排版系统，适用于生成高印刷质量的科技、数学和物理文档（用来发 paper）。

首先说下 Tex 是个啥，Tex 其实是个软件，它是由美国的计算机教授高德纳编写，用来给计算机、物理、数学等科学界学术文档进行排版的软件。高德纳最早开始自行编写 TeX 的原因，是由于当时的电脑排版技术十分粗糙，已经影响到他的巨著《计算机程序设计艺术》的印刷质量。因此他以典型的黑客思维模式，决定自行编写一个排版软件：TeX。Tex 的版本号也非常极客，以 π 作为版本号，每次升级都会在当前版本号后增加一位数字，使其越来越接近 π。Tex 当前的版本号是 3.141592653。

再说下 LaTex 又是个啥，LaTex 是 Tex 中的一种格式，是建立在 Tex 基础上的宏语言，每一个 LaTex 命令最终都会被解释为 Tex 命令，可以简单理解为 LaTex 是对 Tex 软件命令的封装，这种封装是为了方便人们使用。LaTex 根据常见的排版习惯，定义了许多命令和模板，通过这些命令和模板，我们可以很快的得到漂亮的排版结果。

从使用者的角度，我认为理解上边的概念就可以了。如果我想要写一篇排版精美的文档或者论文，那么我通过遵从 LaTex 的语法，按照对应的语法写出我的文章后，利用 LaTex 去排版我的文章生成 pdf 就可以了。至于具体底层的原理就交给专业的人员去做吧。

所以，最终想要利用 LaTex 进行写作和排版，我们需要安装：Tex、LaTex 和 一款趁手的编辑器（vim、vs-code 或者一些专门为 LaTex 开发的编辑器）。下边是以 Linux 平台为例的安装步骤，在树莓派上安装步骤也跟下边一样。



#### 1. 安装准备

[Getting LaTeX](https://www.latex-project.org/get/) 给了各平台的 LaTex 发行版安装程序和镜像地址。

<img src="/images/get-latex.png" alt="get-latex" style="zoom:50%;" />

-   Linux：Tex Live
-   Mac：MacTex
-   Windows：MikTex

Linux 平台下不同发行版的软件源虽然包含 LaTex，但是大多数发行版都是包含的旧版本 LaTex，譬如 Ubuntu 20.04 源中包含了 2019 版的 LaTex， Raspbian Stable 包含的是 2018 版 LaTex。因此，并不推荐从软件源直接安装 LaTex，而是通过 Tex Live 进行安装。

Tex Live 是 Tex 及其相关程序在 GNU/Linux 及其他类 Unix 系统、Mac OS X 和 Windows 系统下的一套发行版，包括了 Tex, LaTex2ε, ConTEXt, METAFONT, MetaPost, BibTEX 等许多可执行程序；种类繁多的宏包、字体和文档，并支持世界上许多不同的语言。所以，通过安装 Tex Live，就可以满足我们的需求。

Tex Live 通过[网络进行安装](https://www.tug.org/texlive/acquire-netinstall.html)，其安装程序是一个 5M 左右的[压缩包](https://mirror.ctan.org/systems/texlive/tlnet/install-tl-unx.tar.gz)，[安装指南](https://www.tug.org/texlive/doc.html)中有详尽的安装步骤，按照指南中的步骤来就可以，不是很难，也有[中文版](https://www.tug.org/texlive/doc/texlive-zh-cn/texlive-zh-cn.pdf)。



#### 2. 安装 Tex Live

安装程序下载完成并解压后，在其根目录下有个名为 install-tl 的可执行 perl 脚本，直接在命令行输入 `sudo ./install-tl` 就可以启动安装程序。

Linux 默认会启动文本形式的安装节目，这个不需要 gui 就可以直接在 terminal 中进行操作。文本界面如下，需要输入对应的字母来进行配置。

```
======================> TeX Live installation procedure <=====================

======>   Letters/digits in <angle brackets> indicate   <=======
======>   menu items for actions or customizations      <=======
= help>   https://tug.org/texlive/doc/install-tl.html   <=======

 Detected platform: GNU/Linux on x86_64
 
 <B> set binary platforms: 1 out of 16

 <S> set installation scheme: scheme-full

 <C> set installation collections:
     40 collections out of 41, disk space required: 7231 MB

 <D> set directories:
   TEXDIR (the main TeX directory):
     /usr/local/texlive/2021
   TEXMFLOCAL (directory for site-wide local files):
     /usr/local/texlive/texmf-local
   TEXMFSYSVAR (directory for variable and automatically generated data):
     /usr/local/texlive/2021/texmf-var
   TEXMFSYSCONFIG (directory for local config):
     /usr/local/texlive/2021/texmf-config
   TEXMFVAR (personal directory for variable and automatically generated data):
     ~/.texlive2021/texmf-var
   TEXMFCONFIG (personal directory for local config):
     ~/.texlive2021/texmf-config
   TEXMFHOME (directory for user-specific files):
     ~/texmf

 <O> options:
   [ ] use letter size instead of A4 by default
   [X] allow execution of restricted list of programs via \write18
   [X] create all format files
   [X] install macro/font doc tree
   [X] install macro/font source tree
   [ ] create symlinks to standard directories

 <V> set up for portable installation

Actions:
 <I> start installation to hard disk
 <P> save installation profile to 'texlive.profile' and exit
 <Q> quit

Enter command: 
```

输入 b，可以进到平台选择界面，默认的检测结果应该是准确的。

```
===============================================================================
Available platforms:

   a [ ] Cygwin on Intel x86 (i386-cygwin)
   b [ ] Cygwin on x86_64 (x86_64-cygwin)
   c [ ] MacOSX current (10.14-) on ARM/x86_64 (universal-darwin)
   d [ ] MacOSX legacy (10.6-) on x86_64 (x86_64-darwinlegacy)
   e [ ] FreeBSD on x86_64 (amd64-freebsd)
   f [ ] FreeBSD on Intel x86 (i386-freebsd)
   g [ ] GNU/Linux on ARM64 (aarch64-linux)
   h [ ] GNU/Linux on ARMv6/RPi (armhf-linux)
   i [ ] GNU/Linux on Intel x86 (i386-linux)
   j [X] GNU/Linux on x86_64 (x86_64-linux)
   k [ ] GNU/Linux on x86_64 with musl (x86_64-linuxmusl)
   l [ ] NetBSD on x86_64 (amd64-netbsd)
   m [ ] NetBSD on Intel x86 (i386-netbsd)
   o [ ] Solaris on Intel x86 (i386-solaris)
   p [ ] Solaris on x86_64 (x86_64-solaris)
   s [ ] Windows (win32)

Actions: (disk space required: 7231 MB)
 <-> deselect all
 <+> select all
 <R> return to main menu
 <Q> quit

Enter letter(s) to (de)select platforms: 
```

输入 s，可以进到方案选择界面，从这里可以选择一套 “安装方案”，也就是对软件包集合的一个统一划分。

```
===============================================================================
Select scheme:

 a [X] full scheme (everything)
 b [ ] medium scheme (small + more packages and languages)
 c [ ] small scheme (basic + xetex, metapost, a few languages)
 d [ ] basic scheme (plain and latex)
 e [ ] minimal scheme (plain only)
 f [ ] ConTeXt scheme
 g [ ] GUST TeX Live scheme
 h [ ] infrastructure-only scheme (no TeX at all)
 i [ ] teTeX scheme (more than medium, but nowhere near full)
 j [ ] custom selection of collections

Actions: (disk space required: 7231 MB)
 <R> return to main menu
 <Q> quit

Enter letter to select scheme: 
```

输入 c，可以进到集合选择界面，collection (安装集合) 是比 scheme (方案) 要更细的一层，实际上一个方案包含了多个集合，而一个集合又包含了一到多个软件包，然后每个软件包 (Tex Live 中最小的组织单位) 则包含了实际的 Tex 宏文件，字体文件，等等。

如果觉得 collection 菜单对安装控制还不够细，可以在安装后使用 Tex Live Manager (tlmgr) 程序，它能在软件包一层控制安装。

在集合选择界面，我选择了：abclmDFGJOP，具体每个集合是什么作用，可以参考 [Tex Live宏包集合和自定义安装](https://zhuanlan.zhihu.com/p/133984428)。

```
===============================================================================
Select collections:

 a [X] Essential programs and files      w [ ] Italian                         
 b [X] BibTeX additional styles          x [ ] Japanese                        
 c [X] TeX auxiliary programs            y [ ] Korean                          
 d [ ] ConTeXt and packages              z [ ] Other languages                 
 e [ ] Additional fonts                  A [ ] Polish                          
 f [ ] Recommended fonts                 B [ ] Portuguese                      
 g [ ] Graphics and font utilities       C [ ] Spanish                         
 h [ ] Additional formats                D [X] LaTeX fundamental packages      
 i [ ] Games typesetting                 E [ ] LaTeX additional packages       
 j [ ] Humanities packages               F [X] LaTeX recommended packages      
 k [ ] Arabic                            G [X] LuaTeX packages                 
 l [X] Chinese                           H [ ] MetaPost and Metafont packages  
 m [X] Chinese/Japanese/Korean (base)    I [ ] Music packages                  
 n [ ] Cyrillic                          J [X] Graphics, pictures, diagrams    
 o [ ] Czech/Slovak                      K [ ] Plain (La)TeX packages          
 p [ ] US and UK English                 L [ ] PSTricks                        
 s [ ] Other European languages          M [ ] Publisher styles, theses, etc.  
 t [ ] French                            N [ ] Windows-only support programs   
 u [ ] German                            O [X] XeTeX and packages              
 v [ ] Greek                            
 P [X] Mathematics, natural sciences, computer science packages
 S [ ] TeXworks editor; TL includes only the Windows binary

Actions: (disk space required: 1866 MB)
 <-> deselect all
 <+> select all
 <R> return to main menu
 <Q> quit

Enter letter(s) to (de)select collection(s): 
```

最后在 main menu 中输入 i 进行安装。



#### 3. 安装后的操作

安装完 Tex Live 后，还有两个操作需要处理：环境变量配置、系统字体配置。

默认安装配置下没有勾选最后一项创建符号连接，那么就需要手动添加 LaTex 相关的程序路径到 PATH。

```
 <O> options:
   [ ] use letter size instead of A4 by default
   [X] allow execution of restricted list of programs via \write18
   [X] create all format files
   [X] install macro/font doc tree
   [X] install macro/font source tree
   [ ] create symlinks to standard directories
```

添加以下命令到对象的 shrc 中，具体路径需要根据自己系统上的路径和安装版本做调整。

```
export PATH=/usr/local/texlive/2021/bin/x86_64-linux:$PATH
export MANPATH=/usr/local/texlive/2021/texmf-dist/doc/man:$MANPATH
export INFOPATH=/usr/local/texlive/2021/texmf-dist/doc/info:$INFOPATH
```

XeTex 和 LuaTex 可以使用任何系统安装的字体，而不只是 Tex 目录树中的那些，但在 Linux 下需要把系统按如下配置一番 XeTex 才能找到随 Tex Live 安装的那些字体。

XeTex 安装后会在 TEXMFSYSVAR/fonts/conf/texlive-fontconfig.conf 创建一个必需的配置文件。（TEXMFSYSVAR：/usr/local/texlive/2021/texmf-var）

要在整个系统中使用 Tex Live 的字体，需要：

1.   sudo 将 texlive-fontconfig.conf 文件复制到 /etc/fonts/conf.d/09-texlive.conf

2.   运行 sudo fc-cache-fsv

如果你没有足够的权限执行上述操作，或者只需要把 Tex Live 字体提供给自己用，可以这么做:

1.   将 texlive-fontconfig.conf 文件复制到 ~/.fonts.conf
2.   运行 fc-cache-fv

Tex Live 包含一个叫 tlmgr 的程序，它可以用来管理安装后的系统。它的功能包括:

• 列出方案 (scheme)，集合和安装包;

• 安装、升级、备份、恢复、卸载软件包，并且能自动计算依赖关系; 

• 查找和列出软件包以及它们的描述;

• 列出、添加和删除不同平台的可执行文件;

• 改变安装选项，比如纸张大小和源文件位置。



#### 4. 测试安装结果

```shell
## 查看版本
$ tex --version
TeX 3.141592653 (TeX Live 2021)
kpathsea version 6.3.3
Copyright 2021 D.E. Knuth.
There is NO warranty.  Redistribution of this software is
covered by the terms of both the TeX copyright and
the Lesser GNU General Public License.
For more information about these matters, see the file
named COPYING and the TeX source.
Primary author of TeX: D.E. Knuth.

## 处理样例，生成结果
$ latex sample2e.tex
This is pdfTeX, Version 3.141592653-2.6-1.40.23 (TeX Live 2021) (preloaded format=latex)
 restricted \write18 enabled.
entering extended mode
(/usr/local/texlive/2021/texmf-dist/tex/latex/base/sample2e.tex
LaTeX2e <2021-06-01> patch level 1
L3 programming layer <2021-07-12>
(/usr/local/texlive/2021/texmf-dist/tex/latex/base/article.cls
Document Class: article 2021/02/12 v1.4n Standard LaTeX document class
(/usr/local/texlive/2021/texmf-dist/tex/latex/base/size10.clo))
(/usr/local/texlive/2021/texmf-dist/tex/latex/l3backend/l3backend-dvips.def)
No file sample2e.aux.
(/usr/local/texlive/2021/texmf-dist/tex/latex/base/omscmr.fd) [1] [2] [3]
(./sample2e.aux) )
Output written on sample2e.dvi (3 pages, 7576 bytes).
Transcript written on sample2e.log.

## 即时预览结果
$ xdvi sample2e.dvi
```

