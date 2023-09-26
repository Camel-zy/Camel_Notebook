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

???+ tip "理解"
    对于函数抽象$\lambda x.t$，可将其理解为$x$为形参，$t$为函数体的函数。

### 恒等函数

$\lambda x.x$是一个恒等函数，其输入恒等于输出，可以用$I$来表示。

### 消歧约定

- 一个函数抽象的函数体尽可能向右扩展
???+ example
    $\lambda x.MN$等价于$\lambda x.(MN)$，而非$(\lambda x.M)N$

- 函数应用是左结合的
???+ example
    $MNP$等价于$(MN)P$，而非$M(NP)$

### 自由变量和绑定变量

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

### 封闭

$\lambda$项$t$是封闭的，当且仅当$FV(t)=\emptyset$，即$t$不包含自由变量。

相对封闭：$t$相对于$t'$封闭，当且仅当$FV(t)\cap BV(t')=\emptyset$。

???+ example
    $t=\lambda x.(\lambda x.x)(xx)$

    $(xx)$相对于$\lambda x.x$不是封闭的。

!!! quote "引理"
    闭项$s$相对于任意词项$t$封闭。

    闭项不含自由变量，因此显然成立。

### $\lambda$形式系统

$\lambda$记法是$\lambda$演算的一部分，这个演算用于对$\lambda$表达式进行推理，包含三个主要部分：

- 公理语义：推导表达式之间等式的形式系统。
- 操作语义：基于归约(reduce)的有向形式。
- 指称语义：TODO

#### 等式语义

- 定义等价（形式等价）：在定义语法时确定的等价，如$\lambda x.x$和$\lambda x.x$定义等价。
- 语义等价：两个词项的语义相同，如$\lambda x.x$和$\lambda y.y$语义等价。
 
三种语义等价：$\alpha$等价、$\beta$等价、$\eta$等价。

等式语义满足自反性、对称性、传递性。

在不引起歧义的情况下，可用$=$代替$\equiv$

##### 代换

记号$[N/x]M$表示在$M$中将表达式$N$代入$x$的结果。

???+ tip "理解"
    将$M$中的$x$替换为$N$,无论$x$是否为自由变量。

注意：$M$中不能有与$N$中的自由变量同名的约束变量，即$N$相对于$M$封闭。

若$M$中存在与$N$中的自由变量同名的约束变量，可先将$M$中的约束变量重命名，再进行代换。

##### $\alpha$等价

renaming, 换名等价

1. 对于任意的参数$x$与任意的词项$t'$，$\lambda x.t' =_\alpha \lambda y.[y/x]t'$。
2. 如果$t' =_\alpha t''$，那么对于任意的参数$x$，$\lambda x.t' =_\alpha \lambda x.t''$。
3. 如果$t_1 =_\alpha t_1'$，且$t_2 =_\alpha t_2'$，那么$t_1t_2 =_\alpha t_1't_2'$。

此处不强调封闭性，因为$y$相对于$\lambda x.t'$一定封闭。

##### $\beta$等价

reduction, 归约等价

1. 对于任意的参数$x$与任意的词项$t_1',t_2$，如果$t_2$相对于$t_1'$封闭，那么$(\lambda x.t_1')t_2 =_\beta [t_2/x]t_1'$。
2. 如果$t' =_\beta t''$，那么对于任意的参数$x$，$\lambda x.t' =_\beta \lambda x.t''$。
3. 如果$t_1 =_\beta t_1'$，且$t_2 =_\beta t_2'$，那么$t_1t_2 =_\beta t_1't_2'$。

---

注意$\beta$等价要求相对封闭。

???+ failure "错误示例"
    $$
    \begin{aligned}
    &\lambda y.\lambda z.(\lambda x.\lambda y.x)y\ z\\\\
    &\equiv_\beta\lambda y.\lambda z.(\lambda y.y)y\ z\\\\
    &\equiv_\beta\lambda y.\lambda z.z\\\\
    \end{aligned}
    $$

    $y$相对$\lambda y.x$不封闭。

??? success "改正"
    $$
    \begin{aligned}
    &\lambda y.\lambda z.(\lambda x.\lambda y.x)y\ z\\\\
    &\equiv_\alpha\lambda y.\lambda z.(\lambda x.\lambda t.x)y\ z\\\\
    &\equiv_\beta\lambda y.\lambda z.(\lambda t.y)z\\\\
    &\equiv_\beta\lambda y.\lambda z.y\\\\
    \end{aligned}
    $$

##### $\eta$等价

1. 对于任意的参数$x$与任意的词项$t'$，$\lambda y.(\lambda x.t')y =_\eta \lambda x.t'$。
2. 如果$t' =_\eta t''$，那么对于任意的参数$x$，$\lambda x.t' =_\eta \lambda x.t''$。
3. 如果$t_1 =_\eta t_1'$，且$t_2 =_\eta t_2'$，那么$t_1t_2 =_\eta t_1't_2'$。

???+ tip "理解"
    对$(\lambda x.t')y$运用$\beta$等价，可得$[y/x]t'$。因此$\lambda y.(\lambda x.t')y \rightarrow \lambda y.[y/x]t'$，显然等价于$\lambda x.t'$。

    就相当于
    ```cpp
    int f(int y) {
        return g(y);
    }
    ```
#### 操作语义

$\lambda$表达式的归约规则提供了等式推理的⼀种有向形式。例如$(\lambda x.x+5)3=3+5=8$

尽管$\beta$等价是等式，但一般倾向于看作化简：$(\lambda x.M)N\rightarrow [N/x]M$。

$\rightarrow$表示一步归约，$\twoheadrightarrow$表示零步或多步归约。

### Curring

将多参数函数转化为单参数函数的过程。例如，$1\ +\ 2\ =\ (+\ 1\ 2)\ =\ ((+\ 1)\ 2)\rightarrow 3$