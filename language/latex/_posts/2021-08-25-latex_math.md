---
title: LaTex 数学公式
tags: latex
typora-root-url: ../../..
---

[toc]



### 一、AMS 宏集

AMS 宏集是美国数学学会 (American Mathematical Society) 提供的对 LaTex 原生数学公式排版的扩展，其核心是 **amsmath** 宏包，对多行公式的排版提供了有力的支持。此外，**amsfonts** 宏包以及基于它的 **amssymb** 宏包提供了丰富的数学符号；**amsthm** 宏包扩展了 LaTex 定理证明格式。



### 二、公式排版基础

**行内公式和行间公式**

数学公式有两种排版方式：其一是与文字混排，称为行内公式；其二是单独列为一行排版，称为行间公式。

行内公式由一对 $ 符号包裹：

```latex
The Pythagorean theorem is
$a^2 + b^2 = c^2$.
```

行间公式由 equation 环境包裹。equation 环境为公式自动生成一个编号，这个编号可以用 `\label` 和 `\ref` 生成交叉引用，amsmath 的 `\eqref` 命令则为引用自动加上圆括号；还可以用 `\tag` 命令手动修改公式的编号，或者用 `\notag` 命令取消为公式编号。

```latex
The Pythagorean theorem is:
\begin{equation}
a^2 + b^2 = c^2 \label{pythagorean}
\end{equation}
Equation \eqref{pythagorean} is
called `Gougu theorem' in Chinese.

It's wrong to say
\begin{equation}
1 + 1 = 3 \tag{dumb}
\end{equation}
or
\begin{equation}
1 + 1 = 4 \notag
\end{equation}
```

如果需要直接使用不带编号的行间公式，则将公式用命令 `\[` 和 `\]` 包裹，与之等效的是 displaymath 环境。有的人更喜欢 equation* 环境，体现了带星号和不带星号的环境之间的区别。

```latex
\begin{equation*}
a^2 + b^2 = c^2
\end{equation*}
For short:
\[ a^2 + b^2 = c^2 \]
Or if you like the long one:
\begin{displaymath}
a^2 + b^2 = c^2
\end{displaymath}
```

为了与文字相适应，行内公式在排版大的公式元素（分式、巨算符等）时显得很“局促”，以下是对比：

```latex
In text:
$\lim_{n \to \infty}
\sum_{k=1}^n \frac{1}{k^2}
= \frac{\pi^2}{6}$.

In display:
\[
\lim_{n \to \infty}
\sum_{k=1}^n \frac{1}{k^2}
= \frac{\pi^2}{6}
\]
```

**数学模式**

当用户使用 $ 开启行内公式输入，或是使用 `\[` 命令、equation 环境时，LaTex 就进入了数学模式。数学模式相比于文本模式有以下特点：

- **空格被忽略**，数学符号的间距默认由符号的性质（关系符号、运算符等）决定。需要人为引入间距时，使用 `\quad` 和 `\qquad` 等命令。
- **不允许有空行**（分段），行间公式中也无法用 `\\` 命令手动换行。
- 所有的字母被当作数学公式中的变量处理，字母间距与文本模式不一致，也无法生成单词之间的空格。如果想在数学公式中输入正体的文本，简单情况下可用 `\mathrm` 命令，或者用 amsmath 提供的 `\text` 命令。

```latex
$x^{2} \geq 0 \qquad \text{for \textbf{all} } x \in \mathbb{R}$
```



### 三、数学符号

LaTex 默认提供了常用的数学符号，amssymb 宏包提供了一些次常用的符号。

**一般符号**

希腊字母符号的名称就是其英文名称，如 α (\alpha)、β (\beta) 等等。大写的希腊字母为首字母大写的命令，如 Γ (\Gamma)、∆ (\Delta) 等等。

<img src="/images/latex/greece_symbol.png" alt="greece_symbol" style="zoom:50%;" />

无穷大符号为 ∞ (\infty)，等等。

<img src="/images/latex/other_symbol.png" alt="other_symbol" style="zoom:50%;" />

省略号：. . . (\dots) 和 · · · (\cdots) ，以及竖排的  (\vdots) 和斜排的 (\ddots)。

**指数、上下标和导数**

用 ^ 和 _ 标明上下标，上下标的内容（子公式）一般需要用花括号包裹，否则上下标只对后面的一个符号起作用。

```latex
$p^3_{ij} \qquad
m_\mathrm{Knuth}\qquad
\sum_{k=1}^3 k $\\[5pt]
$a^x+y \neq a^{x+y}\qquad
e^{x^2} \neq {e^x}^2$
```

导数符号 ' 是一类特殊的上标，可以适当连用表示多阶导数，也可以在其后连用上标：

```latex
$f(x) = x^2 \quad f'(x)
= 2x \quad f''^{2}(x) = 4$
```

**分式和根式**

分式使用 `\frac{分子}{分母}` 来书写。分式的大小在行间公式中是正常大小，而在行内 极度压缩。amsmath 提供了方便的命令 `\dfrac` 和 `\tfrac`，令用户能够在行内使用正常大小的分式，或是反过来。

```latex
In display style:
\[
3/8 \qquad \frac{3}{8}
\qquad \tfrac{3}{8}
\]
In text style:
$1\frac{1}{2}$~hours \qquad
$1\dfrac{1}{2}$~hours
```

根式使用 `\sqrt{...}`，表示 n 次方根时写成 `\sqrt[n]{...}`。

```latex
$\sqrt{x} \Leftrightarrow x^{1/2}
\quad \sqrt[3]{2}
\quad \sqrt{x^{2} + \sqrt{y}}$
```

特殊的分式形式，如二项式结构，由 amsmath 宏包的 `\binom` 命令生成：

```latex
Pascal's rule is
\[
\binom{n}{k} =\binom{n-1}{k}
+ \binom{n-1}{k-1}
\]
```

**关系符**

常见的关系符号除了可以直接输入的 =，>，<，其它符号用命令输入，常用的有不等号 `\ne`、大于等于号 `≥ (\ge)` 和小于等于号 `≤ (\le)`、约等号 `≈ (\approx)`、等价` ≡ (\equiv)`、正比 `∝ (\propto)`、相似 `∼ (\sim)` 等等。

LaTex 还提供了自定义二元关系符的命令 `\stackrel`，用于将一个符号叠加在原有的二元关系符之上：

```latex
\[
f_n(x) \stackrel{*}{\approx} 1 
f_n(x) \stackrel{\ne}{\approx} 1
\]
```

<img src="/images/latex/binary_relation.png" alt="binary_relation" style="zoom:50%;" />

**算符**

算符大多数是二元算符，除了直接用键盘可以输入的 +、−、∗、/，其它符号用命令输入，常用的有乘号 `× (\times)`、除号 `÷ (\div)`、点乘 `· (\cdot)`、加减号 `± (\pm) / ∓ (\mp)` 、`∇ (\nabla)` 和 `∂ (\partial)` 等等。

<img src="/images/latex/binary_calc.png" alt="binary_calc" style="zoom:50%;" />

LaTex 也将数学函数的名称作为一个算符排版，字体为直立字体。

<img src="/images/latex/functions.png" alt="functions" style="zoom:50%;" />

**巨算符**

积分号 `\int`、求和号 `\sum` 等符号称为巨算符。

<img src="/images/latex/giant_operator.png" alt="giant_operator" style="zoom:50%;" />

巨算符在行内公式和行间公式的大小和形状有区别。

```latex
In text:
$\sum_{i=1}^n \quad
\int_0^{\frac{\pi}{2}} \quad
\oint_0^{\frac{\pi}{2}} \quad
\prod_\epsilon $ \\
In display:
\[\sum_{i=1}^n \quad
\int_0^{\frac{\pi}{2}} \quad
\oint_0^{\frac{\pi}{2}} \quad
\prod_\epsilon \]
```

巨算符的上下标位置可由 `\limits` 和 `\nolimits` 调整，前者令巨算符类似 lim 或求和算符上下标位于上下方；后者令巨算符类似积分号，上下标位于右上方和右下方。

```latex
In text:
$\sum\limits_{i=1}^n \quad
\int\limits_0^{\frac{\pi}{2}} \quad
\prod\limits_\epsilon $ \\
In display:
\[\sum\nolimits_{i=1}^n \quad
\int\limits_0^{\frac{\pi}{2}} \quad
\prod\nolimits_\epsilon \]
```

amsmath 宏包还提供了 `\substack`，能够在下限位置书写多行表达式；subarray 环境更进 一步，令多行表达式可选择居中 (c) 或左对齐 (l)：

```latex
\[
\sum_{\substack{0\le i\le n \\
  j\in \mathbb{R}}}
P(i,j) = Q(n)
\]
\[
\sum_{\begin{subarray}{l}
0\le i\le n \\
  j\in \mathbb{R}
\end{subarray}}
P(i,j) = Q(n)
\]
```

**重音和箭头**

<img src="/images/latex/hats.png" alt="hats" style="zoom:50%;" />

<img src="/images/latex/arrows.png" alt="arrows" style="zoom:50%;" />

<img src="/images/latex/hat-arrows.png" alt="hat-arrows" style="zoom:50%;" />

```latex
$\bar{x_0} \quad \bar{x}_0$\\[5pt]
$\vec{x_0} \quad \vec{x}_0$\\[5pt]
$\hat{\mathbf{e}_x} \quad
 \hat{\mathbf{e}}_x$
 
$0.\overline{3} =
\underline{\underline{1/3}}$ \\[5pt]
$\hat{XY} \qquad \widehat{XY}$\\[5pt]
$\vec{AB} \qquad
\overrightarrow{AB}$

$\underbrace{\overbrace{(a+b+c)}^6
\cdot \overbrace{(d+e+f)}^7}
_\text{meaning of life} = 42$
```

amsmath 的 `\xleftarrow` 和 `\xrightarrow` 命令提供了长度可以伸展的箭头，并且可以为箭头增加上下标：

```latex
\[ a\xleftarrow{x+y+z} b \]
\[ c\xrightarrow[x<y]{a*b*c}d \]
```

**括号和定界符**

<img src="/images/latex/braces.png" alt="braces" style="zoom:50%;" />

LaTex 提供了多种括号和定界符表示公式块的边界，如小括号 ()、中括号 []、大括号 {}(\{\})、尖括号 ⟨⟩ (\langle \rangle)等。

```latex
${a,b,c} \neq \{a,b,c\}$
```

使用 `\left` 和 `\right` 命令可令括号（定界符）的大小可变，在行间公式中常用。LaTex 会自动根据括号内的公式大小决定定界符大小。`\left` 和 `\right` 必须成对使用。需要使用单个定界符时，另一个定界符写成 `\left.` 或 `\right.`。

```latex
\[1 + \left(\frac{1}{1-x^{2}}
\right)^3 \qquad
\left.\frac{\partial f}{\partial t}
\right|_{t=0}\]
```



### 四、多行公式

**长公式折行**

通常来讲应当避免写出超过一行而需要折行的长公式。如果一定要折行的话，习惯上优先在等号之前折行，其次在加号、减号之前，再次在乘号、除号之前。其它位置应当避免折行。

amsmath 宏包的 multline 环境提供了书写折行长公式的方便环境。它允许用 `\\` 折行，将公式编号放在最后一行。多行公式的首行左对齐，末行右对齐，其余行居中。

```latex
\begin{multline}
a+b+c+d+e+f + g + h + i \\
= j + k + l + m + n \\
= o + p + q + r + s \\
=t+u+v+x+z
\end{multline}
```

**多行**

align 环境，它将公式用 & 隔为两部分并对齐。分隔符通常放在等号左边：

```latex
\begin{align}
a & = b + c \\
&=d+e
\end{align}
```

align 环境会给每行公式都编号。但可以用 `\notag` 去掉某行的编号：

```latex
\begin{align}
a ={} & b + c \\
  ={} & d + e + f + g + h + i
        + j + k + l \notag \\
      & + m + n + o \\
  ={} & p + q + r + s
\end{align}
```

align 还能够对齐多组公式，除等号前的 & 之外，公式之间也用 & 分隔：

```latex
\begin{align}
a &=1  & b &=2  & c &=3 \\
d &=-1 & e &=-2 & f &=-5
\end{align}
```

如果不需要按等号对齐，只需罗列数个公式，gather 将是一个很好用的环境：

```latex
\begin{gather}
a = b + c \\
d = e + f + g \\
h + i = j + k \notag \\
l+m=n
\end{gather}
```

**公用编号**

另一个常见的需求是将多个公式组在一起公用一个编号，编号位于公式的居中位置。为此，amsmath 宏包提供了诸如 aligned、gathered 等环境，与 equation 环境套用。以 -ed 结尾的环境用法与不以 -ed 结尾的环境用法一一对应。

```latex
\begin{equation}
\begin{aligned}
a &= b + c \\
d &= e + f + g \\
h + i &= j + k \\
l + m &= n
\end{aligned}
\end{equation}
```



### 五、数组和矩阵

array 环境用来排版数组和矩阵，用法与 tabular 环境极为类似，也需要定义列格式，并用 \\ 换行。数组可作为一个公式块，在外套用 `\left`、`\right` 等定界符：

```latex
\[ \mathbf{X} = \left(
\begin{array}{cccc}
x_{11} & x_{12} & \ldots & x_{1n}\\
x_{21} & x_{22} & \ldots & x_{2n}\\
\vdots & \vdots & \ddots & \vdots\\
x_{n1} & x_{n2} & \ldots & x_{nn}\\
\end{array} \right) \]
```

```latex
\[ |x| = \left\{
\begin{array}{rl}
-x & \text{if } x < 0,\\
0 & \text{if } x = 0,\\
x & \text{if } x > 0.
\end{array} \right. \]
```

上述例子也可以用 amsmath 提供的 cases 环境更轻松地完成:：

```latex
\[ |x| =
\begin{cases}
-x & \text{if } x < 0,\\
0 & \text{if } x = 0,\\
x & \text{if } x > 0.
\end{cases} \]
```

amsmath 宏包还直接提供了多种排版矩阵的环境，包括不带定界符的 matrix，以及带各种定界符的矩阵 pmatrix( ( )、bmatrix( [ )、Bmatrix( { )、vmatrix( | )、Vmatrix( || )。

```latex
\[
\begin{matrix}
1 & 2 \\ 3 & 4
\end{matrix} \qquad
\begin{bmatrix}
x_{11} & x_{12} & \ldots & x_{1n}\\
x_{21} & x_{22} & \ldots & x_{2n}\\
\vdots & \vdots & \ddots & \vdots\\
x_{n1} & x_{n2} & \ldots & x_{nn}\\
\end{bmatrix}
\]
```



### 六、公式间距

<img src="/images/latex/distances.png" alt="distances" style="zoom:50%;" />

绝大部分时候，数学公式中各元素的间距是根据符号类型自动生成的，需要我手动调整的情况极少。

一个常见的用途是修正积分的被积函数 f(x) 和微元 dx 之间的距离。

```latex
\[
\int_a^b f(x)\mathrm{d}x
\qquad
\int_a^b f(x)\,\mathrm{d}x
\]
```

另一个用途是生成多重积分号。如果我们直接连写两个 `\int`，之间的间距将会过宽，此时可以使用负间距 `\!` 修正之。不过 amsmath 提供了更方便的多重积分号，如二重积分 `\iint`、三 重积分 `\iiint` 等。

```latex
\newcommand\diff{\,\mathrm{d}}
\begin{gather*}
\int\int f(x)g(y)
\diff x \diff y \\
\int\!\!\!\int
f(x)g(y) \diff x \diff y \\
\iint f(x)g(y) \diff x \diff y \\
\iint\quad \iiint\quad \idotsint
\end{gather*}
```



### 七、定理环境

**LaTex 原始定理环境**

LaTex 提供了 一个基本的命令 `\newtheorem` 提供定理环境的定义：

```latex
\newtheorem{⟨theorem environment⟩}{⟨title⟩}[⟨section-level⟩] 
\newtheorem{⟨theorem environment⟩}[⟨counter⟩]{⟨title⟩}
```

⟨theorem environment⟩ 为定理环境的名称。原始的 LaTex 里没有现成的定理环境，不加定义而直接使用很可能会出错。⟨title⟩ 是定理环境的标题（“定理”，“公理”等）。

定理的序号由两个可选参数之一决定，它们不能同时使用：

-   ⟨section level⟩ 为章节级别，如 chapter、section 等，定理序号成为章节的下一级序号

-   ⟨counter⟩ 为用 `\newcounter` 自定义的计数器名称，定理序号由这个计数器管理

如果两个可选参数都不用的话，则使用默认的与定理环境同名的计数器。

以下示例代码中，定义了一个 mythm 环境，其序号设为 section 的下一级序号。

```latex
\newtheorem{mythm}{My Theorem}[section]
\begin{mythm}\label{thm:light}
The light speed in vacuum
is $299,792,458\,\mathrm{m/s}$.
\end{mythm}
\begin{mythm}[Energy-momentum relation]
The relationship of energy,
momentum and mass is
\[E^2 = m_0^2 c^4 + p^2 c^2\]
where $c$ is the light speed
described in theorem \ref{thm:light}.
\end{mythm}
```

**amsthm 宏包**

amsthm 提供了 `\theoremstyle` 命令支持定理格式的切换，在用 `\newtheorem` 命令定义定理环境之前使用。amsthm 预定义了三种格式用于 \theoremstyle:plain 和 LaTex 原始的格式 一致；definition 使用粗体标签、正体内容；remark 使用斜体标签、正体内容。

另外 amsthm 还支持用带星号的 `\newtheorem*` 定义不带序号的定理环境：

```latex
\theoremstyle{definition} \newtheorem{law}{Law}
\theoremstyle{plain} \newtheorem{jury}[law]{Jury}
\theoremstyle{remark} \newtheorem*{mar}{Margaret}

\begin{law}\label{law:box}
Don't hide in the witness box.
\end{law}
\begin{jury}[The Twelve]
It could be you! So beware and
see law~\ref{law:box}.\end{jury}
\begin{jury}
You will disregard the last
statement.\end{jury}
\begin{mar}No, No, No\end{mar}
\begin{mar}Denis!\end{mar}
```

**证明环境和证毕符号**

amsthm 还提供了一个 proof 环境用于排版定理的证明过程。proof 环境末尾自动加上一个证毕符号：

```latex
\begin{proof}
For simplicity, we use
\[
E=mc^2
\]
That's it.
\end{proof}
```

如果行末是一个不带编号的公式， 符号会另起一行，这时可使用 `\qedhere` 命令将符号放在公式末尾：

```latex
\begin{proof}
For simplicity, we use
\[
E=mc^2 \qedhere
\]
\end{proof}
```

