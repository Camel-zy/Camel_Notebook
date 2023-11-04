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

此处不强调封闭性，因为$y$相对于$t'$一定封闭。

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

### 一些应用

#### Function Composition

In mathematics, function composition can be composed as $(f\circ g)(x)=f(g(x))$.

With $\lambda$ notation, we can write $(f\circ g)(x)=\lambda x.f(g(x))$.

So, $\circ = \lambda f.\lambda g.\lambda x.f(g(x))$.

#### Booleans

$$
\begin{aligned}
&true=\lambda x.\lambda y.x\\\\
&false=\lambda x.\lambda y.y\\\\
\end{aligned}
$$

??? question "why?"
    根据`if`语义理解:
    $$
    \begin{aligned}
    &(\lambda x.\lambda y.x)a\ b=(\lambda y.a)b=a\\\\
    &(\lambda x.\lambda y.y)a\ b=(\lambda y.b)b=b\\\\
    \end{aligned}
    $$

#### Natural Numbers

Use `z` to represent `zero`, `s` to represent `successor`.

$$
\begin{aligned}
&\overline{0}=\lambda s.\lambda z.z\\\\
&\overline{1}=\lambda s.\lambda z.s\ z\\\\
&\overline{2}=\lambda s.\lambda z.s\ (s\ z)\\\\
&...\\\\
&\overline{n}=\lambda s.\lambda z.\underbrace{s\ (s\ (s\ ...\ (s}_{n\ times}\ z)))\\\\
\end{aligned}
$$

##### Define $z$ and $s$

The representation $\overline{n}$ iterates its first argument $n$ times over its second argument:

$\overline{n}\ f\ x=f^n(x)$, where $f^n(x)=\underbrace{f(f(...(f}_{n\ times}(x))))$.

$$
\begin{aligned}
&zero=\overline{0}=\lambda s.\lambda z.z\\\\
&succ=\lambda n.\overline{n+1}=\lambda n.\lambda s.\lambda z.s\ (n\ s\ z)\\\\
\end{aligned}
$$

???+ example
    $$
    \begin{aligned}
    &succ\ \overline{n}=\lambda s.\lambda z.s\ (\overline{n}\ s\ z)\\\\
    & =\lambda s.\lambda z.s\ (s^n(z))\\\\
    & =\lambda s.\lambda z.s^{n+1}(z)\\\\
    & = \overline{n+1}\\\\
    \end{aligned}
    $$

##### Arithmetic

常用运算的$\lambda$表达式详见[wiki](https://en.wikipedia.org/wiki/Church_encoding#Table_of_functions_on_Church_numerals)

此处仅以加法和乘法为例

=== "Add"
    $\lambda a.\lambda b.\lambda f.\lambda x.a\ f\ (b\ f\ x)$

    ??? tip "胡言乱语"
        整数$b$用$(b\ f\ x)$表示，即$f$在$x$上作用$b$次，在这基础上再把$f$作用$a$次，即得到$a+b$的表示。

=== "Mult"
    $\lambda a.\lambda b.\lambda f.\lambda x.a\ (b\ f)\ x$

    ??? tip "胡言乱语"
        表示$(b\ f)$在$x$上作用$a$次，其中$(b\ f)$表示$f$作用$b$次，即实现$a*b$。

#### Y Combinator

$Y$组合用于实现递归。

$Y = \lambda p.(\lambda f.p\ (f\ f))(\lambda f.p\ (f\ f))$

???+ tip "理解"
    假设我们需要实现递归函数$sum(n)=0+1+...+n$，思路为若$n$为$0$，则返回$0$，否则返回$n+sum(n-1)$，即$sum\ =\ \lambda n.\ is\_zero\ n\ 0\ (n\ +\ sum(pred\ n))$

    然而这样的等式目前无法写成$\lambda$表达式，$\lambda$表达式并不提供自我引用名字的机制。

    在$\lambda$表达式中，函数体内只能使用函数的参数，因此我们考虑将函数名作为函数参数的一部分传入函数体。

    ```cpp
    int proto(void *f, int n) {
        if (n == 0) return 0;
        return n + ((int (*)(void *, int))f)(f, n - 1);
    }
    ```

    转换为$\lambda$表达式，可以得到：

    $u\ =\ \lambda f.\lambda n.\ is\_zero\ n\ 0\ (n+f(pred\ n))$，此处的$f$即为$f(int\ n)$

    $proto\ =\ \lambda f.\lambda n.\ is\_zero\ n\ 0\ (n+f\ f\ (pred\ n))$，此处的$f$即为$f(void\ *f, int\ n)$

    $proto\ proto\ =\ (\lambda f.\lambda n.\ is\_zero\ n\ 0\ (n+f\ f\ (pred\ n)))\ proto\\
    =\ \lambda n.\ is\_zero\ n\ 0\ (n+proto\ proto\ (pred\ n))$，将proto代入，替换$f$

    $sum\ =\ proto\ proto\\
    =\ \lambda n.\ is\_zero\ n\ 0\ (n+sum\ (pred\ n))$

    我们将这个打包过程抽象出来，从最初的$u$得到最终所需的$proto\ proto$：

    $sum\ =\ proto\ proto\\
    =\ (\lambda f.u\ (f\ f))(\lambda f.u\ (f\ f))\\
    =\ (\lambda v,(\lambda f.v\ (f\ f))(\lambda f.v\ (f\ f)))\ u\\
    =\ Y\ u$

## Abstract Syntax Tree

- 叶结点是变量（或没有变量的运算符），内部结点是运算符，其子节点是它的参数。
- 变量是某种类型的不指定的语法片段。

<!-- *[或没有变量的运算符]: W4 开课小测 -->

### sort

ast按语法的不同分成不同的类别(sort)，记作$s$，类别的集合记作$S$。

### 运算符、元数

- 运算符$o$，其集合$O_s$，是在某个$s$上的集合
- 运算符的元数(arity)规定了运算符的类别、参数的个数$n$和每个参数的类别$s_i$，记作$(s_1,...,s_n)s$
- 一个类别为$s$、元数为$s_1,...,s_n$的运算符能将$n\ge 0$棵类别分别为$s_1,...,s_n$的ast组合起来，得到类别为$s$的ast

???+ tip "理解"
    运算符可理解为函数，元数可理解为函数的参数。

    就相当于

    ```cpp
    int f(int a, int b)
    ```

### 变量

变量$x$，集合$X_s$为某个$s$上的集合，变量族$X={X_S}_{s\in S}$ 。

### 结构归纳法

若要证明对某个类别的所有ast a，都具有性质$P(a)$，则考虑所有可以生成a的方式。若在每种情况下其子ast都具有该性质，则生成的a也具有该性质。

### 抽象语法树的族

$A[X]=\{A[X]_s\}_{s\in S}$是满足以下条件的最小族：

1. 一个类别为$s$的变量是一棵类别为$s$的ast：如果$x\in X_s$，则$x\in A[X]_s$ ;
2. 用运算符可以组合ast：如果$o$是一个元数为$(s_1,...,s_n)s$的运算符，且$a_1\in A[X]_{s_1},...,a_n\in A[X]_{s_n}$，则$o(a_1;...;a_n)\in A[X]_s$ 。

### 代换

如果$a\in A[X,x]_s$，且$b\in A[X]_s$，则用$b$代换$a$中出现的所有$x$得到的结果是$[b/x]a\in A[X]_s$ 。

定义ast a为代换目标，x为代换主体，代换由以下等式定义：
- $[b/x]x=b$，且当$x\ne y$时，$[b/x]y=y$；
- $[b/x]o(a_1,...,a_n)=o([b/x]a_1,...,[b/x]a_n)$。

#### 定理1

如果$a\in A[X,x]$，则对任意$b\in A[X]$都存在唯一的$c\in A[X]$满足$[b/x]a=c$ 。

## Abstract Binding Tree

abt是对ast的扩展，引入具有指定作用域的新变量和符号。

### let

$let\ x\ be\ a_1\ in\ a_2$，即令$a_2$中的$x$为$a_1$ 。

变量$x$受$let$表达式约束用于$a_2$中。

### 约束绑定

- abt通过允许运算符把任意有限个（可能为0）变量绑定到每个参数来泛化ast。
- 运算符$a$的参数称作抽象子(abstractor)，具有$x_1,...,x_k.a$的形式。
- 变量序列$x_1,...,x_k$在abt a中是约束的。
- $let\ x\ be\ a_1\ in\ a_2$，即$let(a_1;x.a_2)$，表明$x$在$a_2$中是约束的。
- 将$x_1,...,x_n$表示为$\overrightarrow{x}$，用$\overrightarrow{x}.a$表示$x_1,...,x_k.a$ 。

???+ example
    $$
    \begin{aligned}
    &let\ x\ be\ 7\ in\ x+x\ \rightarrow\ 7+7\\\\
    &let\ x\ be\ x*x\ in\ x+x\ \rightarrow\ x*x+x*x\\\\
    \end{aligned}
    $$

## Inductive Definition

### 判断

记法： $a\ J$ : abt a具有J性质。

*[$a\ J$]: 也可以记为$J\ a$

$-J$：不标记参数的J 。

### 推理规则

$$
\frac{J_1...J_k}{J}
$$

表明当$J_1,...,J_k$成立时，$J$成立，反之不一定成立。

其中，分子为前提，分母为结论。没有前提的规则成为公理。

### 推导

一个判断的推导过程是规则的**有限**组合，由公理开始，以判断结束。

推导是一棵树，结点是规则，其子节点是以该规则为前提的推导过程。

### 迭代归纳定义和联立归纳定义

- 迭代(iteration)：一个归纳定义建立在另一个归纳定义之上。
- 联立(simultaneous)：又称相互归纳定义，所有的判断形式的规则是由整个规则集合同时定义的。