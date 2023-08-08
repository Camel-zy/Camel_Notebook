---
title: C大程随性笔记
---

一份C大程的随意笔记

<!--more -->

要是有哪里写错了那就写错了吧 反正考完理论考我也不用记这些东西了(

# enum

关于枚举类型：

## 定义

```c
enum TypeName{ValueName1,ValueName2,...};
```
例如：
```c
enum week{ Mon = 1, Tues = 2, Wed = 3, Thurs = 4, Fri = 5, Sat = 6, Sun = 7 } day;  //内部用逗号分隔 最后为空 不是分号
day = Mon;
```
对于没有赋值的元素，其值为**前一元素加1** 初值为0

例如

```c
enum Test {x, y = 4, z};
x == 0
z == 5    
```

# typedef

这边关于typedef再多记一点：对于数组的typedef

```c
typedef int vector[10];
typedef int matrix[5][5];
```

一个挺好的理解办法：去掉typedef就是变量定义。

# 联合（共同）体

结构体分配足够的空间存储其所有成员，而共用体分配空间仅存储最大的成员。

例如

```c
union S {
  char a;
  int b[4];
  double c;
} x;

sizeof(x) == 16
```

有一个“字节对齐问题”

例如

```c
union U {
    char s[9];
    int n;
    double d;
} x;

sizeof(x) == 16

```

其中，s占9字节，n占4字节，d占8字节。但9既不能被4(int)整除，也不能被8(double)整除，因此补充字节到16。

union大小必须满足：1)大小足够容纳最宽的成员；2)大小能被其包含的所有基本数据类型的大小所整除。

# 指针

## 普通指针
函数形参有地址时，注意不要改变地址，只改变该地址指向的值以实现双向传输。
放个错题：

```c
void ArrayCreate(double **array, int size) {
    array = (double **)malloc(sizeof(double *) * size); //wrong

    *array = (double *)malloc(sizeof(double) * size); //right
}
```
第一种改变会导致该参数的改变无法传出。(这玩意坑了我近两小时)

静态变量地址可返回使用（返回的指针指向的内存空间没有释放，在主调函数能够继续使用）

```c
int * f4() {
  static int d[10];
  return d;
}
```

如果函数的返回类型是指针，则可以返回0。（NULL == 0）

使用前记得分配空间。

## 字符串指针

两个字符串直接比较 比较的是首地址

```c
char *str = "hello";
str[0] = 'H';
```
这种写法不合法，str指针指向的是常量字符串，不允许修改，会导致**段错误**。

```c
char *color[5];
for (int i = 0; i < 5; i++)
    scanf("%s", color[i]);
```
这种写法亦不合法。段错误。

字符串数组的数组名不可修改 例如：

```c
char color[ ][7] = {"red", "blue", "yellow", "green", "black"};
char * tmp = color[0]; 
color[0] = color[4]; 
color[4] = tmp; 
```

**编译错误！**

字符串处理相关函数(string.h库)

| 函数名                                         | 功能                                                         |
| :--------------------------------------------- | ------------------------------------------------------------ |
| int strcmp(const char *str1, const char *str2) | 如果返回值小于 0，则表示 str1 小于 str2。                    |
| char *strcat(char *dest, const char *src)      | 把 **src** 所指向的字符串追加到 **dest** 所指向的字符串的结尾（dest会被修改）。返回一个指向最终的目标字符串 dest 的指针 |
| char *strcpy(char *dest, const char *src)      | 把 **src** 所指向的字符串复制到 **dest**（dest会被修改），返回一个指向最终的目标字符串 dest 的指针。 |

## 函数指针

顺带提一句，C语言不允许函数嵌套定义 即一个函数定义中**不可以**完整地包含另一个函数的定义。  

### 定义
函数返回类型(*指针变量名)(函数形参类型列表);

函数名代表函数的入口地址(常量)，是一个指针。通过函数指针可以调用函数，它也可以作为函数的参数。

放个栗子

```c
#include <stdio.h>
int add(int num1, int num2) {
    return num1 + num2;
}
​
​
int sub(int num1, int num2) {
    return num1 - num2;
}
​
​
int calculate(int (*fp)(int,int), int num1, int num2) {
    return (*fp)(num1,num2);
}
​
​
int main() {
    printf("3+5=%d\n", calculate(add,3,5));
    printf("3-5=%d\n", calculate(sub,3,5));
}
```

## 数组指针

```c
int *p[4];
int (*p)[4];
```
前者为指针数组，p先与```[]```结合代表其为数组。

后者为数组指针，p先与```*```结合代表其为指针，指向的为```[4]```数组。

一个小练习：

变量定义```int *(*f[3])();```中`f`是（ ）   D

A.一组指向返回int的函数的二级指针数组

B.一个指向一组返回int *的函数数组的指针

C.一个返回指向一组int *数组的指针的函数

D.一个返回int *的函数指针的数组

E.一个指向返回int *数组的函数的指针

F.一个指向一组返回int的函数指针的数组的指针

## 二维数组与指针
放个栗子

对于以下程序，能够正确表示二维数组 t 的元素地址的表达式是(C)
```c
int main(void) {
    int k, t[3][2], *pt[3];
        
    for ( k = 0; k < 3; k++) {
        pt[k] = t[k];
    }
                
    return 0;
}

A. &t[3][2]
B. *pt[0]
C. *(pt+1)
D. &pt[2]
```

## 指针形参实参匹配
二维数组名不是二级指针,而是一个数组指针。

即二维数组的每个元素都是一个一维数组。

放个栗子 感觉这很难说 也很无聊

```c
下列哪些实参(左边)和形参(右边)能够匹配的是（DEFH）。
A. double a[10][10]	double **b
B. double a[10][10] double b[][]
C. double a[10][10] double b[10][]
D. double a[10][10] double b[][10]
E. double a[10][10] double (*b)[10]
F. double *a[10] double **b
G. double (*a)[10] double **b
H. double **a double *b[]
I. double **a double b[][]
```

另外注意有的时候需要对指针强制转换  比如void *很多时候需要转换。

## 文件指针

> **错误**：文件指针指向文件缓冲区中文件数据的存取位置

> 文件指针实际上是指向一个结构体类型的指针，包含有诸如：缓冲区的地址在缓冲区中当前存取的字符的位置、对文件是“读”还是“写”、是否出错、是否已经遇到文件结束标志等信息。

### fopen

```c
FILE *fopen(const char *filename, const char *mode)
```

返回一个 FILE 指针。否则返回 **NULL**。

文件打开方式参数表：

| 参数 | 含义                       |
| ---- | -------------------------- |
| r    | 只读（文件必须存在）       |
| w    | 只写（若文件不存在则新建） |
| a    | 追写                       |
| r+   | 读写（文件必须存在）       |
| w+   | 读写（若文件不存在则新建） |
| a+   | 读/追写                    |

加个b就是相应的二进制文件操作 懒得再赘述。

###  feof

```c
int feof(FILE *stream)
```

文件结束：返回非0值；文件未结束：返回0值。

### fputc

```c
int fputc(int char, FILE *stream)
```

如果没有发生错误，则返回被写入的字符。如果发生错误，则返回 EOF。

### rewind

```c
void rewind(FILE *stream)
```

移到文件首。

### ftell

```c
long int ftell(FILE *stream)
```

返回位置标识符的当前值。如果发生错误，则返回 -1L。

### fseek

```c
int fseek(FILE *stream, long int offset, int whence)
```

参数：

- **stream**  -- 这是指向 FILE 对象的指针，该 FILE 对象标识了流。
- **offset** -- 这是相对 whence 的偏移量，以字节为单位。
- **whence** -- 这是表示开始添加偏移 offset 的位置。它一般指定为下列常量之一：

| 常量     | 描述               |
| -------- | ------------------ |
| SEEK_SET | 文件的开头         |
| SEEK_CUR | 文件指针的当前位置 |
| SEEK_END | 文件的末尾         |

### ferror

```c
int ferror(FILE *stream)
```

如果设置了与流关联的错误标识符，该函数返回一个非零值，否则返回一个零值。

### fread

```c
size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream)
```

成功读取的元素总数会以 size_t 对象返回。

参数：

- **ptr**  -- 这是指向带有最小尺寸 *size\*nmemb* 字节的内存块的指针。
- **size** -- 这是要读取的每个元素的大小，以字节为单位。
- **nmemb**  -- 这是元素的个数，每个元素的大小为 size 字节。
- **stream** -- 这是指向 FILE 对象的指针，该 FILE 对象指定了一个输入流。

### fwrite

```c
size_t fwrite(const void *ptr, size_t size, size_t nmemb, FILE *stream)
```

参数：

- **ptr**  -- 这是指向要被写入的元素数组的指针。
- **size**  -- 这是要被写入的每个元素的大小，以字节为单位。
- **nmemb**  -- 这是元素的个数，每个元素的大小为 size 字节。
- **stream**  -- 这是指向 FILE 对象的指针，该 FILE 对象指定了一个输出流。

如果成功，该函数返回一个 size_t 对象，表示元素的总数。

# 命令行参数

## main
```c
int main(int argc, char *argv[]);
```
argc为参数数量，argv为参数列表。
**文件名本身也算一个参数** 所以argv[0]就是文件名

# 链表

在单向链表中，头指针中存放的不是头结点的内容，头指针指向头结点。（都是些什么无聊的概念）

# 递归

C语言规定，程序中各函数之间既允许直接递归调用也允许间接递归调用。

# 文件包含与工程文件

放一道题：

```c
在Dev-C++中创建工程P1，增加main函数所在的project.c，该工程需要使用libgraphics的linkedlist类库，关于linkedlist使用说明正确的有（ ）。	ADF
    
A.通过文件包含将project.c和linkedlist.c连接成一个完整的可执行程序时，可以在project.c中#include "linkedlist.c"
B.通过文件包含将project.c和linkedlist.c连接成一个完整的可执行程序时，可以在project.c中#include "linkedlist.c"，同时工程P1增加linkedlist.h和linkedlist.c
C.通过工程文件将project.c和linkedlist.c连接成一个完整的可执行程序时，可以在project.c中#include "linkedlist.h"
D.通过工程文件将project.c和linkedlist.c连接成一个完整的可执行程序时，可以在project.c中#include "linkedlist.h"，同时工程P1增加linkedlist.h和linkedlist.c
E.通过文件包含将project.c和linkedlist.c连接成一个完整的可执行程序时，编译生成project.o和linkedlist.o，连接生成P1.exe
F.通过工程文件将project.c和linkedlist.c连接成一个完整的可执行程序时，编译生成project.o和linkedlist.o，连接生成P1.exe

B.重复包含
```

文件包含最终是单文件编译，相当于直接把包含的文件代码复制过来

而工程文件是需要编译连接

# 有的没的函数

## qsort

```c
void qsort(void *base, size_t nitems, size_t size, int (*compar)(const void *, const void*))
```

参数：

-  **base**  -- 指向要排序的数组的第一个元素的指针。
-  **nitems**  -- 由 base 指向的数组中元素的个数。
-  **size**  -- 数组中每个元素的大小，以字节为单位。
-  **compar**  -- 用来比较两个元素的函数。

## sprintf

```c
int sprintf(char *str, const char *format, ...)
```

函数格式化输出成功，返回输出的字符数，**不**包括字符串结束符`'\0'`，失败则返回一个负整数

形参str是指向输出缓冲区的指针，函数结束后，格式化的输出结果在缓冲区中，形参format的使用用法与函数printf的形参format相同

## sscanf

```c
int sscanf(const char *str, const char *format, ...)
```

形参s指定从中读取数据的字符串，形参format格式字符串，指定了输入的格式，并按照格式说明符解析输入对应位置的信息，并存储于可变参数列表中对应的指针所指位置，使用格式控制字符%n可以获取输入数据所消耗的字符数

格式化读取成功，返回读取的数据的个数（不包括%n哦），返回值为0表示没有将任何字段赋值，如果读入数据时遇到了“字符串结束”则返回EOF

例如：

```c
char buffer[] = "abc 5 6.7 cde";
int a, m, n;
char str[4];
n = sscanf(buffer, "%s %d%n", str, &a, &m);

// m = 5, n = 2
```

## 一些已知宏

NULL 0

EOF -1

# 奇奇怪怪的东西

- 正确：一般不能用任何一个文本编辑器打开二进制文件进行阅读。  

- malloc内存分配时记得强制转换  理论题里认为需要强制转换（虽然C语言允许省略 但C++不大行

## register变量

寄存器变量，其存储于寄存器，速度更快。

>  只有局部变量和形式参数才能定义为寄存器变量。
>
> 局部静态变量不能定义为寄存器变量，因为一个变量只能声明为一种存储类别。
>
> 寄存器的长度一般和机器的字长一致，所以，只有较短的类型如int、char、short等才适合定义为寄存器变量，诸如double等较大的类型，不推荐将其定义为寄存器类型。
>
> CPU的寄存器数目有限，因此，即使定义了寄存器变量，编译器可能并不真正为其分配寄存器，而是将其当做普通的auto变量来对待，为其分配栈内存。当然，有些优秀的编译器，能自动识别使用频繁的变量，如循环控制变量等，在有可用的寄存器时，即使没有使用 register 关键字，也自动为其分配寄存器，无须由程序员来指定。

# 参考

后面的函数相关笔记来自[菜鸟教程](https://www.runoob.com/cprogramming/c-tutorial.html)
