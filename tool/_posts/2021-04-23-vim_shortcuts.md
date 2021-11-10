---
title: Vim Shortcuts
tags: vim
typora-root-url: ../..
---

网上的一篇 vim 新手教程，非常不错。

[简明 VIM 练级攻略](https://coolshell.cn/articles/5426.html)

```
:help x		命令帮助
:e file		打开另一个文件到 buffer
:bn				切换 buffer
:bp				切换 buffer
.					重复上个命令
N<command>	重复某个命令 N 次
100ihello [esc]	插入 100 个 hello
%				括号跳转
* #			匹配光标当前所在的单词，移动光标到下一个匹配单词
fx			移动到下一个 x 字符处
3fx			移动到第 3 个 x 字符处
tx			移动到下一个 x 的前一字符处
dtx			删除直到遇到下一个 x 字符
ytx			复制到下一个 x 字符处
ctx			类似 dtx，只是会进入 insert
```

![Word moves example](/images/word_moves.jpg)

![Line moves](/images/line_moves.jpg)

##### `<action>a<object>` 或 `<action>i<object>`，这两个命令非常强大。

-   action 可以是任何的命令，如 `d` (删除), `y` (拷贝), `v` (可以视模式选择)。
-   object 可能是： `w` 一个单词， `W` 一个以空格为分隔的单词， `s` 一个句字， `p` 一个段落，也可以是一个特别的字符：`"`、 `'`、 `)`、 `}`、 `]。`

假设你有一个字符串 `(map (+) ("foo"))`.而光标键在第一个 `o `的位置。

>   -   `vi"` → 会选择 `foo`.
>   -   `va"` → 会选择 `"foo"`.
>   -   `vi)` → 会选择 `"foo"`.
>   -   `va)` → 会选择`("foo")`.
>   -   `v2i)` → 会选择 `map (+) ("foo")`
>   -   `v2a)` → 会选择 `(map (+) ("foo"))`

![Text objects selection](/images/textobjects.png)

##### 宏录制

-   `qa` 把你的操作记录在寄存器 `a。`
-   于是 `@a` 会replay被录制的宏。
-   `@@` 是一个快捷键用来replay最新录制的宏。

>   **示例**
>
>   在一个只有一行且这一行只有“1”的文本中，键入如下命令：
>
>   -   qaYp<C-a>q
>       -   `qa` 开始录制
>       -   `Yp` 复制行
>       -   `<C-a>` 增加1
>       -   `q` 停止录制
>   -   `@a` → 在1下面写下 2
>   -   `@@` → 在2 正面写下3
>   -   现在做 `100@@` 会创建新的100行，并把数据增加到 103.



以下是自己之前的一些记录，学而时习之。

#### 移动

Normal 模式下

```shell
gg		    首行
G			末行
#G/gg		第 # 行
H			屏幕最上方
M			屏幕中间
L			屏幕最下方
%			括号内，在左右两括号间跳转（默认支持大、中、小三种括号）
0			行首
^			非空行首
$			行尾
f/Fx		跳到下/前一个 x 出现的位置
{			下一个段落
}			上一个段落

zz			光标所在处滚动到屏幕中央
C-e			屏幕下滚一行
C-y			屏幕上滚一行
C-d			光标下移半屏幕
C-u			光标上移半屏幕
C-f			光标下移整屏幕
C-b			光标上移整屏幕

[{			跳转到当前代码段开始处
]}			跳转到当前代码段结束处

hjkl		左上下右
w/W			到下一个单词首
e/E			到当前单词末尾
b/B			到前一单词首
#xxx		以上命令之前可以加数字，表示执行多次
```



#### 编辑

Normal 模式下

```shell
I			行首插入
i			光标前插入
a			光标后插入
A			行尾插入
o			在后一行插入
O			在前一行插入
ea			当前词尾插入

x			删除当前字符
#x			删除 # 个字符
dw/W		删除单词（光标处到词尾）
diw/W 		删除单词（单词头到单词尾）
d0/$		删除到行首/尾
dd			删除当前行
d#w			删除 # 个单词
#dd			删除 # 行
dib/B		删除代码段
dab/B		删除代码段（连带括号）
das			删除语句

yw/W		复制到词尾
yiw/W		复制当前词
y0/$		复制到行首/尾
yy			复制当前行
y#w			复制 # 个词
#yy			复制 # 行
yib/B		复制代码段
yab/B		复制代码段（连带括号）
yas			复制语句
“+y			复制到系统剪切板

p			行后粘贴
P			行前粘贴
“+p			从系统剪切板粘贴

rx			用新字符 x 替换当前光标下的字符
R			进入 replace 模式，可以连续替换字符

cw/W		删除到词尾，并进入 insert 模式（change）
ciw/W		删除当前单词，并进入 insert 模式
ce/E		删除到词尾，并进入 insert 模式
c#w/e		删除多个单词，并进入 insert 模式
c0/$		删除到行首/尾，并进入 insert 模式
cc			删除整行，并进入 insert 模式
cib/B		删除代码段，并进入 insert 模式		
cab/B		删除代码段（连带括号），并进入 insert 模式
cas			删除语句，并进入 insert 模式

s			删除当前字符，并进入 insert 模式

J			用空格连接当前行和下一行
gJ			直接连接当前行和下一行
#J			连接 # 行

u			undo
U			undo 当前行
C-r			redo

g~			随着光标移动切换大小写
gu			随着光标移动切换小写
gU			随着光标移动切换大写

>>			增加缩进			
<<			减少缩进
```

Insert 模式下

```shell
C-h			删除前一个字符
C-w			删除前一个单词
C-t			缩进
C-d			减少缩进
```



#### 查找

Normal 模式下（hlsearch，incsearch）

```shell
*			搜索当前光标下的单词并高亮
/xxx		回车后，查找 xxx，n 向下查找，N 向上查找
?xxx		与 / 查找相反

:s/old/new/g		在当前行做文本替换
:#,#s/old/new/g		在 #,# 行间做文本替换
:%s/old/new/g		在全文做文本替换
:%s/old/new/gc		每次替换前进行确认
```



#### 读写文件

Visual 模式下

```shell
:w filename			写选中内容到新文件
```

Normal 模式下

```shell
:r filename    读文件内容到当前行
:r !cmd        读 cmd 执行结果到当前行
:w file        另存为文件 file
:m,n w file    m-n 行另存到文件 file 中
:m,n w >> file m-n 行追加到文件 file 中
```



#### 可视模式

Visual 模式下（v，V，C-v）

```shell
aw			标记单词
a/ib		标记 () 块
a/iB		标记 {} 块
a/it		标记 <> 块

y			复制
d			删除
~			切换大小写
u			改为小写
U			改为大写
J			连接所有行
< >		左右缩进
=			自动缩进
```



#### 标记和位置

Normal 模式下

```shell
m[a-z]		为当前位置做标记
`[a-z]		跳转到某位置
:marks		查看所有标记

:ju			查看所有跳转位置
``			跳转到上次位置
```



#### 命令

```shell
C-g			在状态栏显示当前光标所处位置

:ter		打开一个终端

:!cmd		执行一个 cmd 命令

:x			wq

K			man 当前光标所在词

gt/T		切换标签

ga			显示光标下字符的 ASCII
```



#### 窗口

```shell
C-ww			切换窗口
C-wq			关闭窗口
C-ws			分割窗口
C-wv			竖直分割窗口
C-wh/j/k/l		移动光标到相应窗口
```



#### 操作

1.  连续替换

```shell
* cw xxx esc n .

*		用来搜索当前光标下的单词
cw		改写该单词
xxx		为新单词
n		跳转到下一处
.		重复执行
```

2.  插入多次

```shell
3i xxx esc

3i		表示 3 次重复
xxx		需要写入的词
ese		执行 3 次重复
```

3.  多行注释

```shell
C-v <select> I # esc esc

C-v			进入块选择模式
<select>	选择行
I			插入（大写 I）
#			插入注释符号
esc			两次 esc 执行
```

