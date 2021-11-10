---
title: BigDecimal
tags: bigdecimal
---

> **Need Of BigDecimal**
>
> - The two java primitive types(double and float) are floating point numbers, which is stored as a binary representation of a fraction and a exponent.
> - Other primitive types(except boolean) are fixed-point numbers. Unlike fixed point numbers, floating point numbers will most of the times return an answer with a small error (around 10^-19) This is the reason why we end up with 0.009999999999999998 as the result of 0.04-0.03 in the above example.
>
> But BigDecimal provides us with the exact answer.

Java原始类型 double、float在使用中会有微小误差。

BigDecimal提供精确计算。

```java
System.out.println(1.01+2.02);
// 3.0300000000000002
```

```java
System.out.println(BigDecimal.valueOf(1.01).add(BigDecimal.valueOf(2.02)));
// 3.03
```





#### 创建BigDecimal对象

- BigDecimal(String val)，字符串生成对象。
- static BigDecimal valueOf(double val)，静态方法等价于：new BigDecimal(Double.toString(val))。
- BigDecimal(double val)，禁用，由于double的不确定性，导致构造函数结果不确定。

```java
BigDecimal b1 = new BigDecimal("3.14");
BigDecimal b2 = BigDecimal.valueOf(3.14);
BigDecimal b3 = new BigDecimal(3.14);
System.out.println(String.format("%s, %s, %s", b1, b2, b3));
// 3.14, 3.14, 3.140000000000000124344978758017532527446746826171875
```





#### 特殊BigDecimal对象

0、1、10

```java
BigDecimal zero = BigDecimal.ZERO;
BigDecimal one = BigDecimal.ONE;
BigDecimal ten = BigDecimal.TEN;
System.out.println(String.format("%s, %s, %s", zero, one, ten));
// 0, 1, 10
```





#### 运算

```java
public BigDecimal add(BigDecimal value);//加法  
public BigDecimal subtract(BigDecimal value);//减法   
public BigDecimal multiply(BigDecimal value);//乘法  
public BigDecimal divide(BigDecimal value);//除法
```





#### 不可变

BigDecimal都是不可变的（immutable）的，进行每一步运算都会产生一个新的对象，所以在做加减乘除运算时要保存操作后的值。

```java
BigDecimal b1 =new BigDecimal("1.34");
System.out.println("b1 before: " + b1);
BigDecimal b2 =new BigDecimal("2.34");
b1.add(b2);
System.out.println("b1 after: " + b1);
// b1 before: 1.34
// b1 after: 1.34
```