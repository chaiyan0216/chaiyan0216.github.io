---
title: 终端录屏
tags: asciinema
typora-root-url: ../..
---

- [asciinema](https://github.com/asciinema/asciinema)，终端录制

安装：brew install asciinema

使用：

**asciinema rec** *filename*：进行录制，完毕时通过exit或ctrl+d结束，结束后会生成一个*.cast后缀的JSON文件；

**asciinema play** *filename*：播放录制的文件。



- [asciicast2gif](https://github.com/asciinema/asciicast2gif)，从cast文件生成git

安装依赖：brew install ImageMagick gifsicle node

安装：npm install --global asciicast2gif

使用：**asciicast2gif** *input.cast* output.git



![asciinema](/images/asciinema.gif)