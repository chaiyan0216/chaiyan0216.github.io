---
title: LaTeX 自定义命令
tags: latex
typora-root-url: ../../..
---

[toc]



本章介绍如何制作一个简单但像样的毕业论文/书籍/简历模板，每次可以直接套用，而不是再在导言区写一堆代码。



### 一、自定义命令和环境

**定义新命令**

自定义命令：`\newcommand{\⟨name⟩}[⟨num⟩]{⟨definition⟩}`。

⟨name⟩ 是要定义的命令名称（带反斜线），⟨definition⟩ 是命令的具体定义。参数 ⟨num⟩ 是可选的，用于指定新命令所需的参数数目（最多 9 个，默认为 0）。

不带参数的示例：

```latex
\newcommand{\tnss}{The not so Short Introduction to \LaTeXe}
% in document:
This is ``\tnss'' \ldots{} ``\tnss''
```

带参数的示例：

```latex
\newcommand{\txsit}[1]
{This is the \emph{#1} Short Introduction to \LaTeXe}
% in document:
\begin{itemize}
\item \txsit{not so}
\item \txsit{very}
\end{itemize}
```

LaTeX 不允许使用 `\newcommand` 定义一个与现有命令重名的命令，如有需要可以使用以下方式：

- `\renewcommand`，修改已有命令定义
- `\providecommand`，命令末定义时相当于 `newcommand`，命令已定义时沿用已有的定义

**定义新环境**

自定义环境：`\newenvironment{⟨name⟩}[⟨num⟩]{⟨before⟩}{⟨after⟩}`。

⟨name⟩ 是要定义的环境名称，⟨before⟩ 中的内容将在此环境包含的文本之前处理，⟨after⟩ 中的内容将在遇到 `\end{⟨name⟩}` 命令时处理。参数 ⟨num⟩ 是可选的，用法与 `\newcommand` 一样。

```latex
\newenvironment{king}
{\rule{1ex}{1ex} \hspace{\stretch{1}}}
{\hspace{\stretch{1}} \rule{1ex}{1ex}}

\begin{king}
My humble subjects \ldots
\end{king}
```

如果需要改变一个现有环境，可以使用 `\renewenvironment`，语法与 `\newenvironment` 一致。



### 二、自定义宏包和文档类

**简单宏包**

当有很多自定义命令和环境时，文档的导言区将变得很长，这时可以建立一个新的 LaTeX 宏包来存放所有自定义的命令和环境，然后在文档中使用
`\usepackage` 命令来调用自定义的宏包。

```latex
% Demo Package
\ProvidesPackage{demopack}
\newcommand{\tnss}{The not so Short Introduction to \LaTeXe}
\newcommand{\txsit}[1]{The \emph{#1} Short Introduction to \LaTeXe}
\newenvironment{king}{\begin{quote}}{\end{quote}}
```

宏包可以理解为：以 .sty 为后缀的文本文件，其内容包含了宏包专用命令 `\ProvidesPackage{⟨package name⟩}` 和 自定义命令、环境。

需要注意的是：⟨package name⟩ 要和宏包的文件名一致。

如果想进一步把各种宏包的功能汇总到一个文件里，而不是在文档的导言区罗列一大堆宏包，可以使用命令 `\RequirePackage`，用法与
`\usepackage` 一致：`\RequirePackage[⟨options⟩]{⟨package name⟩}`。

**文档类**

编写自定义文档类，如论文模板等会稍麻烦一些。

- 首先，文档类应以 .cls 作扩展名，开头使用 `\ProvidesClass` 命令：`\ProvidesClass{⟨class name⟩}`，{⟨class name⟩} 需要和文档类的文件名一致
- 其次，需要通过 `\LoadClass` 命令调用一个基本文当类：`\LoadClass[⟨options⟩]{⟨package name⟩}`
- 或者，不调用基本文档类，而是自己实现诸如 `\chapter`、`\section` 等命令



### 三、计数器

LaTeX 对文档元素自动计数的能力：章节符号、列表、图表，都是依靠计数器功能完成的。

**定义和修改计数器**

定义一个计数器的方法：`\newcounter{⟨counter name⟩}[⟨parent counter name⟩]`。

⟨counter name⟩ 为计数器的名称。计数器可以有上下级的关系，可选参数 ⟨parent countername⟩ 定义为 ⟨counter name⟩ 的上级计数器。

以下命令修改计数器的数值：

- `\setcounter` 将数值设为 ⟨number⟩
- `\addtocounter` 将数值加上 ⟨number⟩
- `\stepcounter` 将数值加 1，并将所有下级计数器归零

```latex
\setcounter{⟨counter name⟩}{⟨number⟩}
\addtocounter{⟨counter name⟩}{⟨number⟩}
\stepcounter{⟨counter name⟩}
```

**输出格式**

计数器 ⟨counter⟩ 的输出格式由 `\the⟨counter⟩` 表示，如 `\thechapter`。计数器值默认以阿拉伯数字形式输出，如果想改成其它形式，需要重定义 `\the⟨counter⟩`，如将 equation 计数器的格式定义为大写字母：

```latex
\renewcommand\theequation{\Alph{equation}}
```

命令 `\Alph` 控制计数器 ⟨counter⟩ 的值以大写字母形式显示。下表列出所有可用于修改计数器格式的命令。注意：这些命令只能用于计数器，不能直接用于数字，如 `\roman{1}` 这样的命令会出错。

| 命令      | 样式                                      | 范围       |
| --------- | ----------------------------------------- | ---------- |
| \arabic   | 阿拉伯数字（默认）                        |            |
| \alph     | 小写字母                                  | 限 0–26    |
| \Alph     | 大写字母                                  | 限 0–26    |
| \roman    | 小写罗马数字                              | 限非负整数 |
| \Roman    | 大写罗马数字                              | 限非负整数 |
| \fnsymbol | 一系列符号，用于 `\thanks` 命令生成的脚注 | 限 0–9     |

注：`\fnsymbol` 使用的符号顺次为：∗ † ‡ § ¶ ‖ ∗∗ †† ‡‡

计数器可以组合，如标准文档类里对 `\subsection` 相关的计数器的输出格式的定义相当于：

```latex
\renewcommand\thesubsection{\thesection.\arabic{subsection}}
```

**已有计数器**

- 所有章节命令 `\chapter`、`\section` 等分别对应计数器 chapter、section 等等，且有上下级的关系。part 为独立计数器。
- 有序列表 enumerate 的各级计数器为 enumi, enumii, enumiii, enumiv，也有上下级的关系。
- 图表浮动体的计数器就是 table 和 figure；公式的计数器为 equation。这些计数器在 article 文档类中是独立的，而在 book 和 report 中以 chapter 为上级计数器。
- 页码、脚注的计数器分别是 page 和 footnote。

示例，把页码修改成大写罗马字母，左右加横线，或是给脚注加上方括号：

```latex
\renewcommand\thepage{--~\Roman{page}~--}
\renewcommand\thefootnote{[\arabic{footnote}]}
```

两个跟章节编号有关的计数器：

- secnumdepth

  标准文档类对章节划分了层级：

  - 在 article 文档类里 part 为 0，section 为 1，依此类推；
  - 在 report/book 文档类里 part 为 -1，chapter 为 0，section 为 1，等等。

  secnumdepth 计数器控制章节编号的深度，如果章节的层级大于secnumdepth，那么章节的标题在目录和页眉页脚的标题都不编号（照常生成目录和页眉页脚），章节计数器也不计数。

  可以用 `\setcounter` 命令设置 secnumdepth 为较大的数使得层级比较深的章节也编号，如设置为 4 令 `\paragraph` 也编号；或者设置一个较小的数以取消编号，如设置为 -1 令 `\chapter` 不编号，免去了手动使用 `\addcontentsline` 和 `\mark-both` 的麻烦。

  secnumdepth 计数器在 article 文档类里默认为 3（subsubsection 一级）；在 report 和 book 文档类里默认为 2（subsection 一级）。

- tocdepth

  tocdepth 计数器控制目录的深度，如果章节的层级大于 tocdepth，那么章节将不会自动写入目录项。默认值同 secnumdepth。



### 四、可定制项

LaTeX 有一些比较容易定制的内容：标题名称、前后辍，长度。

- 标题名称/前后缀等，可以用 `\renewcommand` 来修改

  | 命令 | 默认值 | 含义 |
  | --- | --- | --- |
  | \partname | Part | \part 命令生成的标题前缀 |
  | \chaptername | Chapter | \chapter 命令生成的标题前缀 |
  | \appendixname | Appendix | 使用 \appendix 命令生成的附录部分的章标题前缀 |
  | \abstractname | Abstract | 摘要环境 abstract 的标题名称 |
  | \contentsname | Contents | \tableofcontents 命令生成的目录标题 |
  | \listfigurename | List of Figures | \listoffigures 命令生成的插图目录标题 |
  | \listtablename | List of Tables | \listoftables 命令生成的表格目录标题 |
  | \tablename | Table | table 浮动体中 \caption 命令生成的标题前缀 |
  | \figurename | Figure | figure 浮动体中 \caption 命令生成的标题前缀 |
  | \refname | References | thebibliography 环境或 \bibliography 命令生成的参考文献标题（article 文档类） |
  | \bibname | Bibliography | thebibliography 环境或 \bibliography 命令生成的参考文献标题（book / report 文档类） |
  | \indexname | Index | \printindex 命令生成的索引标题 |
  
- 长度，可用 `\setlength` 来修改

  | 命令 | 默认值 | 含义 |
  | --- | --- | --- |
  | \fboxrule | 0.4pt | \fbox 或 \framebox 等带框盒子的线宽 |
  | \fboxsep | 3pt | \fbox 或 \framebox 等带框盒子的内边距 |
  | \arraycolsep | 5pt array | 环境的表格项前后的间距 |
  | \tabcolsep | 6pt tabular | 环境的表格项前后的间距 |
  | \arrayrulewidth | 0.4pt | 表格线宽 |
  | \doublerulesep | 2pt | 连续两根表格线之间的间距 |
  | \abovecaptionskip | 10pt | \caption 命令位于图表下方时，与上方图表的间距 |
  | \belowcaptionskip | 0pt | \caption 命令位于图表上方时，与下方图表的间距 |
  | \columnsep | 10pt | 双栏排版下两栏的间距 |
  | \columnseprule | 0pt | 双栏排版下两栏之间竖线的宽度 |

