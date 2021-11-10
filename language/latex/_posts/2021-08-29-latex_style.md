---
title: LaTex 排版样式
tags: latex
typora-root-url: ../../..
---

[toc]



本章关注如何修改 LATEX 的排版样式。



### 一、字体和字号

LaTex 提供了两组修改字体的命令，如下图。其中诸如 `\bfseries` 形式的命令将会影响之后所有的字符，如果想要让它在局部生效，需要用花括号分组，也就是写成 `{\bfseries ⟨some text⟩}` 的形式；对应的 `\textbf` 形式带一个参数，只改变参数内部的字体，更为常用。

<img src="/images/latex/font_style.png" alt="font_style" style="zoom:50%;" />

下图列出了字号命令在标准文档类中的绝对大小，单位为 pt。使用字号命令的时候，通常也需要用花括号进行分组，如同 `\rmfamily` 那样。

<img src="/images/latex/font_size.png" alt="font_size" style="zoom:50%;" />

<img src="/images/latex/standard_font_size.png" alt="standard_font_size" style="zoom:50%;" />

另外还有一个基础命令 `\fontsize` 用于设定任意大小的字号：`\fontsize{⟨size⟩}{⟨base line-skip⟩}`，⟨size⟩ 为字号，⟨base line-skip⟩ 为基础行距，上图中的基础行距为 1.2。如果不是在导言区，`\fontsize` 的设定需要 `\selectfont` 命令才能立即生效，而上图的字号设定都是立即生效的。

**文字装饰和强调**

`\underline` 命令用来为文字添加下划线：

```latex
An \underline{underlined} text.
```

`\underline` 命令生成下划线的样式不够灵活，不同的单词可能生成高低各异的下划线，并且无法换行。ulem 宏包提供了更灵活的解决方案，它提供的 `\uline` 命令能够轻松生成自动换行的下划线：

```latex
An example of \uline{some
long and underlined words.}
```



### 二、段落格式和间距

**长度**

长度的数值 ⟨length⟩ 由数字和单位组成。常用的单位：

-   pt 点阵宽度，1/72.27in
-   bp 点阵宽度，1/72in
-   in 英寸
-   cm 厘米
-   mm 毫米
-   em 当前字号下大写字母 M 的宽度，常用于水平距离的设定
-   ex 当前字号下小写字母 x 的高度，常用于垂直距离的设定

一些情况下还会用到可伸缩的“弹性长度”，如 12pt plus 2pt minus 3pt 表示基础长度为 12pt，可以伸展到 14pt，也可以收缩到 9pt。也可只定义 plus 或者 minus 的部分，如 0pt plus 5pt。

长度的数值还可以用长度变量本身或其倍数来表达，如 2.5\parindent 等。

LaTex 预定义了大量的长度变量用于控制版面格式。如页面宽度和高度、首行缩进、段落间距等。如果需要自定义长度变量，需使用命令：`\newlength{\⟨length command⟩}`。

长度变量可以用 `\setlength` 赋值，或用 `\addtolength` 增加长度：

```latex
\setlength{\⟨length command⟩}{⟨length⟩}
\addtolength{\⟨length command⟩}{⟨length⟩}
```

**行距**

`\fontsize` 命令可以为字号设定对应的行距，但很少那么用。更常用的办法是在导言区使用 `\linespread` 命令：`\linespread{⟨factor⟩}`。

 ⟨factor⟩ 作用于基础行距而不是字号。缺省的基础行距是 1.2 倍字号大小，因此使用 `\linespread{1.5}` 意味着最终行距为 1.8 倍的字号大小。

如果不是在导言区全局修改，而想要局部地改变某个段落的行距，需要用 `\selectfont` 命令使 `\linespread` 命令的改动立即生效：

```latex
{\linespread{2.0}\selectfont
The baseline skip is set to be
twice the normal baseline skip.
Pay attention to the \verb|\par|
command at the end. \par}

In comparison, after the
curly brace has been closed,
everything is back to normal.
```

字号的改变是即时生效的，而行距的改变直到文字**分段**时才生效。如果需要改变某一部分文字的行距，那么不能简单地将文字包含在花括号内。注意下面两个例子中 `\par` 命令的位置，以及上一个例子的写法（\par 相当于分段)：

```latex
{\Large Don't read this!
 It is not true.
 You can believe me!\par}
```

```latex
{\Large This is not true either.
But remember I am a liar.}\par
```

**缩进**

段落的左缩进、右缩进和首行缩进，与设置行距的命令一样，在分段时生效。

```latex
 \setlength{\leftskip}{⟨length⟩}
 \setlength{\rightskip}{⟨length⟩}
 \setlength{\parindent}{⟨length⟩}
```

控制段落缩进的命令为：

```latex
\indent
\noindent
```

默认在段落开始时缩进，长度为用上述命令设置的 `\parindent`。如果需要在某一段不缩进，可在段落开头使用 `\noindent` 命令。相反地，`\indent` 命令强制开启一段首行缩进的段落。在段落开头使用多个 `\indent` 命令可以累加缩进量。

**水平间距**

LaTex 默认为将单词之间的“空格”转化为水平间距。如果需要在文中手动插入额外的水平间距，可使用 `\hspace` 命令：

```latex
This\hspace{1.5cm}is a space
of 1.5 cm.
```

`\hspace` 命令生成的水平间距如果位于一行的开头或末尾，则有可能因为断行而被舍弃。可使用 `\hspace*` 命令代替 `\hspace` 命令得到不会因断行而消失的水平间距。

命令 `\stretch{⟨n⟩}` 生成一个特殊弹性长度，参数 ⟨n⟩ 为权重。它的基础长度为 0pt，但可以无限延伸，直到占满可用的空间。如果同一行内出现多个 `\stretch{⟨n⟩}`，这一行的所有可用空间将按每个 `\stretch` 命令给定的权重 ⟨n⟩ 进行分配。

命令 `\fill` 相当于 `\stretch{1}`。

```latex
x\hspace{\stretch{1}}
x\hspace{\stretch{3}}
x\hspace{\fill}x
```

数学公式中遇到的 `\quad` 和 `\qquad` 命令，也可以用于文本中，分别相当于 `\hspace{1em}` 和 `\hspace{2em}`。

**垂直间距**

使用 `\vspace` 命令可以人为增加两个段落间的垂直距离：

```latex
A paragraph.

\vspace{2ex}
Another paragraph.
```

`\vspace` 命令生成的垂直间距在一页的顶端或底端可能被“吞掉”，类似 `\hspace` 在一行 开头和末尾那样。对应地，`\vspace*` 命令产生不会因断页而消失的垂直间距。`\vspace` 也可用 `\stretch` 设置无限延伸的垂直长度。



### 三、页面

控制页边距的参数由下图里给出的各种长度变量控制。可以用 `\setlength` 命令修改这些长度变量，以达到调节页面尺寸和边距的作用；反之也可以利用这些长度变量来决定排版内容的尺寸。

<img src="/images/latex/page_length.png" alt="page_length" style="zoom: 50%;" />

图中有些页边距的参数并不好设定，而 geometry 宏包提供了设置页边距等参数的简便方法。

geometry 宏包可以通过两种方式进行设定：

-   调用 geometry 宏包然后用其提供的 `\geometry` 命令设置页面参数

    ```latex
    \usepackage{geometry} 
    \geometry{⟨geometry-settings⟩}
    ```

-   将参数指定为宏包的选项

    ```latex
    \usepackage[⟨geometry-settings⟩]{geometry}
    ```

比如，符合 Microsoft Word 习惯的页面设定是 A4 纸张，上下边距 1 英寸，左右边距 1.25 英寸，可以通过如下两种等效的方式之一设定页边距：

```latex
\usepackage[left=1.25in,right=1.25in,top=1in,bottom=1in]{geometry}
% or like this:
\usepackage[hmargin=1.25in,vmargin=1in]{geometry}
```

又比如，需要设定周围的边距一致为 1.25 英寸，可以用更简单的语法： `\usepackage[margin=1.25in]{geometry}`。

对于书籍等双面文档，习惯上奇数页右边、偶数页左边留出较多的页边距，而书脊一侧的奇数页左边、偶数页右边页边距较少。可以这样设定:

```latex
\usepackage[inner=1in,outer=1.25in]{geometry}
```

geometry 宏包本身也能够修改纸张大小、页眉页脚高度、边注宽度等等参数。更详细的用法可以查阅 geometry 宏包的帮助文档。



### 四、页眉页脚

命令 `\pagestyle` 用来修改页眉页脚的样式：`\pagestyle{⟨page-style⟩}`。

命令 `\thispagestyle` 只影响当页的页眉页脚样式：`\thispagestyle{⟨page-style⟩}`。

⟨page-style⟩ 参数为样式的名称，预定义的有四类样式：

-   empty，页眉页脚为空
-   plain，页眉为空，页脚为页码。（article 和 report 文档类默认；book 文档类的每章第一页也为 plain 格式）
-   headings，页眉为章节标题和页码，页脚为空。（book 文档类默认）
-   myheadings，页眉为页码及 `\markboth` 和 `\markright` 命令手动指定的内容，页脚为空。

headings 样式：

-   article 文档类，twoside 选项偶数页为页码和节标题，奇数页为小节标题和页码
-   article 文档类，oneside 选项页眉为节标题和页码
-   book/report 文档类，twoside 选项偶数页为页码和章标题，奇数页为节标题和页码
-   book/report 文档类，oneside 选项页眉为章标题和页码

命令 `\pagenumbering` 用来改变页眉页脚中的页码样式：`\pagenumbering{⟨style⟩}`。

⟨style⟩ 为页码样式，默认为 arabic（阿拉伯数字），还可修改为 roman（小写罗马数字）、 Roman（大写罗马数字）等。注意使用 `\pagenumbering` 命令后会将页码重置为 1。

