---
title: LaTex 特色工具和功能
tags: latex
typora-root-url: ../../..
---

[toc]



BIBTEX 和 makeindex 依靠一些辅助程序自动生成参考文献、索引等；颜色、超链接等则可以生成美观易用的电子文档。



### 一、参考文献

**基本的参考文献和使用**

LaTeX 提供的参考文献和引用方式比较原始，需要用户自行书写参考文献列表（包括格式），较难直接使用。

`\cite` 命令用于在正文中引用参考文献：`\cite[additional content]{⟨citation⟩}`。

⟨citation⟩ 为引用的参考文献的标签，类似 `\ref` 里的参数；`\cite` 带一个可选参数，为引用的编号后加上额外的内容，如 `\cite[page 22]{Paper2013}`。

参考文献由 thebibliography 环境包裹。每条参考文献由 `\bibitem` 开头，其后是参考文献本身的内容：

```latex
\begin{thebibliography}{⟨widest label⟩}
  \bibitem[⟨item number⟩]{⟨citation⟩} ...
\end{thebibliography}
```

⟨citation⟩ 是 `\cite` 使用的文献标签，⟨item number⟩ 自定义参考文献的序号，如果省略则按自然排序给定序号。⟨widest label⟩ 用以限制参考文献序号的宽度，如 99 意味着不超过两位数字，通常设定为与参考文献的数目一致。

thebibliography 环境自动生成不带编号的一节（article 文档类）或一章（report / book 文档类）。在 article 文档类的节标题默认为 “Reference”，而在 report / book 文档类的章标题默认为 “Bibliography”。

```latex
\documentclass{article}
\begin{document}
\section{Introduction}
Partl~\cite{germenTeX} has proposed that \ldots
\begin{thebibliography}{99}
\bibitem{germenTeX} H.~Partl: \emph{German \TeX},
TUGboat Volume~9, Issue~1 (1988)
\end{thebibliography}
\end{document}
```

**BibTeX 数据库和样式**

BibTeX 是最流行的参考文献数据组织格式之一，使用 BibTeX 数据库可以让我们摆脱手写参考文献条目的麻烦。同时，通过参考文献样式的支持，还可以让同一份 BibTeX 数据库生成不同样式的参考文献列表。

BibTeX 数据库以 .bib 作为扩展名，其内容是若干个文献条目，每个条目的格式为：

```latex
@⟨type⟩{⟨citation⟩,
    ⟨key1⟩ = {⟨value1⟩},
    ⟨key2⟩ = {⟨value2⟩},
    ...
}
```

⟨type⟩ 为文献的类别，如 article 为学术论文，book 为书籍，incollection 为论文集中的某一篇，等等。⟨citation⟩ 为 `\cite` 命令使用的文献标签。在 ⟨citation⟩ 之后为条目里的各个字段，以 ⟨key⟩ = {⟨value⟩} 的形式组织。

简单列举学术论文里使用较多的 BibTeX 文献条目类别如下：

- article 学术论文，必需字段有 author, title, journal, year；可选字段包括 volume, number, pages, doi 等
- book 书籍，必需字段有 author/editor, title, publisher, year；可选字段包括 volume/number, series, address 等
- incollection 论文集中的一篇，必需字段有 author, title, booktitle, publisher, year；可选字段包括 editor, volume/number, chapter, pages, address 等
- inbook 书中的一章，必需字段有 author/editor, title, chapter/pages, publisher, year；可选字段包括 volume/number, series, address 等

例如 article 类别的参考文献数据条目写法如下：

```latex
@article{Alice13,
  title = {Demostration of bibliography items},
  author = {Alice Axford and Bob Birkin and Charlie Copper and Danny Dannford},
  year = {2013},
  month = {Mar},
  journal = {Journal of \TeX perts},
  volume = {36},
  number = {7},
  pages = {114-120}}
```

多数时候，我们无需自己手写 BibTeX 文献条目。从 Google Scholar 或者期刊/数据库的网站上都能够导出 BibTeX 文献条目。

参考文献的写法在不同文献里千差万别，包括作者、标题、年份等各项的顺序和字体样式、文献在列表中的排序规则等。BibTeX 用样式（style）来管理参考文献的写法。BibTeX 提供了几个预定义的样式，如 plain, unsrt, alpha 等。如果使用期刊模板的话，可能会提供自用的样式。样式文件以 .bst 为扩展名。

使用样式文件的方法是在源代码内（导言区）使用 `\bibliographystyle` 命令：`\bibliographystyle{⟨bst-name⟩}`。

 ⟨bst-name⟩ 为 .bst 样式文件的名称，不要带 .bst 扩展名。

**使用 BibTeX 排版参考文献**

1. 准备一份 BibTex 数据库，假设数据库文件名为 books.bib，和 LaTeX 源代码一般位于同一个目录下。

2. 在源代码中添加必要的命令，假设源代码名为 demo.tex。

   a. 首先，需要使用命令 `\bibliographystyle` 设定参考文献的格式

   b. 其次，使用命令 `\cite` 在正文中引用参考文献

   c. 再次，在需要列出参考文献的位置，使用 `\bibliography` 命令代替 thebibliography 环境

   ```latex
   % demo.tex
   \documentclass{article}
   \bibliographystyle{plain}
   \begin{document}
   \section{Some words}
   Some excellent books, for example, \cite{citation1}
   and \cite{citation2} \ldots
   \bibliography{books}
   \end{document}
   % 注意：\bibliographystyle 和\bibliography 命令缺一不可，否则生成参考文献列表的时候会报错。
   ```

3. 进行编译。

   a. 首先使用 pdflatex 或 xelatex 等命令编译 LaTeX 源代码 demo.tex

   b. 接下来用 bibtex 命令处理 demo.aux 辅助文件记录的参考文献格式、引用条目等信息。bibtex 命令处理完毕后会生成 demo.bbl 文件，内容就是一个 thebibliography 环境

   c. 再使用 pdflatex 或 xelatex 等命令把源代码 demo.tex 编译两遍，读入参考文献并正确生成引用

   

   整个过程使用的命令如下（可以略去扩展名）：
   xelatex demo
   bibtex demo
   xelatex demo
   xelatex demo

**natbib 宏包**

natbib 宏包在正文中支持以下两种人名——年份的引用方式：

```latex
\citep{⟨citation⟩}
\citet{⟨citation⟩}
```

分别生成形如 (Axford et al., 2013) 和 Axford et al. (2013) 的人名——年份引用。

正确排版人名——年份引用还依赖于特定的 BibTeX 样式。natbib 提供了与 LaTeX 预定义样式相对应的几个样式，包括 plainnat、abbrvnat 和 unsrtnat。学术论文模板是否支持 natbib，需要参考其帮助文档。

**biblatex 宏包**

biblatex 宏包是一套基于 LaTeX 宏命令的参考文献解决方案，提供了便捷的格式控制和强大的排序、分类、筛选、多文献表等功能。biblatex 宏包也因其对 UTF-8 和中文参考文献的良好支持，被国内较多 LaTeX 模板采用。

基于 biblatex 宏包的方式与基于 BibTeX 的传统方式有一定区别：

1. 文档结构和 biblatex 相关命令

   a. 在导言区调用 biblatex 宏包。宏包支持以 ⟨key⟩=⟨value⟩ 形式指定选项，包括参考文献样式 style、参考文献著录排序的规则 sorting 等

   b. 在导言区使用 `\addbibresource` 命令为 biblatex 引入参考文献数据库。与基于 BibTeX 的传统方式不同的是，biblatex 需要写完整的文件名

   c. 在正文中使用 `\cite` 命令引用参考文献。除此之外还可以使用丰富的命令达到不同的引用效果，如 `\citeauthor` 和 `\citeyear` 分别单独引用作者和年份，`\textcite` 和 `\parencite` 分别类似 natbib 宏包提供的 `\citet` 和 `\citep` 命令，以及脚注式引用 `\footcite` 等

   d. 在需要排版参考文献的位置使用命令 `\printbibliography`

2. 编译方式

   与基于 BibTeX 的传统方式不同的是，biblatex 宏包使用 biber 程序处理参考文献。因此之前文档的编译步骤变为：
   xelatex demo
   biber demo
   xelatex demo
   xelatex demo

   ```latex
   % File: egbibdata.bib
   @book{caimin2006,
   title = {UML 基础和 Rose 建模教程},
   address = {北京},
   author = {蔡敏 and 徐慧慧 and 黄柄强},
   publisher = {人民邮电出版社},
   year = {2006},
   month = {1}
   }
   
   % File: demo.tex
   \documentclass{ctexart}
   % 使用符合 GB/T 7714-2015 规范的参考文献样式
   \usepackage[style=gb7714-2015]{biblatex}
   % 注意加 .bib 扩展名
   \addbibresource{egbibdata.bib}
   \begin{document}
   见文献\cite{caimin2006}。
   \printbibliography
   \end{document}
   ```

3. 样式

   biblatex 使用的参考文献样式分为著录样式（bibliography style）和引用样式（citation style），分别以 .bbx 和 .cbx 为扩展名。参考文献的样式在调用宏包时使用 style 选项指定，或者使用 bibstyle 或 citestyle 分别指定：

   ```latex
   % 同时调用 gb7714-2015.bbx 和 gb7714-2015.cbx
   \usepackage[style=gb7714-2015]{biblatex}
   % 著录样式调用 gb7714-2015.bbx，引用样式调用 biblatex 宏包自带的 authoryear
   \usepackage[bibstyle=gb7714-2015,citestyle=authoryear]{biblatex}
   ```

   

### 二、索引和 makeindex 工具

书籍和大文档通常用索引来归纳关键词，方便用户查阅。LaTeX 借助配套的 makeindex 程序完成对索引的排版。

**使用 makeindex**

1. 在导言区调用 makeidx 宏包，并使用 `\makeindex` 命令开启索引的收集：

   ```latex
   \usepackage{makeidx}
   \makeindex
   ```

2. 在正文中需要索引的地方使用 `\index` 命令，并在需要输出索引的地方（如所有章节之后）使用 `\printindex` 命令

3. 编译

   a. 首先用 xelatex 等命令编译源代码，编译过程中产生索引记录文件 *.idx

   b. 用 makeindex 程序处理 *.idx，生成用于排版的索引列表文件 *.ind

   c. 再次编译源代码，正确生成索引列表

**索引项写法**

添加索引项的命令为：`\index{⟨index entry⟩}`。

⟨index entry⟩ 为索引项，写法由下表汇总。另外，!、@、| 为特殊符号，如果要向索引项直接输出这些符号，需要加前缀 "；而 " 需要输入两个引号 "" 才能输出到索引项。

| 举例                                                   | 索引项              | 备注                       |
| ------------------------------------------------------ | ------------------- | -------------------------- |
| 普通索引                                               |                     |                            |
| hello                                                  | hello, 1            | 普通索引                   |
|                                                        |                     |                            |
| 分级索引，以 ! 分隔，最多支持三级                      |                     |                            |
| hello                                                  | hello, 1            | 一级索引                   |
| hello!Peter                                            | Peter, 2            | 二级索引                   |
| hello!Peter!Jack                                       | Jack, 3             | 三级索引                   |
|                                                        |                     |                            |
| 格式化索引，形式为 ⟨alpha⟩@⟨format⟩                    |                     |                            |
| ⟨alpha⟩ 为纯字母，用来排序                             |                     |                            |
| ⟨format⟩ 为索引的格式，可以包括 LATEX 代码和简单的公式 |                     |                            |
| Mobius@M\\""obius                                      | Möbius, 2           | 输出重音                   |
| alpha@$\alpha$                                         | α, 7                | 输出公式                   |
| bold@\textbf{bold}                                     | bold, 12            | 输出粗体                   |
|                                                        |                     |                            |
| 页码范围                                               |                     |                            |
| morning\|(                                             | morning, 6–7        | 范围索引的开头             |
| morning\|)                                             |                     | 范围索引的结尾             |
|                                                        |                     |                            |
| 格式化索引页码                                         |                     |                            |
| Jenny\|textbf                                          | Jenny, 3            | 调用 \textbf 加粗页码      |
| Joe\|see{Jenny}                                        | Joe, see Jenny      | 调用 \see 生成特殊形式     |
| Joe\|seealso{Jenny}                                    | Joe, see also Jenny | 调用 \seealso 生成特殊形式 |



### 三、颜色

原始的 LaTeX 不支持使用各种颜色。color 宏包或者 xcolor 宏包提供了对颜色的支持，给 PDF 输出生成颜色的特殊指令。

**颜色表达式**

调用 color 或 xcolor 宏包后，通过以下两种方式切换颜色：

```latex
\color[⟨color-mode⟩]{⟨code⟩}
\color{⟨color-name⟩}
```

- 使用色彩模型和色彩代码，代码用 0 ∼1 的数字代表成分的比例。color 宏包支持 rgb、cmyk 和 gray 模型，xcolor 支持更多的模型如 hsb 等。

  ```latex
  \large\sffamily
  {\color[gray]{0.6}
  60\% 灰色} \\
  {\color[rgb]{0,1,1}
  青色}
  ```

- 直接使用名称代表颜色，前提是已经定义了颜色名称（没定义会报错）。

  ```latex
  \large\sffamily
  {\color{red} 红色} \\
  {\color{blue} 蓝色}
  ```

  color 宏包仅定义了 8 种颜色名称，xcolor 补充了一些，总共有 19 种。如果调用 color 或 xcolor 宏包时指定 dvipsnames 选项，就有额外的 68 种颜色名称可用。

  ![color_xcolor](/images/latex/color_xcolor.png)

xcolor 还支持将颜色通过表达式混合或互补：

```latex
\large\sffamily
{\color{red!40} 40\% 红色}\\
{\color{blue}蓝色
\color{blue!50!black}蓝黑
\color{black}黑色}\\
{\color{-red}红色的互补色}
```

或者通过命令自定义颜色名称：`\definecolor{⟨color-name⟩}{⟨color-mode⟩}{⟨code⟩}`。

**带颜色的文本和盒子**

输入带颜色的文本可以用类似 \textbf 的命令：

```latex
\textcolor[⟨color-mode⟩]{⟨code⟩}{⟨text⟩}
\textcolor{⟨color-name⟩}{⟨text⟩}
```

以下命令构造一个带背景色的盒子，⟨material⟩ 为盒子中的内容：

```latex
\colorbox[⟨color-mode⟩]{⟨code⟩}{⟨material⟩}
\colorbox{⟨color-name⟩}{⟨material⟩}
```

以下命令构造一个带背景色和有色边框的盒子，⟨fcode⟩ 或 ⟨fcolor-name⟩ 用于设置边框颜色：

```latex
\fcolorbox[⟨color-mode⟩]{⟨fcode⟩}{⟨code⟩}{⟨material⟩}
\fcolorbox{⟨fcolor-name⟩}{⟨color-name⟩}{⟨material⟩}
```

示例如下：

```latex
\sffamily
文字用\textcolor{red}{红色}强调\\
\colorbox[gray]{0.95}{浅灰色背景} \\
\fcolorbox{blue}{yellow}{
\textcolor{blue}{蓝色边框+文字，
黄色背景}
}
```



### 四、超链接

LaTeX 通过 hyperref 宏包实现超链接。

**hyperref 宏包**

hyperref 宏包涉及到的链接遍布 LaTeX 的每一个角落——目录、引用、脚注、索引、参考文献等等都被封装成超链接。这使得它与其它宏包发生冲突的可能性大大增加，虽然宏包已经尽力解决各方面的兼容性，但仍不能面面俱到。为减少可能的冲突，习惯上**将 hyperref 宏包放在其它宏包之后调用**。

hyperref 宏包提供了命令 `\hypersetup` 配置各种参数。参数也可以作为宏包选项，在调用宏包时指定：

```latex
\hypersetup{⟨option1⟩,⟨option2⟩={value},...}
\usepackage[⟨option1⟩,⟨option2⟩={value},...]{hyperref}
```

可选参数如下：

| 参数                            | 默认值 | 含义                                                         |
| ------------------------------- | ------ | ------------------------------------------------------------ |
| colorlinks=⟨true\|false⟩        | false  | 设置为 true 为链接文字带颜色，反之加上带颜色的边框           |
| hidelinks                       |        | 取消链接的颜色和边框                                         |
| pdfborder={⟨n⟩ ⟨n⟩ ⟨n⟩}         | 0 0 1  | 超链接边框设置，设为 0 0 0 可取消边框                        |
|                                 |        |                                                              |
| bookmarks=⟨true\|false⟩         | true   | 是否生成书签                                                 |
| bookmarksopen=⟨true\|false⟩     | false  | 是否展开书签                                                 |
| bookmarksnumbered=⟨true\|false⟩ | false  | 书签是否带章节编号                                           |
|                                 |        |                                                              |
| pdftitle=⟨string⟩               | 空     | 标题                                                         |
| pdfauthor=⟨string⟩              | 空     | 作者                                                         |
| pdfsubject=⟨string⟩             | 空     | 主题                                                         |
| pdfkeywords=⟨string⟩            | 空     | 关键词                                                       |
| pdfstartview=⟨Fit\|FitH\|FitV⟩  | Fit    | 设置 PDF 页面以适合页面/适合宽度/适合高度等方式显示，默认为适合页面 |

**超链接**

hyperref 宏包提供了直接书写超链接的命令：

```latex
\url{⟨url⟩}
\nolinkurl{⟨url⟩}
\href{⟨url⟩}{⟨text⟩}
```

`\url` 和 `\nolinkurl` 都生成可以点击的 URL，区别是前者有彩色，后者没有。在 `\url` 命令中作为参数的 URL 里，可直接输入如 %、& 这样的特殊符号。

```latex
\url{http://wikipedia.org} \\
\nolinkurl{http://wikipedia.org} \\
\href{http://wikipedia.org}{Wiki}
```

用户也可对某个 `\label` 命令定义的标签 ⟨label⟩ 作超链接（注意这里的 ⟨label⟩ 虽然是可选参数的形式，但通常是必填的）：`\hyperref[⟨label⟩]{⟨text⟩}`。

默认的超链接在文字外边加上一个带颜色的边框（在打印 PDF 时边框不会打印），可指定 colorlinks 参数修改为将文字本身加上颜色，或修改 pdfborder 参数调整边框宽度以“去掉”边框；hidelinks 参数则令超链接既不变色也不加边框。

```latex
\hypersetup{colorlinks=true}
% or:
\hypersetup{pdfborder={0 0 0}}
% or:
\hypersetup{hidelinks}
```

**PDF 书签**

hyperref 宏包另一个强大的功能是为 PDF 生成书签。对于章节命令 `\chapter`、`\section` 等，默认情况下会为 PDF 自动生成书签。和交叉引用、索引等类似，生成书签也需要多次编译源代码，第一次编译将书签记录写入 .out 文件，第二次编译才正确生成书签。

