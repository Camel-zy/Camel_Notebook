---
title: Principles of Programming Languages
subtitle: 编程语言原理
status: new
---

# Principles of Programming Languages

## $\lambda$ Calculus

### $\lambda$ terms

$\lambda$演算的语法将一些表达式定义为有效的$\lambda$演算式。

- 变量（名字）：变量$x$是一个有效的$\lambda$项
- 抽象（定义）：如果$t$是一个$\lambda$项，⽽$x$是⼀个变量，则$\lambda x.t$是⼀个$\lambda$项
- 应⽤（调⽤）：如果$t$和$s$是$\lambda$项，那么$ts$是⼀个$\lambda$项，有的作者⽤$ap(ts)$来表达

??? tip "理解"
    对于函数抽象$\lambda x.t$，可将其理解为$x$为形参，$t$为函数体的函数。

#### 恒等函数

$\lambda x.x$是一个恒等函数，其输入恒等于输出，可以用$I$来表示。

#### 消歧约定

- 一个函数抽象的函数体尽可能向右扩展
!!! example
    $\lambda x.MN$等价于$\lambda x.(MN)$，而非$(\lambda x.M)N$

- 函数应用是左结合的
!!! example
    $MNP$等价于$(MN)P$，而非$M(NP)$

#### 自由变量和绑定变量

类似形参绑定于函数体，此时形参即为绑定变量，不是绑定变量的就是自由变量。

???+ success "显然可得"
    $$
    \begin{aligned}
    &FV(x)={x}\\\\
    &FV(\lambda x.t')=FV(t')-{x}\\\\
    &FV(t_1t_2)=FV(t_1)\cup FV(t_2)\\\\
    \\\\
    &BV(x)=\emptyset\\\\
    &BV(\lambda x.t')=BV(t')\cup{x}\\\\
    &BV(t_1t_2)=BV(t_1)\cup BV(t_2)\\\\
    \end{aligned}
    $$
