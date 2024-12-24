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
    $t=\lambda x.(\lambda x.x)(x\ x)$

    $(x\ x)$相对于$\lambda x.x$不是封闭的。

!!! quote "引理"
    闭项$s$相对于任意词项$t$封闭。

    闭项不含自由变量，因此显然成立。

### $\lambda$形式系统

$\lambda$记法是$\lambda$演算的一部分，这个演算用于对$\lambda$表达式进行推理，包含三个主要部分：

- 公理语义：推导表达式之间等式的形式系统。
- 操作语义：基于归约(reduce)的有向形式。
- 指称语义：

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
    &\equiv_\beta\lambda y.\lambda z.(\lambda y.y)\ z\\\\
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

    我们发现，$proto\ proto$就是我们所需的递归函数。

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

## Statics

引入一种简单表达式语言**E**。

### 语法

$$
\begin{aligned}
Typ\quad \tau\quad ::=\quad &num\\
&str\\
Exp\quad e\quad ::=\quad &x\\
&num[n]\\
&str[s]\\
&plus(e_1;e_2)\\
&times(e_1;e_2)\\
&cat(e_1;e_2)\\
&len(e)\\
&let(e_1;x.e_2)\quad 表示let\ x\ be\ e_1\ in\ e_2\\
\end{aligned}
$$


???+ quote "引入与消去"
    语言的结构分为两种形式。

    - 引入(introduction)：确定类型的值或范式。
    - 消去(elimination)：确定如何操作该类型的值以形成另一种类型的计算。

    在**E**语言系统中，$num$和$str$是将字面量引入**E**语言系统，加法、乘法即为消去。

### 类型系统

类型系统用于约束短语(phrase)的形成，其中短语是上下文敏感的。

例如，表达式$plus(x;num[n])$是否合理，取决于$x$的类型是否为$num$。

**E**的静态语义由如下形式的泛型假言判断的归纳定义组成：

$$
\overrightarrow{x}|\Gamma \vdash e:\tau
$$

- $\overrightarrow{x}$：变量的有限集合
- $\Gamma$：定型上下文（typing context），针对每个$x\in\overrightarrow{x}$有一个形如$x:\tau$的假设，$\Gamma={x_1:\sigma_1,...,x_k:\sigma_k}$
- 定型公理：$e:\tau$，符号$e$有类型$\tau$

??? note "**E**的静态语义规则"
    前三条为定型公理，之后四条为定型判断，最后一条表示若变量$x$尚未在$\Gamma$中声明，$e_1$是类型$\tau_1$的而$e_2$是类型$\tau_2$的，则将$e_2$中的$e_1$替换为$x$之后，$e_2$的类型不变。
    $$
    \begin{aligned}
    &\frac{}{\Gamma,x:\tau\vdash x:\tau}\\\\
    &\frac{}{\Gamma\vdash str[s]:str}\\\\
    &\frac{}{\Gamma\vdash num[n]:num}\\\\
    \\\\
    &\frac{\Gamma\vdash e_1:num\quad\Gamma\vdash e_2:num}{\Gamma\vdash plus(e_1;e_2):num}\\\\
    &\frac{\Gamma\vdash e_1:num\quad\Gamma\vdash e_2:num}{\Gamma\vdash times(e_1;e_2):num}\\\\
    &\frac{\Gamma\vdash e_1:str\quad\Gamma\vdash e_2:str}{\Gamma\vdash cat(e_1;e_2):str}\\\\
    &\frac{\Gamma\vdash e:str}{\Gamma\vdash len(e):num}\\\\
    \\\\
    &\frac{\Gamma\vdash e_1:\tau_1\quad\Gamma,x:\tau_1\vdash e_2:\tau_2}{\Gamma\vdash let(e_1;x.e_2):\tau_2}\\\\
    \end{aligned}
    $$

我们用$x\notin dom(\Gamma)$表示对任意类型$\tau$，在$\Gamma$中没有$x:\tau$的假设，此时称变量$x$对于$\Gamma$是新的，且有$\Gamma,x:\tau=\Gamma\cup x:\tau$。

#### 类型唯一性

引理4.1(Unicity of Typing):

对每个定型上下⽂$\Gamma$和表达式$e$，最多存在⼀个$\tau$，满⾜$\Gamma\vdash e:\tau$。

即**E**语言没有变量重载。

#### 定型反转

引理4.2(inversion for typing):

假设$\Gamma\vdash e:\tau$，若$e=plus(e_1;e_2)$，则$\tau=num,\Gamma\vdash e_1:num$，且$\Gamma\vdash e_2:num$。

即可以根据结果类型反推出参数类型。

#### 弱化

引理4.3(weakening):

若$\Gamma\vdash e':\tau'$，则对任意$x\notin dom(\Gamma)$和类型$tau$，有$\Gamma,x:\tau\vdash e':\tau'$。

即可以在上下文中添加新的变量，不影响原有的定型。

#### 代换

引理4.4(substitution):

若$\Gamma,x:\tau\vdash e':\tau'$，且$\Gamma\vdash e:\tau$，则$\Gamma\vdash [e/x]e':\tau'$。

即可以同类型代换。

#### 分解

引理4.5(decomposition):

若$\Gamma\vdash [e/x]e':\tau'$，则对满足$\Gamma\vdash e:\tau$的每个类型$\tau$，有$\Gamma,x:\tau\vdash e':\tau'$。

即代换的逆过程。

## Dynamics

### 转换系统

转换系统由以下4种形式的判断来描述：

1. $s\ state$，断言$s$是转换系统的一个状态
2. $s\ final$，对$s\ state$，断言$s$是一个终结状态
3. $s\ initial$，对$s\ state$，断言$s$是一个初始状态
4. $s\mapsto s'$，对$s\ state$和$s'\ state$，断言状态$s$可以转换到状态$s'$

---

- 如果$s\ final$，那么没有$s'$满足$s\mapsto s'$。
- 一个无法转换的状态是卡住的(stuck)
- 所有终结状态都是卡住的，非终结状态也可能是卡住的
- 一个转换系统是确定的(deterministic)当且仅当对所有的状态$s$，最多只有一个$s'$满足$s\mapsto s'$。【注意：转换过程可以有多条路径，最终状态唯一即可】

#### 转换序列

转换序列是一系列状态$s_0,...,s_n$，满足$s_0\ initial$，且对任意$0\le i<n$，都有$s_i\mapsto s_{i+1}$。

- 一个转换序列是最大的(maximal)当且仅当没有$s$满足$s_n\mapsto s$，即$s_n$是卡住的。
- 一个转换序列是完备的(complete)当且仅当它是最大的且$s_n\ final$。

判断$s\downarrow$表示有一个从$s$开始的完备转换序列，也就是说存在$s'\ final$满足$s\mapsto^* s'$。

其中，$s\mapsto^* s'$转换判断的任意次迭代由以下规则归纳定义：

$$
\begin{aligned}
&\frac{}{s\mapsto^* s}\\
&\frac{s\mapsto s'\quad s'\mapsto^* s''}{s\mapsto^* s''}\\
\end{aligned}
$$

> 该迭代是自反且传递的。

### 结构化动态语义

定义判断$e\ eval$，表示$e$是一个值。

$$
\begin{aligned}
&\frac{}{num[n]\ eval}\\
&\frac{}{str[s]\ eval}\\
\end{aligned}
$$

??? note "状态之间的转换判断$e\mapsto e'$的规则"
    转换的路径是确定的（即串行计算），在$plus$中一定先完成$e_1$的计算再算$e_2$,最后执行$plus$。

    $$
    \begin{aligned}
    &\frac{n_1+n_2=n}{plus(num[n_1];num[n_2])\mapsto num[n]}\\\\
    &\frac{e_1\mapsto e_1'}{plus(e_1;e_2)\mapsto plus(e_1';e_2)}\\\\
    &\frac{e_1\ val\quad e_2\mapsto e_2'}{plus(e_1;e_2)\mapsto plus(e_1;e_2')}\\\\
    &\frac{s_1\text{\textasciicircum}s_2=s\ str}{cat(str[s_1];str[s_2])\mapsto str[s]}\\\\
    &\frac{e_1\mapsto e_1'}{cat(e_1;e_2)\mapsto cat(e_1';e_2)}\\\\
    &\frac{e_1\ val\quad e_2\mapsto e_2'}{cat(e_1;e_2)\mapsto cat(e_1;e_2')}\\\\
    &[\frac{e_1\mapsto e_1'}{let(e_1;x.e_2)\mapsto let(e_1';x.e_2)}]\\\\
    &\frac{[e_1\ val]}{let(e_1;x.e_2)\mapsto [e_1/x]e_2}\\\\
    \end{aligned}
    $$

    ???+ example "🌰"
        $$
        \begin{aligned}
        &let(plus(num[1];num[2]);x.plus(x;num[3]);num[4])\\
        &\mapsto let(num[3];x.plus(x;num[3]);num[4])\\
        &\mapsto plus(plus(num[3];num[3]);num[4])\\
        &\mapsto plus(num[6];num[4])\\
        &\mapsto num[10]\\
        \end{aligned}
        $$

#### 值的终结性

引理5.2(Finality of Values):

不存在表达式$e$，使得对某个$e'$，$e\ val$和$e\mapsto e'$同时成立。

#### 确定性

引理5.3(Determinacy):

如果$e\mapsto e'$且$e\mapsto e''$，那么$e'$和$e''$$\alpha$等价。

### 上下文动态语义

使用求值上下文(evaluation context)的方法对定位下一条的指令的过程加以形式化。

判断$\varepsilon\ ectxt$（E是一个求值上下文）确定下一条要执行的指令的位置。下一条指令用"洞"表示，记做$\circ$。

$$
\begin{aligned}
&\frac{}{\circ\ ectxt}\\
&\frac{\varepsilon_1\ ectxt}{plus(\varepsilon_1;e_2)\ ectxt}\\
&\frac{e_1\ val\quad\varepsilon_2\ ectxt}{plus(e_1;\varepsilon_2)\ ectxt}\\
\end{aligned}
$$

## Function Definitions and Values

引入语言**ED**作为**E**的扩展，增加了函数定义和函数应用。

### 一阶函数

$$
\begin{aligned}
Exp\quad e\quad ::=\quad &apply\{f\}(e)&\quad &表示f(e)\\
&fun\{\tau_1;\tau_2\}(x_1.e_2;f.e)&\quad &表示fun\ f(x_1:\tau_1):\tau_2=e_2\ in\ e\\
\end{aligned}
$$

把$e$中的函数名$f$绑定到模式$x_1.e_2$，该模式具有参数$x_1$和定义$e_2$，规定了在应用$f$时实例化的模式。

??? tip "理解"
    函数名为$f$，形参为$x_1$，形参类型为$\tau_1$，返回值类型为$\tau_2$，函数体为$e_2$，实参为$e$。

一阶函数的定义域和值域只能是$num$和$str$。

#### 函数代换

记作$[[x.e/f]]e'$，定义如下：

$$
\frac{}{[[x.e/f]]apply\{f\}(e')=let([[x.e/f]]e';x.e)}
$$

??? quote "解释（原文）"
    At application sites to $f$ with argument $e′$, function substitution yields a $let$ expression that binds $x$ to the result of expanding any further applications to $f$ within $e′$.

在$f(e')$处，函数代换生成一个$let$表达式，该表达式将$x$绑定到在$e'$中任意进一步应用$f$的结果。

相当于$let\ x\ be\ [[x.e/f]]e'\ in\ e$，但由于$e'$中可能仍有$f$（递归），因此需要用$let$表达式将$x$绑定到$[[x.e/f]]e'$。

#### 静态语义

$$
\begin{aligned}
&\frac{\Gamma ,x_1:\tau_1\vdash e_2:\tau_2\quad \Gamma ,f(\tau_1):\tau_2\vdash e:\tau}{\Gamma \vdash fun\{\tau_1;\tau_2\}(x_1.e_2;f.e):\tau}\\\\
&\frac{\Gamma \vdash f(\tau_1):\tau_2\quad \Gamma \vdash e:\tau_1}{\Gamma \vdash apply\{f\}(e):\tau_2}\\\\
\end{aligned}
$$

#### 动态语义

$$
\frac{}{fun\{\tau_1;\tau_2\}(x_1.e_2;f.e)\longmapsto [[x_1.e_2/f]]e}
$$

### 高阶函数

函数的参数或返回类型也是函数，即让函数成为一种数据类型。

引入**EF**语言，增加一种新的类型`arr`

$$
\begin{aligned}
Typ\quad \tau\quad ::=\quad &arr(\tau_1;\tau_2)\\
Exp\quad e\quad ::=\quad &lam\{\tau\}(x.e)&\quad &表示\lambda (x:\tau)e\\
&ap(e_1;e_2)&\quad &表示e_1(e_2)\\
\end{aligned}
$$

### 静态语义

$$
\begin{aligned}
&\frac{\Gamma ,x:\tau_1\vdash e:\tau_2}{\Gamma \vdash lam\{\tau_1\}(x.e):arr(\tau_1;\tau_2)}\\\\
&\frac{\Gamma \vdash e_1:arr(\tau_2;\tau)\quad\Gamma \vdash e_2:\tau_2}{\Gamma \vdash ap(e_1;e_2):\tau}\\\\
\end{aligned}
$$

#### 反转

引理8.2：

假设$\Gamma \vdash e:\tau$

1. 如果$e=lam\{\tau_1\}(x.e_2)$，那么$\tau=arr(\tau_1;\tau_2)$且$\Gamma ,x:\tau_1\vdash e_2:\tau_2$
2. 如果$e=ap(e_1;e_2)$，那么存在$\tau_2$使得$\Gamma \vdash e_1:arr(\tau_2;\tau)$且$\Gamma \vdash e_2:\tau_2$

#### 代换

引理8.3：

如果$\Gamma ,x:\tau\vdash e':\tau'$且$\Gamma \vdash e:\tau$，那么$\Gamma \vdash [e/x]e':\tau'$

#### 保持性

定理8.4：

如果$e:\tau$且$e\mapsto e'$，那么$e':\tau$

#### 范式

引理8.5：

如果$e:arr(\tau_1;\tau_2)$，且$e\ val$，那么对满足$x:\tau_1\vdash\ e_2:\tau_2$的变量$x$和表达式$e_2$，有$e=\lambda(x:\tau_1)e_2$

#### 进展性

引理8.6：

如果$e:\tau$，则要么$e\ val$，要么存在$e'$使得$e\mapsto e'$

### 动态作用域

以定义时的上下文作为运行上下文的为静态作用域。

以执行时的上下文作为运行上下文的为动态作用域。

例如以下代码

```
x <- 1
f <- function(a) x + a
g <- function() {
    x <- 2
    f(0)
}
g(0)
```

在静态作用域中，`g(0)`的结果为`1`，而在动态作用域中，`g(0)`的结果为`2`。

（常见的编程语言均为静态作用域，即使是解释型语言Python也是静态作用域）

## System T of Higher-Order Recursion

### 全函数与计算的可终止性

#### 部分函数

如果$f$是从A到B的二元关系，且$\forall a \in A$，有$f(a) = \empty$或$\{b\}$。

当$f(a)=\{b\}$时，记作$f(a)\downarrow$。（即表示$f(a)$非空）

#### 全函数

如果$\forall a \in A$，都有$f(a)\downarrow$，则称$f$是A上的全函数，可记为$f:A\rightarrow B$。

#### 非终止性

定义特殊元素$\bot$，称为bottom，表示非终止性。

一个函数称为严格的(strict)，如果接受一个非终止的输入表达式函数的计算也不会终止，即$f(\bot)=\bot$；否则称为非严格的(non-strict)。

### 合成运算与原始递归运算

#### 合成运算

设$f$时$n$元部分函数，$g_1,g_2,...,g_k$是$k$个$n$元部分函数，令：

$h(x_1,...,x_n)=f(g_1(x_1,...,x_n),...,g_k(x_1,...,x_n))$

则称$h$是由$f$和$g_1,...,g_k$经过合成得到的。

#### 原始递归运算

设$f$是一个$n$元全函数，$g$是$n+2$元全函数，令，

$h(x_1,...,x_n,0)=f(x_1,...,x_n)$

$h(x_1,...,x_n,t+1)=g(t,h(x_1,...,x_n,t),x_1,...,x_n)$

则称$h$是由$f$和$g$经过原始递归运算得到的。

### System T: $G\"{o}del’s\ T$

**E**是以$num$和$str$为类型的语言，

**T**是以$nat$为类型的语言（原始递归）。

#### 语法

$$
\begin{aligned}
Typ\quad \tau\quad ::=\quad &nat\\
&arr(\tau_1;\tau_2)\\
Exp\quad e\quad ::=\quad &x&\quad &&&变量\\
&z&\quad &&&零\\
&s(e)&\quad &&&后继\\
&rec\{e_0;x.y.e_1\}(e)&\quad &rec\ e\{z\hookrightarrow e_0 | s(x)\ with\ y \hookrightarrow e_1\}&\quad &递归\\
&lam\{\tau\}(x.e)&\quad &\lambda(x:\tau)e&\quad &抽象\\
&ap(e_1;e_2)&\quad &e_1(e_2)& \quad &应用\\
\end{aligned}
$$

???+ tip "递归算子理解"
    其中，对递归算子(recursive)作以下说明：

    $\hookrightarrow$称为`lead to`。$rec\{e_0;x.y.e_1\}(e)$可理解为一个分段函数：

    $$
    e = \begin{cases}
    e_0 &\text{if } e==z \\
    e_1 &\text{if } e==s(x)\ with\ y
    \end{cases}
    $$

    运算时，$rec\{e_0;x.y.e_1\}(e)$表示从$e_0$开始，对变换$x.y.e_1$进行$e$轮迭代（每一轮$x-1$，直到$x$为$z$）。$x$是前驱，$y$是经历迭代的结果。

另一种迭代算子$iter\{e_0;y.e_1\}(e)$，这里没有绑定变量$x$，递归调用的结果绑定到$e_1$中的$y$。

#### 静态语义

$$
\begin{aligned}
&\frac{}{\Gamma ,x:\tau\vdash x:\tau}\\\\
&\frac{}{\Gamma \vdash z:nat}\\\\
&\frac{\Gamma \vdash e:nat}{\Gamma \vdash s(e):nat}\\\\
&\frac{\Gamma \vdash e:nat\quad\Gamma \vdash e_0:\tau\quad\Gamma ,x:nat,y:\tau\vdash e_1:\tau}{\Gamma \vdash rec\{e_0;x.y.e_1\}(e):\tau}\\\\
&\frac{\Gamma ,x:\tau_1\vdash e:\tau_2}{\Gamma \vdash lam\{\tau_1\}(x.e):arr(\tau_1;\tau_2)}\\\\
&\frac{\Gamma \vdash e_1:arr(\tau_2;\tau)\quad\Gamma \vdash e_2:\tau_2}{\Gamma \vdash ap(e_1;e_2):\tau}\\\\
\end{aligned}
$$

##### 代换

引理9.1：

如果$\Gamma \vdash e:\tau$并且$\Gamma, x:\tau \vdash e':\tau'$，则$\Gamma \vdash [e/x]e':\tau'$。

#### 动态语义

##### 闭值

其中`[]`表示eager解释

$$
\begin{aligned}
&\frac{}{z\ val}\\\\
&\frac{[e\ val]}{s(e)\ val}\\\\
&\frac{}{lam\{\tau\}(x.e)\ val}
\end{aligned}
$$

##### 转换规则

$$
\begin{aligned}
&\left [\frac{e\mapsto e'}{s(e)\mapsto s(e')}\right ]\\\\
&\frac{e_1\mapsto e_1'}{ap(e_1;e_2)\mapsto (e_1';e_2)}\\\\
&\left [\frac{e_1\ val\quad e_2 \mapsto e_2'}{ap(e_1;e_2)\mapsto ap(e_1;e_2'))}\right ]\\\\
&\frac{[e_2\ val]}{ap(lam\{\tau\}(x.e);e_2)\mapsto [e_2/x]e}\\\\
&\frac{e\mapsto e'}{rec\{e_0;x.y.e_1\}(e)\mapsto rec{e_0;x.y.e_1}(e')}\\\\
&\frac{}{rec\{e_0;x.y.e_1\}(z)\mapsto e_0}\\\\
&\frac{s(e)\ val}{rec\{e_0;x.y.e_1\}(s(e))\mapsto [e,rec\{e_0;x.y.e_1\}(e)/x,y]e_1}\\\\
\end{aligned}
$$

最后一条即为递归计算一层。

###### 范式

引理9.2：

如果$e:\tau$且$e\ val$，则

1. 如果$\tau = nat$，则$e=z$，或者存在$e'$使得$e=s(e')$成立
2. 如果$\tau=\tau_1\rightarrow\tau_2$，则存在$\tau_2$使得$e=\lambda(x:\tau_1)e_2$成立

###### 安全性

1. 如果$e:\tau$且$e\mapsto e'$，则$e':\tau$
2. 如果$e:\tau$则要么$e\ val$，要么存在$e'$使得$e\mapsto e'$成立

## Product Types

### 基本定义

#### 二元积

- 二元积(binary product)：值的有序对(ordered pairs)。
- 消去形式：投影(project)，选择有序对的第一个或第二个分量。
- 空积(nullary product)：唯一的不包含任何值的空元组，没有对应的消去形式。
- 惰性(lazy)动态语义：无论分量是否是值，有序对都是值。
- 急性(eager)动态语义：分量都是值时，有序对才是值。

#### 有限积

二元积以上称为有限积(infinite product)。

- $<\tau_i>_{i\in I}$，其中$I$是索引的有限集。
- 每个有限积类型的元素都是$I$-索引的元组，每个元组的第$i$个分量的类型为$\tau_i(i\in I)$
- 分量可以用$I$-索引的投影来访问。
- n元组：用$I=\{0,...,n-1\}$索引
- 记录(record)：用有限符号集索引
- 有限积同样可做急性和惰性解释。

### 空积和二元积

#### 语法

$$
\begin{aligned}
Typ\quad \tau\quad ::=\quad &unit\quad& &unit\quad& &空积&\\
&prod(\tau_1;\tau_2)\quad& &\tau_1\times\tau_2\quad& &二元积&\\
Exp\quad e\quad ::=\quad &triv\quad& &<>\quad& &空元组&\\
&pair(e_1;e_2)\quad& &<e_1,e_2>\quad& &有序对&\\
&pr[l](e)\quad& &e\cdot l\quad& &左投影&\\
&pr[r](e)\quad& &e\cdot r\quad& &右投影&\\
\end{aligned}
$$

#### 静态语义

$$
\begin{aligned}
&\frac{}{\Gamma \vdash <>:unit}\\\\
&\frac{\Gamma \vdash e_1:\tau_1\quad\Gamma \vdash e_2:\tau_2}{\Gamma \vdash <e_1;e_2>:\tau_1\times tau_2}\\\\
&\frac{\Gamma \vdash e:tau_1\times\tau_2}{\Gamma \vdash e\cdot l:\tau_1}\\\\
&\frac{\Gamma \vdash e:tau_1\times\tau_2}{\Gamma \vdash e\cdot r:\tau_2}\\\\
\end{aligned}
$$

#### 动态语义

$$
\begin{aligned}
&\frac{}{<>\ val}\\\\
&\frac{[e_1\ val]\quad [e_2\ val]}{<e_1;e_2>\ val}\\\\
&\left [\frac{e_1\mapsto e_1'}{<e_1;e_2>\mapsto <e_1';e_2>}\right ]\\\\
&\left [\frac{e_1\ val\quad e_2\mapsto e_2'}{<e_1;e_2>\mapsto <e_1;e_2'>}\right ]\\\\
&\frac{e\mapsto e'}{e\cdot l\mapsto e'\cdot l}\\\\
&\frac{e\mapsto e'}{e\cdot r\mapsto e'\cdot r}\\\\
&\frac{[e_1\ val]\quad [e_2\ val]}{<e_1;e_2>\cdot l\mapsto e_1}\\\\
&\frac{[e_1\ val]\quad [e_2\ val]}{<e_1;e_2>\cdot r\mapsto e_2}\\\\
\end{aligned}
$$

#### 安全性

定理10.1：

1. 如果$e:\tau$且$e\mapsto e'$，则$e':\tau$；
2. 如果$e:\tau$，那么必有$e\ val$或存在$e'$满足$e\mapsto e'$。

### 原始互递归

可以简化原始递归，使得传递给后继分支的只有前驱的递归结果而没有前驱本身$iter\{e_0;y.e_1\}(e)$，于是$rec\{e_0;x.y.e_1\}(e)$可以定义为$e'\cdot r$，其中$e'$是表达式：
$iter\{<z,e_0>;x'.<s(x'\cdot l),[x'\cdot r/x]e_1>\}(e)$

???+ example "🌰"
    奇偶函数的递归方程：
    $$
    \begin{aligned}
    &e(0)&&=&1\\\\
    &o(0)&&=&0\\\\
    &e(n+1)&&=&o(n)\\\\
    &o(n+1)&&=&e(n)\\\\
    \end{aligned}
    $$

    首先定义辅助函数$e_{eo}$，其类型为$nat\to (nat\times nat)$，即：
    $\lambda(n:nat)\ iter\ n\{z\hookrightarrow <1,0>|s(b)\hookrightarrow <b\cdot r,b\cdot l>\}$

    于是：

    $$
    \begin{aligned}
    &e_{ev}\triangleq\lambda(n:nat)\ e_{eo}(n)\cdot l\\
    &e_{od}\triangleq\lambda(n:nat)\ e_{eo}(n)\cdot r\\
    \end{aligned}
    $$

### 积类型的PL意义

在大多数常见的计算机语言中，复合数据类型都是积类型。（例如结构体）

## Sum Types

### 空和与二元和

#### 语法

$$
\begin{aligned}
Typ\quad \tau\quad ::=\quad &void\quad& &void\quad& &空和&\\
&sum(\tau_1;\tau_2)\quad& &\tau_1+\tau_2\quad& &二元和&\\
Exp\quad e\quad ::=\quad &abort\{\tau\}(e)\quad& &abort(e)\quad& &中止&\\
&in[l]\{\tau_1;\tau_2\}(e)\quad& &l\cdot e\quad& &左注入&\\
&in[r]\{\tau_1;\tau_2\}(e)\quad& &r\cdot e\quad& &右注入&\\
&case(e;x_1.e_1;x_2.e_2)\quad& &case(e\{l\cdot x_1 \hookrightarrow e_1 | r\cdot x_2 \hookrightarrow e_2\})\quad& &情况分析&\\
\end{aligned}
$$

注入运算$l\cdot e$表示结果$e+e'$，加在左边。

#### 静态语义

懒得抄

#### 动态语义

懒得抄

### 和类型的PL意义

例如枚举类型，其中的值不会同时出现，只能取其一。

!!! warning "attention"
    C语言的union不是和类型，union是对内存空间的复用，可以同时解释为多个类型。事实上union是一种简单数据类型，不是和/积类型。
    和类型只能取一种类型。

### 和类型的应用

#### void和unit

unit有且仅有一个元素`<>`，void没有元素。

#### 布尔类型

$$
\begin{aligned}
Typ\quad \tau\quad ::=\quad &bool\quad& &bool\quad& &布尔类型&\\
Exp\quad e\quad ::=\quad &true\quad& &true\quad& &真&\\
&false\quad& &false\quad& &假&\\
&if(e;e_1;e_2)\quad& &if\ e\ then\ e_1\ else\ e_2\quad& &条件&\\
\end{aligned}
$$

布尔类型可以用二元和与空积来定义：

$$
\begin{aligned}
&bool& &=& &unit+unit&\\
&true& &=& &l\cdot <>&\\
&false& &=& &r\cdot <>&\\
&if\ e\ then\ e_1\ else\ e_2& &=& &case\ e\{l\cdot x_1\hookrightarrow e_1|r\cdot x_2\hookrightarrow e_2\}&\\
\end{aligned}
$$

