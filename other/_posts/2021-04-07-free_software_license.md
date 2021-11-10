---
title: 自由软件与开源许可
tags: gnu gpl mit bsd apache
typora-root-url: ../..
---

所谓自由软件（Free Software），就是让你可以自由使用的软件。

英文 free software 里的 free 不是单单指免费（free of charge）的意思，而重要的是自由（freedom）的意思。

自由软件的作者这样做，并不是因为他们不需要钱，而是因为他们觉得，**自由比金钱更重要**。

> “自由软件”尊重用户的自由，并且尊重整个社区。粗略来讲，一个软件如果是自由软件，这意味着**用户可以自由地运行，拷贝，分发，学习，修改并改进该软件**。因此，“自由软件”是关乎自由的问题，与价格无关，软件如何定价并不影响它是否被归类为自由软件。

> 现在还有另外一伙人，使用“开源”一词来表达与“自由软件”类似，但不完全相同的概念。我们更倾向于使用“自由软件”这个词。因为一旦你看到自由二字，就明白了它所要表达的意思。而“开放”[却并不意味着自由](https://www.gnu.org/philosophy/open-source-misses-the-point.html)。

> by Richard Stallman
>
> The terms “free software” and “open source” stand for almost the same range of programs. However, they say deeply different things about those programs, based on different values. The free software movement campaigns for freedom for the users of computing; it is a movement for freedom and justice. By contrast, the open source idea values mainly practical advantage and does not campaign for principles. This is why we do not agree with open source, and do not use that term.

自由软件和开源软件总体上来说代表的同一类软件，但自由软件基金会的创始人理查德·斯托曼从理念的角度给出了其两者深层次的巨大差异。自由软件是以用户的自由作为出发点的，而开源软件则重视的是实践上的优势。



### 开源许可

开源许可证有很多种，但我们常见的主要有：MIT、BSD、Apache、LGPL 和  GPL。

这里有这些许可的内容：https://opensource.org/licenses

这些开源许可从宽松到严格排序是：MIT <= BSD < Apache < LGPL < GPL

MIT、BSD 源自大学，体现了简单、开放和包容的特点。

MIT、BSD、Apache 三者都支持闭源的后续开发，可以用在商业领域。

GPL、LGPL 传染性开源，编译的代码里用了这里的代码，都必须开源。

具体每一种许可的限制如下：

- MIT

  只需要保留作者的版权，而再无其他任何限制。

  - 有对软件/源码有无限制的处理权力，包括但不限于使用、复制、发布、出售等等
  - 在项目副本中要包含版权声明和许可声明
  - 作者和版权所有者无需承担任何责任

- BSD

  BSD 分为 2-Clause 和 3-Clause 两种，2-Clause 的又叫做 FreeBSD。3-Clause 是额外增加了不可以用作者或者贡献者的名字做推广的要求。

  - 如果再发布的产品中包含源代码，则在源代码中必须带有原来代码中的 BSD 协议。
  - 如果再发布的只是二进制类库/软件，则需要在类库/软件的文档和版权声明中包含原来代码中的 BSD 协议。
  - 不可以用开源代码的作者/机构名字和原来产品的名字做市场推广。

- Apache License 2.0

  - 需要给代码的用户一份Apache Licence。
  - 需要对任何被修改文件进行说明。
  - 在延伸的代码中（修改和有源代码衍生的代码中）需要带有原来代码中的协议，商标，专利声明和其他原来作者规定需要包含的说明。
  - 如果再发布的产品中包含一个 Notice 文件，则在 Notice 文件中需要带有 Apache Licence。你可以在Notice中增加自己的许可，但不可以表现为对Apache Licence构成更改。

- GPL

  来源自由软件联盟 GNU，GPL/LGPL 侧重于代码及衍生代码的开源与免费使用。

  GPL 协议的主要内容是只要在一个软件中使用（“使用”指类库引用，修改代码或者衍生代码）GPL 协议的产品，则该软件产品必须也采用 GPL 协议，既必须也是开源和免费，**这就是所谓的”传染性”**。

  Linux 采用了 GPL。

- LGPL

  Lesser GPL 是稍宽松的 GPL 许可。LGPL 跟 GPL 的主要差别在于引用方式的类库在 LGPL 许可下不需要开源，这就使得其可以以类库引用的方式应用在商业软件上，不需要商业软件强制开源。

  但如果需要修改 LGPL 许可的代码或者衍生，那所有修改的代码或者衍生代码依然还是需要采用 LGPL 许可。



最后附上阮一峰老师制作的中文版开源许可选择图。

![select_open_source_licence](https://www.ruanyifeng.com/blogimg/asset/201105/free_software_licenses.png)