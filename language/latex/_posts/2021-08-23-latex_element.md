---
title: LaTex 文档元素
tags: latex
typora-root-url: ../../..
---

[toc]



本章介绍一个结构化的文档所依赖的各种元素：章节、目录、列表、图表、交叉引用、脚注等等。



### 一、章节和目录

**章节标题**

三个标准文档类（article、book、report）的章节划分命令：

```latex
\chapter{⟨title⟩} % 只在 book 和 report 中有
\section{⟨title⟩}
\subsection{⟨title⟩}
\subsubsection{⟨title⟩}
\paragraph{⟨title⟩}
\subparagraph{⟨title⟩}
```

以上命令除了生成带编号的标题之外，还会向目录中添加条目。同时，每条命令有两个变体：

- 带可选参数的变体：`\section[⟨short title⟩]{⟨title⟩}`

  ​	标题使用 ⟨title⟩ 参数，在目录和页眉页脚中使用 ⟨short title⟩ 参数

- 带星号的变体：`\section*{⟨title⟩}`

  ​	标题不带编号，也不生成目录项和页眉页脚

较低层次如 `\paragraph` 和 `\subparagraph` 即使不用带星号的变体，生成的标题默认也不带编号。因此，实际带编号的层级为：

- article：`\section`，`\subsection`，`\subsubsection`
- report/book：`\chapter`，`\section`，`\subsection`

**目录**

生成目录非常容易，只需在合适的地方使用命令：`\tableofcontents`，但正确生成目录项，一般需要**编译两次**源代码。

目录会被生成为单独的一章（book / report）或一节（article），标题默认为“Contents”。

当使用了 `\chapter`* 或 `\section*` 这样不生成目录项的章节标题命令，而又想手动生成该章节的目录项时，可以在标题命令后面使用：`\addcontentsline{toc}{⟨level⟩}{⟨title⟩}`，其中 ⟨level⟩ 为章节层次 chapter 或 section 等，⟨title⟩ 为出现于目录项的章节标题。（这个操作怎么感觉多此一举呢，为什么不自动生成目录项）

**文档结构划分**

标准文档类提供了一个 `\appendix` 命令将正文和附录分开，使用该命令后，最高一级章节改为使用拉丁字母编号，从 A 开始。

book 文档类还提供了前言、正文、后记结构的划分命令：

- `\frontmatter` 前言部分：页码为小写罗马字母格式；其后的 \chapter 不编号。

- `\mainmatter` 正文部分：页码为阿拉伯数字格式，从 1 开始计数；其后的章节编号正常。

- `\backmatter` 后记部分：页码格式不变，继续正常计数；其后的 \chapter 不编号。

以上三个命令还可和 `\appendix` 命令结合，生成有前言、正文、附录、后记四部分的文档，下面是个示例。

```latex
\documentclass{book}

% 导言区，加载宏包和各项设置，包括参考文献、索引等
\usepackage{makeidx} % 调用 makeidx 宏包，用来处理索引
\makeindex % 开启索引的收集
\bibliographystyle{plain} % 指定参考文献样式为 plain

\begin{document}

\frontmatter % 前言部分
\maketitle % 标题页
\include{preface} % 前言章节 preface.tex
\tableofcontents

\mainmatter % 正文部分
\include{chapter1} % 第一章 chapter1.tex
\include{chapter2} % 第二章 chapter2.tex
...
\appendix % 附录
\include{appendixA} % 附录A appendixA.tex
...

\backmatter % 后记部分
\include{prologue} % 后记 prologue.tex
\bibliography{books} % 利用 BibTeX 工具从数据库文件 books.bib 生成参考文献
\printindex % 利用 makeindex 工具生成索引

\end{document}
```



### 二、标题页

LaTex 支持生成简单的标题页，在导言区增加命令：`\title{⟨title⟩} \author{⟨author⟩} \date{⟨date⟩}`，之后就可以使用 `\maketitle` 命令生成一个简单的标题页了。

LaTex 提供了一个 `\today` 命令自动生成当前日期，`\date` 默认使用 `\today`。

```latex
\title{Test title}
\author{ Mary\thanks{E-mail:*****@***.com}
\and Ted\thanks{Corresponding author}
\and Louis}
\date{\today}
\begin{document}
\maketitle % 标题页
\end{document}
```

article 文档类的标题默认不单独成页，而 report 和 book 默认单独成页。可在 `\documentclass` 命令调用文档类时指定 titlepage / notitlepage 选项以修改默认的行为。



### 三、交叉引用

交叉引用是 LaTex 强大的自动排版功能的体现之一。在能够被交叉引用的地方，如章节、公式、图表、定理等位置使用命令：`\label{⟨label-name⟩}`，之后可以在需要引用处使用 `\ref` 或 `\pageref` 命令，分别生成交叉引用的编号和页码：`\ref{⟨label-name⟩} \pageref{⟨label-name⟩}`。

```latex
A reference to this subsection
\label{sec:this} looks like:
``see section~\ref{sec:this} on
page~\pageref{sec:this}.''
```

为了生成正确的交叉引用，一般也需要多次编译源代码。

`\label` 命令可用于记录各种类型的交叉引用，使用位置分别为：

- 章节标题：在章节标题命令 `\section` 等之后紧接着使用。
- 行间公式：单行公式在公式内任意位置使用；多行公式在每一行公式的任意位置使用。
- 有序列表：在 enumerate 环境的每个 `\item` 命令之后、下一个 `\item` 命令之前任意位置使用。
- 图表标题：在图表标题命令 `\caption` 之后紧接着使用。
- 定理环境：在定理环境内部任意位置使用。

在使用不记编号的命令形式（`\section\*`、`\caption\*`、带可选参数的 `\item` 命令等）时不要使用 `\label` 命令，否则生成的引用编号不正确。



### 四、脚注和边注

使用 `\footnote` 命令可以在页面底部生成一个脚注：`\footnote{⟨footnote⟩}`。

```latex
“天地玄黄，宇宙洪荒。日月盈昃，辰宿列张。”\footnote{出自《千字文》。}
```

表格内使用 `\footnote` 并不能正确生成脚注。需要分两步进行，先使用 `\footnotemark` 为脚注计数，再在合适的位置用 `\footnotetext` 生成脚注。

```latex
\begin{tabular}{l}
\hline
“天地玄黄，宇宙洪荒。日月盈昃，辰宿列张。”\footnotemark \\
\hline
\end{tabular}
\footnotetext{表格里的名句出自《千字文》。}
```

使用 `\marginpar` 命令可在边栏位置生成边注：`\marginpar[⟨left-margin⟩]{⟨right-margin⟩}`。如果只给定了 ⟨right-margin⟩，那么边注在奇偶数页文字相同；如果同时给定了 ⟨left-margin⟩，则偶数页使用 ⟨left-margin⟩ 的文字。

```latex
\marginpar{\footnotesize 边注较窄，不要写过多文字，最好设置较小的字号。}
```



### 五、特殊环境

**列表**

有序和无序列表环境 enumerate 和 itemize，两者的用法很类似，都用 `\item` 标明每个列表项。

```latex
% 有序列表
\begin{enumerate}
\item ...
\end{enumerate}

\begin{enumerate}
  \item An item.
    \begin{enumerate}
      \item A nested item.\label{itref}
      \item[*] A starred item.
    \end{enumerate}
  \item Reference(\ref{itref}).
\end{enumerate}

% 无序列表
\begin{itemize}
\item ...
\end{itemize}

\begin{itemize}
  \item An item.
    \begin{itemize}
      \item A nested item.
      \item[+] A `plus' item.
      \item Another item.
    \end{itemize}
  \item Go back to upper level.
\end{itemize}
```

关键字环境 description 的用法与以上两者类似，不同的是 `\item` 后的可选参数用来写关键字，以粗体显示，一般是必填的：

```latex
\begin{description}
\item[⟨item title⟩] ...
\end{description}

\begin{description}
  \item[Enumerate] Numbered list.
  \item[Itemize] Non-numbered list.
\end{description}
```

- 各级有序列表的符号由命令 `\labelenumi` 到 `\labelenumiv` 定义，重新定义这些命令需要用到后续计数器相关命令。

- 各级无序列表的符号由命令 `\labelitemi` 到 `\labelitemiv` 定义，可以简单地重新定义它们。

```latex
\renewcommand{\labelenumi}%
{\Alph{enumi}>}
\begin{enumerate}
  \item First item
  \item Second item
\end{enumerate}

\renewcommand{\labelitemi}{\ddag}
\renewcommand{\labelitemii}{\dag}
\begin{itemize}
  \item First item
  \begin{itemize}
    \item Subitem
    \item Subitem
  \end{itemize}
  \item Second item
\end{itemize}
```

**对齐**

center、flushleft 和 flushright 环境分别用于生成居中、左对齐和右对齐的文本环境。

```latex
\begin{center} ... \end{center}
\begin{flushleft} ... \end{flushleft}
\begin{flushright} ... \end{flushright}

\begin{center}
Centered text using a
\verb|center| environment.
\end{center}
\begin{flushleft}
Left-aligned text using a
\verb|flushleft| environment.
\end{flushleft}
\begin{flushright}
Right-aligned text using a
\verb|flushright| environment.
\end{flushright}
```

除此之外，还可以用以下命令直接改变文字的对齐方式：`\centering \raggedright \raggedleft`。

```latex
\centering
Centered text paragraph.

\raggedright
Left-aligned text paragraph.

\raggedleft
Right-aligned text paragraph.
```

三个命令和对应的环境经常被误用，有一点可以将两者区分开来：center 等环境会在上下文产生一个额外间距，而 `\centering` 等命令不产生，只是改变对齐方式。比如在浮动体环境 table 或 figure 内实现居中对齐，用 `\centering` 命令即可，没必要再用 center 环境。

**引用**

引用环境较一般文字有额外的左右缩进。

quote 用于引用较短的文字，首行不缩进；quotation 用于引用若干段文字，首行缩进。

```latex
Francis Bacon says:
\begin{quote}
Knowledge is power.
\end{quote}

《木兰诗》:
\begin{quotation}
万里赴戎机，关山度若飞。
朔气传金柝，寒光照铁衣。
将军百战死，壮士十年归。

归来见天子，天子坐明堂。
策勋十二转，赏赐百千强。⋯⋯ 
\end{quotation}
```

verse 用于排版诗歌，与 quotation 恰好相反，verse 是首行悬挂缩进的。

```latex
Rabindranath Tagore's short poem:
\begin{verse}
Beauty is truth's smile
when she beholds her own face in
a perfect mirror.
\end{verse}
```

**摘要**

摘要环境 abstract 默认只在标准文档类中的 article 和 report 文档类可用，一般用于紧跟 `\maketitle` 命令之后介绍文档的摘要。如果文档类指定了 titlepage 选项，则单独成页。

**代码**

将一段代码原样转义输出，可以用代码环境 verbatim，它以等宽字体排版代码，回车和空格也分别起到换行和空位的作用；带星号的版本更进一步将空格显示成 “␣”。

```latex
\begin{verbatim}
#include <iostream>
int main()
{
  std::cout << "Hello, world!"
            << std::endl;
return 0; 
}
\end{verbatim}

\begin{verbatim*}
for (int i=0; i<4; ++i)
  printf("Number %d\n",i);
\end{verbatim*}
```

要排版简短的代码或关键字，可使用 `\verb` 命令：`\verb⟨delim⟩⟨code⟩⟨delim⟩`。⟨delim⟩ 标明代码的分界位置，前后必须一致，除字母、空格或星号外，可任意选择使得不与代码本身冲突，习惯上使用 | 符号。`\verb` 后也可以带一个星号，以显示空格：

```latex
\verb|\LaTeX| \\
\verb+(a || b)+ \verb*+(a || b)+
```



### 六、表格

排版表格最基本的 tabular 环境用法为：

```latex
\begin{tabular}[⟨align⟩]{⟨column-spec⟩} 
⟨item1⟩ & ⟨item2⟩ & ... \\
\hline
⟨item1⟩ & ⟨item2⟩ & ... \\
\end{tabular}
```

其中 ⟨column-spec⟩ 是列格式标记，在接下来的内容将仔细介绍；& 用来分隔单元格；\\\\ 用来换行；`\hline` 用来在行与行之间绘制横线。

通常情况下 tabular 环境很少与文字直接混排，而是会放在 table 浮动体环境中，并 用 `\caption` 命令加标题。

**列格式**

| 列格式      | 说明                                 |
| ----------- | ------------------------------------ |
| l/c/r       | 单元格内容左对齐/居中/右对齐，不折行 |
| p{⟨width⟩}  | 单元格宽度固定为 ⟨width⟩，可自动折行 |
| \|          | 绘制竖线                             |
| @{⟨string⟩} | 自定义内容 ⟨string⟩                  |

```latex
\begin{tabular}{lcr|p{6em}}
  \hline
  left & center & right
       & par box with fixed width \\
  L    & C      & R     & P \\ 
  \hline
\end{tabular}
```

表格中每行的单元格数目不能多于列格式里 l/c/r/p 的总数（可以少于这个总数），否则出错。

@ 格式可在单元格前后插入任意的文本，但同时它也消除了单元格前后额外添加的间距。@ 格式可以适当使用以充当“竖线”。特别地，@{} 可直接用来消除单元格前后的间距：

```latex
\begin{tabular}{@{} r@{:}lr @{}}
  \hline
  1  & 1 & one \\
  11 & 3 & eleven \\
  \hline
\end{tabular}
```

LaTex 还提供了简便的将格式参数重复的写法 *{⟨n⟩}{⟨column-spec⟩}，比如以下两种写法是等效的:

```latex
\begin{tabular}{|c|c|c|c|c|p{4em}|p{4em}|}
\begin{tabular}{|*{5}{c|}*{2}{p{4em}|}}
```

**横线**

`\hline` 用于绘制横线，`\cline{⟨i⟩-⟨j⟩}` 用来绘制跨越部分横线单元格的横线。

```latex
\begin{tabular}{|c|c|c|}
  \hline
  4 & 9 & 2 \\ \cline{2-3}
  3 & 5 & 7 \\ \cline{1-1}
  8 & 1 & 6 \\
  \hline
\end{tabular}
```

在科技论文排版中广泛应用的表格形式是三线表，形式干净简明。三线表由 booktabs 宏包支持，它提供了 `\toprule`、`\midrule` 和 `\bottomrule` 命令用以排版三线表的三条线，以及和 `\cline` 对应的 `\cmidrule`。除此之外，最好不要用其它横线以及竖线：

```latex
\begin{tabular}{cccc}
  \toprule
   & \multicolumn{3}{c}{Numbers} \\
  \cmidrule{2-4}
           & 1 & 2 & 3 \\
  \midrule
  Alphabet & A & B & C \\
  Roman    & I & II& III \\
  \bottomrule
\end{tabular}
```

**合并单元格**

LaTex 是一行一行排版表格的，横向合并单元格较为容易，由 `\multicolumn` 命令实现：`\multicolumn{⟨n⟩}{⟨column-spec⟩}{⟨item⟩}`。

其中 ⟨n⟩ 为要合并的列数，⟨column-spec⟩ 为合并单元格后的列格式，只允许出现一个 l/c/r 或 p 格式。如果合并前的单元格前后带表格线 |，合并后的列格式也要带 | 以使得表格的竖线一致。

```latex
\begin{tabular}{|c|c|c|}
  \hline
  1 & 2 & Center \\
  \hline
  \multicolumn{2}{|c|}{3} & \multicolumn{1}{r|}{Right} \\
  \hline
  4 & \multicolumn{2}{c|}{C} \\
  \hline
\end{tabular}
```

上面的例子还体现了，形如 `\multicolumn{1}{⟨column-spec⟩}{⟨item⟩}` 的命令可以用来修改某一个单元格的列格式。

纵向合并单元格需要用到 multirow 宏包提供的 `\multirow` 命令：`\multirow{⟨n⟩}{⟨width⟩}{⟨item⟩}`，⟨width⟩ 为合并后单元格的宽度，可以填 * 以使用自然宽度。下面是一个结合 `\cline`、`\multicolumn` 和 `\multirow` 的例子：

```latex
\begin{tabular}{ccc}
  \hline
  \multirow{2}{*}{Item} & \multicolumn{2}{c}{Value} \\ \cline{2-3}
    & First & Second \\ \hline
  A & 1     & 2 \\ \hline
\end{tabular}
```

**行距控制**

原生的表格看起来通常比较紧凑。修改参数 `\arraystretch` 可以得到行距更加宽松的表格：

```latex
\renewcommand\arraystretch{1.8}
\begin{tabular}{|c|}
  \hline
  Really loose \\ \hline
  tabular rows.\\ \hline
\end{tabular}
```

另一种增加间距的办法是给换行命令 \\ 添加可选参数，在这一行下面加额外的间距：

```latex
\begin{tabular}{c}
  \hline
  Head lines \\[6pt]
  tabular lines \\
  tabular lines \\ \hline
\end{tabular}
```

但是这种换行方式的存在导致了一个缺陷：表格的首个单元格不能直接使用中括号 []， 否则 \\ 往往会将下一行的中括号当作自己的可选参数，因而出错。如果要使用中括号，应当放在花括号 {} 里面。或者也可以选择将换行命令写成 `\[0pt]`。



### 七、图片

LaTex 本身不支持插图功能，需要由 graphicx 宏包辅助支持。

使用 latex + dvipdfmx 编译命令时，调用 graphicx 宏包时要指定 dvipdfmx 选项；而使用 pdflatex 或 xelatex 命令编译时不需要。

各编译方式支持的主流图片格式：

| 格式             | 矢量图    | 位图           |
| ---------------- | --------- | -------------- |
| latex + dvipdfmx | .eps      | n/a            |
| pdflatex         | .pdf      | .jpg .png      |
| xelatex          | .pdf .eps | .jpg .png .bmp |

在调用了 graphicx 宏包以后，就可以使用 `\includegraphics` 命令加载图片了：`\includegraphics[⟨options⟩]{⟨filename⟩}`，其中 ⟨filename⟩ 为图片文件名，与 `\include` 命令的用法类似，文件名可能需要用相对路径或绝对路径表示。图片文件的扩展名一般可不写。

另外 graphicx 宏包还提供了 `\graphicspath` 命令，用于声明一个或多个图片文件存放的目录，使用这些目录里的图片时可不用写路径：

```latex
% 假设主要的图片放在 pics 子目录下，标志放在 logo 子目录下
\graphicspath{ {pics/} {logo/} }
```

`\includegraphics` 命令的可选参数 ⟨options⟩ 支持 ⟨key⟩=⟨value⟩ 形式赋值，常用的参数如下：

| 参数            | 含义                              |
| --------------- | --------------------------------- |
| width=⟨width⟩   | 将图片缩放到宽度为 ⟨width⟩        |
| height=⟨height⟩ | 将图片缩放到高度为 ⟨height⟩       |
| scale=⟨scale⟩   | 将图片相对于原尺寸缩放 ⟨scale⟩ 倍 |
| angle=⟨angle⟩   | 令图片逆时针旋转 ⟨angle⟩ 度       |



### 八、盒子

**标尺盒子**

`\rule` 命令用来画一个实心的矩形盒子，也可适当调整以用来画（(标尺）：`\rule[⟨raise⟩]{⟨width⟩}{⟨height⟩}`。

```latex
Black \rule{12pt}{4pt} box.

Upper \rule[4pt]{6pt}{8pt} and
lower \rule[-4pt]{6pt}{8pt} box.

A \rule[-.4pt]{3em}{.4pt} line.
```

