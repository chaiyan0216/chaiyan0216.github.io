---
title: LaTex 符号
tags: latex
typora-root-url: ../../..
---

[toc]



本章介绍如何在 LaTex 中输入各种文字符号，包括标点符号等，以及控制文字断行和断页的方式。



**编码**

Unicode 是一个多国字符的集合，覆盖了几乎全球范围内的语言文字。

UTF-8 是 Unicode 的一套编码方案，一个字符由一个到四个字节编码，其中单字节字符的编码与 ASCII 编码兼容。

较为现代的 TEX 引擎，如 XƎTEX 和 LuaTEX，它们均原生支持 UTF-8 编码。使用 xelatex 和 lualatex 排版时，将源代码保存为 UTF-8 编码，并借助 fontspec 宏包调用适当的字体，原则上就可以在源代码中输入任意语言的文字。

**中文**

CJK 宏包对中文字体的支持比较麻烦，已经不再推荐直接使用。

XƎTEX 和 LuaTEX 除了直接支持 UTF-8 编码外，还支持直接调用 TrueType / OpenType 格式的字体。

xeCJK 及 luatexja 宏包则在此基础上封装了对汉字排版细节的处理功能。

ctex 宏包和文档类进一步封装了 CJK、xeCJK、luatexja 等宏包，使得用户在排版中文时不用再考虑排版引擎等细节。ctex 宏包本身用于配合各种文档类排版中文，而 ctex 文档类对 LaTex 的标准文档类进行了封装，对一些排版根据中文排版习惯做了调整，包括 ctexart / ctexrep / ctexbook。ctex 宏包和文档类能够识别操作系统和 TEX 发行版中安装的中文字体，因此基本无需额外配置即可排版中文文档。下面举一个使用 ctex 文档类排版中文的最简例子:

```latex
\documentclass{ctexart}
\begin{document}
在\LaTeX{}中排版中文。 汉字和English单词混排，通常不需要在中英文之间添加额外的空格。 当然，为了代码的可读性，加上汉字和 English 之间的空格也无妨。 汉字换行时不会引入多余的空格。
\end{document}
```

注意源代码须保存为 UTF-8 编码，并使用 xelatex 或 lualatex 命令编译。



### 一、字符

**空格和分段**

-   空格键和 Tab 键输入的空白字符视为“空格”
-   连续的若干个空白字符视为一个空格
-   一行开头的空格忽略不计
-   行末的换行符视为一个空格
-   连续两个换行符，也就是空行，会将文字分段
-   多个空行被视为一个空行
-   可以在行末使用 `\par` 命令分段

```latex
Several spaces     equal one.
  Front spaces are ignored.
An empty line starts a new paragraph.\par
A \verb|\par| command does the same.
```

**注释**

% 字符作为注释。在这个字符之后直到行末，所有的字符都被忽略，行末的换行符也不引入空格。

```latex
This is an % short comment
% ---
% Long and organized
% comments
% ---
example: Comments do not bre%
ak a word.
```

**特殊字符**

\# $ % & { } _ ^ ~ \ 为特殊字符，无法直接输入，需要按照以下带反斜线方式输入：

```latex
\# \$ \% \& \{ \} \_ \^{} \~{} \textbackslash
```

其中 `\^` 和 `\~` 两个命令需要一个参数，加一对花括号的写法相当于提供了空的参数，否则它们可能会将后面的字符作为参数，形成重音效果。

\\ 被直接定义成了手动换行的命令，输入反斜线就需要用 \textbackslash。

**标点**

单引号 ‘ ’ 分别用 ` 和 ' 输入，双引号 “ 和” 分别用 `` 和 '' 输入。

```latex
``Please press the `x' key.''
```

连字号(hyphen)、短破折号(en-dash)和长破折号(em-dash)。连字号 - 用来组成复合词；短破折号 – 用来连接数字表示范围，长破折号 — 用来连接单词，语义上类似中文的破折号。

```latex
daughter-in-law, X-rated\\
pages 13--67\\
yes---or no?
```

省略号：`\dots`，`\ldots`。

```latex
one, two, three, \dots one hundred.
```

其它符号：

<img src="/images/latex/other-character.png" style="zoom:33%;" />

**LaTex 标志**

```latex
\TeX
\LaTeX
\LaTeXe
```



### 二、断行和断页

LaTex 将文字段落在合适的位置进行断行，尽可能做到每行的疏密程度匀称，单词间距不会过宽或过窄。文字段落和公式、图表等内容从上到下顺序排布，并在合适的位置断页，分割成匀称的页面。在绝大多数时候，我们无需自己操心断行和断页。但偶尔会遇到需要手工调整的地方。

**单词间距**

西文排版实践中，断行的位置优先选取在两个单词之间，也就是在源代码中输入的“空格”。“空格”本身通常生成一个间距，它会根据行宽和上下文自动调整，文字密一些的地方，单词间距就略窄，反之略宽。

文字在单词间的“空格”处断行时，“空格”生成的间距随之舍去。我们可以使用字符 ~ 输入一个不会断行的空格，通常用在英文人名、图表名称等上下文环境:

```latex
Fig.~2a \\
Donald~E. Knuth
```

**手动断行和断页**

断行命令：

```latex
\\[⟨length⟩] % 可以带可选参数 ⟨length⟩，用于在断行处向下增加垂直间距，也在表格、公式等地方用于换行
\\*[⟨length⟩] % 禁止在断行处分页
\newline % 不带可选参数，只用于文本段落中
```

断页命令：

```latex
\newpage
\clearpage
```

