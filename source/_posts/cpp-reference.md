---
title: cpp reference
date: 2024-06-28 11:36:03
categories:
- Program
tags:
- C++
---


# 简介
C++ Reference[^1]提供了C++标准库的详细文档和参考信息.

## Arithmetic operators
算数操作很多,这里只针对除零问题进行简单分析.关于其它一些数学计算如atan2[^3]也有关于输入时inf或者NaN的标准定义

### Multiplicative operators
主要包括乘法(Multiplication),除法(division)和取余(remainder)
- lhs * rhs
- lhs / rhs
- lhs % rhs

#### Division

- c++整数相除，除数=0，标准未定义，由编译器决定，通常是程序崩溃（win，qnx）
- c++浮点数相除（只要有1个操作数被定义为浮点数，如float，double，另一个操作数即使整数也会隐式转换为浮点数），被除数=0，程序不崩溃。
  - 任意操作数NaN，返回NaN（not a number）
  - 带符号无穷大，+/0.0==inf，-/0.0=-inf
  - 0.0/0.0=NaN

参考C++  Arithmetic operators[^2]中除法定义

>In the remaining description in this section, "operand(s)>", lhs and rhs refer to the converted operand(s).
>
>2) The result of built-in division is lhs divided by rhs. >If rhs is zero, the behavior is undefined.
>If both operands have an integral type, the result is the >algebraic quotient (performs integer division): the >quotient is truncated towards zero (fractional part is >discarded).
>If both operands have a floating-point type, and the type >supports IEEE floating-point arithmetic (see >std::numeric_limits::is_iec559):
>- If one operand is NaN, the result is NaN.
>- Dividing a non-zero number by ±0.0 gives the >correctly-signed infinity and FE_DIVBYZERO is raised.
>- Dividing 0.0 by 0.0 gives NaN and FE_INVALID is raised.


[^1]: https://en.cppreference.com/w/
[^2]: https://en.cppreference.com/w/cpp/language/operator_arithmetic
[^3]: https://en.cppreference.com/w/cpp/numeric/math/atan2