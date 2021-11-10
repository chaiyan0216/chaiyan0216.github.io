---
title: LaTex 基本概念
tags: latex
typora-root-url: ../../..
---

[toc]



[之前的章节](https://zju-cy.github.io/2021/08/09/latex1)说明了怎么安装 LaTex，如果觉着安装麻烦或者不想在本地安装，其实还有很多线上网站可以直接用。

[Overleaf](https://www.overleaf.com) 就是一个在线编辑 LaTex 的网站，注册之后可以免费使用，虽然免费帐户有些同步、编译的限制，但对普通用户来说够用了。

之后的系列笔记我都会用 Overleaf 来测试，而系列笔记本身是对官方文档（[一份(不太)简短的 LATEX2ε 介绍](https://mirror.aarnet.edu.au/pub/CTAN/info/lshort/chinese/lshort-zh-cn.pdf)）的一个精减提炼。



### 一、 Hello World

话不多说，先上两个 hello world。

```latex
\documentclass{article}
\begin{document}
``Hello world!'' from \LaTeX.
\end{document}
```

```latex
\documentclass{ctexart}
\begin{document}
“你好，世界!”来自 \LaTeX{} 的问候。
\end{document}
```

在使用中文时，需要注意编译器要选 XeLaTex，不然会编译失败。



### 二、命令和结构

LaTex 的源代码为文本文件。这些文本除了文字本身，还包括各种命令，用在排版公式、划分文档结构、控制样式等等不同的地方。

**命令形式**

LaTex 命令大小写敏感，以反斜线 `\` 开头，为以下两种形式之一：

-   反斜线和后面的一串字母，如 `\LaTeX`，它们以任意非字母符号（空格、数字、标点等）为界限。
-   反斜线和后面的单个非字母符号，如 `\$`。

**忽略空格**

字母形式的命令忽略其后的所有连续空格。如果要人为引入空格，需要在命令后面加一对花括号阻止其忽略空格。

```latex
Shall we call ourselves \TeX users
or \TeX{} users?
```

**命令参数**

命令可以接收一些参数，参数的内容会影响命令的效果。

参数分为可选参数和必选参数：

-   可选参数以方括号 [ 和 ] 包裹
-   必选参数一般以花括号 { 和 } 包裹
-   带星号 * 命令，带星号和不带星号的命令效果有一定差异

**环境**

环境用以令一些效果在局部生效，或是生成特殊的文档元素，用法为一对命令 `\begin` 和 `\end`:

```latex
\begin{⟨environment name⟩}[⟨optional arguments⟩]{⟨mandatory arguments⟩}
...
\end{⟨environment name⟩}
```

**分组**

有些命令会对其后所有内容产生作用。

若要限制其作用范围，则需要使用分组。

使用一对花括号 { 和 } 作为分组，在分组中使用的命令被限制在分组内，不会影响到分组外的内容。

**代码结构**

LaTex 源码以一个 `\documentclass` 命令作为开头，它指定了文档使用的文档类。`document` 环境当中的内容是文档正文。

在 `\documentclass` 和 `\begin{document}` 之间的位置称为导言区。在导言区中一般会使用 `\usepackage` 命令调用宏包，以及进行文档的全局设置。

```latex
\documentclass{...} % ... 为某文档类 
% 导言区
\begin{document}
% 正文内容
\end{document}
% 此后内容会被忽略
```



### 三、文档类和宏包

**文档类**

文档类规定了源码所要生成的文档类型：普通文章、书籍、演示文稿、个人简历等等。

语法为：`\documentclass[⟨options⟩]{⟨class-name⟩}`。

LaTex 提供的标准文档类有以下三种：

-   article：文章格式的文档类，广泛用于科技论文、报告、说明文档等。
-   report：长篇报告格式的文档类，具有章节结构，用于综述、长篇论文、简单的书籍等。
-   book：书籍文档类，包含章节结构和前言、正文、后记等结构。

可选参数 ⟨options⟩ 为文档类指定选项，以全局地规定一些排版的参数，如字号、纸张大小、 单双面等等。

比如调用 article 文档类排版文章，指定纸张为 A4 大小，基本字号为 11pt，双面排版：`\documentclass[11pt,twoside,a4paper]{article}`。

标准文档类可指定选项：

-   10pt, 11pt, 12pt：指定文档的基本字号。默认为 10pt。
-   a4paper, letterpaper, ...：指定纸张大小，默认为美式信纸 letterpaper (8.5 × 11 英寸)。 可指定选项还包括 a5paper，b5paper，executivepaper 和 legalpaper。
-   twoside, oneside：指定单面/双面排版。双面排版时，奇偶页的页眉页脚、页边距不同。article 和 report 默认为 oneside，book 默认为 twoside
-   onecolumn, twocolumn：指定单栏/双栏排版。默认为 onecolumn。
-   openright, openany：指定新的一章 `\chapter` 是在奇数页（右侧）开始，还是直接紧跟着上一页开始。report 默认为 openany，book 默认为 openright。对 article 无效。
-   landscape：指定横向排版。默认为纵向。
-   titlepage, notitlepage：指定标题命令 `\maketitle` 是否生成单独的标题页。article 默认为 notitlepage，report 和 book 默认为 titlepage。
-   fleqn：令行间公式左对齐。默认为居中对齐。
-   leqno：将公式编号放在左边。默认为右边。
-   draft, final：指定草稿/终稿模式。草稿模式下，断行不良的地方会在行尾添加一个黑色方 块。默认为 final。

**宏包**

在使用 LaTex 时，时常需要依赖一些扩展来增强或补充 LaTex 的功能，比如排版复杂的表 格、插入图片、增加颜色甚至超链接等等。这些扩展称为宏包。

调包语法：`\usepackage[⟨options⟩]{⟨package-name⟩}`。

帮助语法：`texdoc ⟨pkg-name⟩`。



### 四、文件类型

LaTex 源码是以 .tex 为后缀的文本文件，但在 LaTex 使用和编译过程中还会遇到很多其它后缀的文件，对应文件的说明如下：

-   .sty 宏包文件。宏包的名称与文件名一致。
-   .cls 文档类文件。文档类名称与文件名一致。
-   .bib BIBTEX 参考文献数据库文件。
-   .bst BIBTEX 用到的参考文献格式模板。
-   .log 排版引擎生成的日志文件，供排查错误使用。
-   .aux LATEX 生成的主辅助文件，记录交叉引用、目录、参考文献的引用等。 .toc LATEX 生成的目录记录文件。
-   .lof LATEX 生成的图片目录记录文件。
-   .lot LATEX 生成的表格目录记录文件。
-   .bbl BIBTEX 生成的参考文献记录文件。
-   .blg BIBTEX 生成的日志文件。
-   .idx LATEX 生成的供 makeindex 处理的索引记录文件。
-   .ind makeindex 处理 .idx 生成的用于排版的格式化索引文件。
-   .ilg makeindex 生成的日志文件。
-   .out hyperref 宏包生成的 PDF 书签记录文件。



### 五、文件组织

当编写长篇文档时，例如当编写书籍、毕业论文时，单个源文件会使修改、校对变得十分困难。将源文件分割成若干个文件，例如将每章内容单独写在一个文件中，会大大简化修改和校对的工作。

命令 `\include` 用来在源代码里插入文件:

```latex
\include{⟨filename⟩}
\include{chapters/a.tex} % 相对路径
\include{/home/Bob/file.tex} % 绝对路径
```

命令 `\include` 在读入文件之前会另起一页，但有时我们并不想这样，而是用 `\input` 命令进行插入：`\input{⟨filename⟩}`。

最后工具宏包 syntonly 可令 LaTex 编译后不生成 DVI 或者 PDF 文档，只排查错误，编译速度会快不少。

```latex
\usepackage{syntonly}
\syntaxonly
```



### 六、术语和概念

-   引擎

    ​	全称为排版引擎，是编译源代码并生成文档的程序，如 pdfTEX、XƎTEX 等。有时也称为编译器。

-   格式

    ​	定义了一组命令的代码集。LaTex 就是最广泛应用的一个格式。

-   编译命令

    ​	实际调用的、结合了引擎和格式的命令。如 `xelatex` 命令是结合 XƎTEX 引擎和 LaTex 格式的一个编译命令。

几个编译命令的特点：

-   latex

    ​	虽然名为 latex 命令，底层调用的引擎其实是 pdfTEX。该命令生成 dvi(Device Inde-pendent) 格式的文档，用 dvipdfmx 命令可以将其转为 pdf。

-   pdflatex

    ​	底层调用的引擎也是 pdfTEX，可以直接生成 pdf 格式的文档。

-   xelatex

    ​	底层调用的引擎是 XƎTEX，支持 UTF-8 编码和对 TrueType / OpenType 字体的调用。 当前较为方便的中文排版解决方案基于 xelatex。

-   lualatex

    ​	底层调用的引擎是 LuaTEX，这个引擎在 pdfTEX 引擎基础上发展而来，除了支持 UTF-8 编码和对 TrueType / OpenType 字体的调用外，还支持通过 Lua 语言扩展 TEX 的功能。lualatex 编译命令下的中文排版支持需要借助 luatexja 宏包。
