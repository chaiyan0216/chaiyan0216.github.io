---
title: 电子
typora-root-url: ../../..
---

- [x] [文章] [How to Read a Schematic](https://learn.sparkfun.com/tutorials/how-to-read-a-schematic/all)

  内容比较好理解，各种原理图中符号的介绍。补充两个只记着名字的二极管。

  - 肖特基二极管（Schottky）：以其发明人肖特基博士命名，SBD 是肖特基势垒二极管（Schottky Barrier Diode）的简称。最显著的特点为反向恢复时间极短（可以小到几纳秒），正向导通压降仅 0.4V 左右。其多用作高频、低压、大电流整流二极管、续流二极管、保护二极管，也有用在微波通信等电路中作整流二极管、小信号检波二极管使用。在通信电源、变频器等中比较常见。
  
  - 齐纳二极管（Zener）：稳压二极管。利用 pn 结反向击穿状态，其电流可在很大范围内变化而电压基本不变的现象，制成的起稳压作用的二极管。
  
  ![齐纳二极管](/images/diode.png)



- [x] [文章] [Raspberry Pi GPIO Pinout: What Each Pin Does](https://www.tomshardware.com/reviews/raspberry-pi-gpio-pinout,6122.html)

  树莓派GPIO介绍：

  1. 每个引脚最大输出电流为16mA
  2. 同一时刻所有引脚总输出电流不超过51mA

