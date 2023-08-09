---
title: Python3学习笔记
---

# Python3学习笔记

## 配置环境时遇到的乱七八糟的问题

### no module named pip

在更新pip出错后再次使用pip显示如上错误

解决：

```bash
python -m ensurepip
python -m pip install --upgrade pip
```

[参考](https://blog.csdn.net/wwangfabei1989/article/details/80107147)

### 换清华源

```bash
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

## 基础语法

### 零零散散

- 标识符对大小写敏感

- 保留字：Python 的标准库提供了一个 keyword 模块，可以输出当前版本的所有关键字


```python
import keyword

keyword.kwlist
```

- 注释以#开头 多行注释亦可使用三引号'''或"""

- Python使用缩进表示代码块，不需要C系的大括号，但缩进的空格数需保持一致

- 可在句末加反斜杠\实现多行语句

- Python 可以在同一行中使用多条语句，语句之间使用分号```;```分割


### 类型

1. int 整型 

   - python3没有long 只有这一种整型

   - 允许数字中间用```_```分隔，如```10_000_000_000```和```10000000000```是完全一样的

   - 运算包括以下： ```+``` ```-``` ```*``` ```/``` ```//```（整除） ```%``` ```**```（次方）

2. bool 布尔

   - True和False

3. float 浮点

   - 普通小数和科学计数法 如```1.23``` ```1.2e9```

4. complex 复数

   - 复数由实数部分和虚数部分构成，可以用```a + bj```,或者```complex(a,b)```表示， 复数的实部a和虚部b都是浮点型

5. string 字符串

   - 字符串可用单引号或双引号表示（这两种引号完全相同）

   - 使用三引号可表示多行字符串

   - 若不希望转义，可在字符串前加```r``` 如 ```r"this is a line with \n"```

   - 字符串可以用```+```运算符连接在一起，用```*```运算符重复

   - 字符串内容不可更改

   - 字符串的截取的语法格式如下：```变量[头下标:尾下标:步长]```

   - 字符串有两种索引方式，从左往右以 0 开始，从右往左以 -1 开始 具体如下图

   ![字符串索引示意图](https://static.runoob.com/wp-content/uploads/123456-20200923-1.svg)

   - Python 没有单独的字符类型，一个字符就是长度为 1 的字符串。

   - 字符串格式化方式与C语言类似

   - 关于字符串编码问题可以看[这里](https://www.liaoxuefeng.com/wiki/1016959663602400/1017075323632896)

   字符串演示见下：


```python
str = "Camel"

print(str)
print(str[1:-1], "\n")

print(str[3])
print(str[3:], "\n")

print(str * 2)
print(str + "Cat", "\n")

## 字符串格式化主要有以下三种方法
## 直接使用%
print("%d%%" % 7)  ## 两个%表示%

## 使用format()
print("{0}+{1}={2}".format(1, 2, 1 + 2))

## 使用f-string
r = 2
s = 3.14 * r**2
print(f"半径为{r}的圆面积为{s:.2f}")
```

    Camel
    ame 
    
    e
    el 
    
    CamelCamel
    CamelCat 
    
    7%
    1+2=3
    半径为2的圆面积为12.56


### 变量

Python 中的变量不需要声明。每个变量在使用前都必须赋值，变量赋值以后该变量才会被创建。

在 Python 中，变量就是变量，它没有类型，我们所说的"类型"是变量所指的内存中对象的类型。

允许同时为多个变量赋值


```python
a, b, c, d = 1, 2.5, True, "string"
```

可以使用type()查看变量类型


```python
print(type(a), type(b), type(c), type(d))
```

可以使用isinstance()判断是否为子类型


```python
print(isinstance(a, int), isinstance(b, int), isinstance(c, int))
```

### import

在 python 用```import```或者```from...import```来导入相应的模块。

将整个模块(somemodule)导入，格式为：```import somemodule```

从某个模块中导入某个函数,格式为：```from somemodule import somefunction```

从某个模块中导入多个函数,格式为：```from somemodule import firstfunc, secondfunc, thirdfunc```

将某个模块中的全部函数导入，格式为：```from somemodule import *```

### print

print 默认输出是换行的，如果要实现不换行需要在变量末尾加上```end=""```

## 结构类型

### 列表（List）

列表可以完成大多数集合类的数据结构实现。列表中元素的类型可以不相同，它支持数字，字符串，亦支持列表嵌套

列表支持类似字符串的切片和索引，且支持改变列表中的元素

列表可使用append()追加元素，insert()插入元素，pop()删除元素

举个栗子


```python
list = [1, True, "hello"]
print("初始",list)
list=list[-1::-1]
print("反转",list)

list.append(2.56)
print("追加",list)

list.insert(1,"插入")
print("插入",list)

list.pop(1)
print("删除",list)

list[1]=False
print("更改",list)
print(f"长度：{len(list)}")
```

    初始 [1, True, 'hello']
    反转 ['hello', True, 1]
    追加 ['hello', True, 1, 2.56]
    插入 ['hello', '插入', True, 1, 2.56]
    删除 ['hello', True, 1, 2.56]
    更改 ['hello', False, 1, 2.56]
    长度：4


### 元组（Tuple）

元组和列表类似，但其中的元素不允许修改

因此代码安全性更强

注意下下面的空元组和单元素元组即可


```python
turple = () ## 空元组
turple = (1,) ## 单元素元组 如果没有逗号的话就是普通变量定义
```

### 字典（Dictionary）

键值对

键必须是唯一的，但值则不必。

值可以取任何数据类型，但键必须是不可变的，如字符串，数字。


```python
## 使用大括号 {} 来创建空字典
emptyDict = {}

## 打印字典
print(emptyDict)

## 查看字典的数量
print("Length:", len(emptyDict))

## 查看类型
print(type(emptyDict))

mydict = {"name": "camel", "date": "2022.06"}

## 修改字典
mydict["name"] = "Camel"
mydict["new"] = 1

## 删除字典
del mydict["new"]
mydict.clear()
del mydict
```

    {}
    Length: 0
    <class 'dict'>


### 集合（Set）

可以使用大括号```{ }```或者```set()```函数创建集合，注意：创建一个空集合必须用```set()```而不是```{ }```，因为```{ }```是用来创建一个空字典。 


```python
myset = {"a", "a"}  ## 自动去重

print("a" in myset)

bset = set("b")
myset |= bset ## 支持- | & ^运算

myset.add("b")
myset.remove("b")
len(myset),myset
```

    True





    (1, {'a'})



## 迭代器和生成器

迭代器对象从集合的第一个元素开始访问，直到所有的元素被访问完结束。迭代器只能往前不会后退。

迭代器有两个基本的方法：iter() 和 next() ，即创建迭代器和访问下一个元素。

---

在 Python 中，使用了 yield 的函数被称为生成器（generator）。

跟普通函数不同的是，生成器是一个返回迭代器的函数，只能用于迭代操作，更简单点理解生成器就是一个迭代器。

在调用生成器运行的过程中，每次遇到 yield 时函数会暂停并保存当前所有的运行信息，返回 yield 的值, 并在下一次执行 next() 方法时从当前位置继续运行。


```python
import sys


def fibonacci(n):  ## 生成器函数 - 斐波那契
    a, b, counter = 0, 1, 0
    while True:
        if (counter > n):
            return
        yield a
        a, b = b, a + b
        counter += 1


f = fibonacci(10)  ## f 是一个迭代器，由生成器返回生成

while True:
    try:
        print(next(f), end=" ")
    except StopIteration:
        sys.exit()
```

    0 1 1 2 3 5 8 13 21 34 55 


    ---------------------------------------------------------------------------
    
    StopIteration                             Traceback (most recent call last)
    
    Input In [21], in <cell line: 16>()
         17 try:
    ---> 18     print(next(f), end=" ")
         19 except StopIteration:


    StopIteration: 




    During handling of the above exception, another exception occurred:


    SystemExit                                Traceback (most recent call last)
    
        [... skipping hidden 1 frame]


    Input In [21], in <cell line: 16>()
         19 except StopIteration:
    ---> 20     sys.exit()


    SystemExit: 




    During handling of the above exception, another exception occurred:


    AssertionError                            Traceback (most recent call last)
    
        [... skipping hidden 1 frame]


    File D:\Python\lib\site-packages\IPython\core\interactiveshell.py:1983, in InteractiveShell.showtraceback(self, exc_tuple, filename, tb_offset, exception_only, running_compiled_code)
       1980 if exception_only:
       1981     stb = ['An exception has occurred, use %tb to see '
       1982            'the full traceback.\n']
    -> 1983     stb.extend(self.InteractiveTB.get_exception_only(etype,
       1984                                                      value))
       1985 else:
       1986     try:
       1987         ## Exception classes can customise their traceback - we
       1988         ## use this in IPython.parallel for exceptions occurring
       1989         ## in the engines. This should return a list of strings.


    File D:\Python\lib\site-packages\IPython\core\ultratb.py:585, in ListTB.get_exception_only(self, etype, value)
        577 def get_exception_only(self, etype, value):
        578     """Only print the exception type and message, without a traceback.
        579 
        580     Parameters
       (...)
        583     value : exception value
        584     """
    --> 585     return ListTB.structured_traceback(self, etype, value)


    File D:\Python\lib\site-packages\IPython\core\ultratb.py:443, in ListTB.structured_traceback(self, etype, evalue, etb, tb_offset, context)
        440     chained_exc_ids.add(id(exception[1]))
        441     chained_exceptions_tb_offset = 0
        442     out_list = (
    --> 443         self.structured_traceback(
        444             etype, evalue, (etb, chained_exc_ids),
        445             chained_exceptions_tb_offset, context)
        446         + chained_exception_message
        447         + out_list)
        449 return out_list


    File D:\Python\lib\site-packages\IPython\core\ultratb.py:1118, in AutoFormattedTB.structured_traceback(self, etype, value, tb, tb_offset, number_of_lines_of_context)
       1116 else:
       1117     self.tb = tb
    -> 1118 return FormattedTB.structured_traceback(
       1119     self, etype, value, tb, tb_offset, number_of_lines_of_context)


    File D:\Python\lib\site-packages\IPython\core\ultratb.py:1012, in FormattedTB.structured_traceback(self, etype, value, tb, tb_offset, number_of_lines_of_context)
       1009 mode = self.mode
       1010 if mode in self.verbose_modes:
       1011     ## Verbose modes need a full traceback
    -> 1012     return VerboseTB.structured_traceback(
       1013         self, etype, value, tb, tb_offset, number_of_lines_of_context
       1014     )
       1015 elif mode == 'Minimal':
       1016     return ListTB.get_exception_only(self, etype, value)


    File D:\Python\lib\site-packages\IPython\core\ultratb.py:865, in VerboseTB.structured_traceback(self, etype, evalue, etb, tb_offset, number_of_lines_of_context)
        856 def structured_traceback(
        857     self,
        858     etype: type,
       (...)
        862     number_of_lines_of_context: int = 5,
        863 ):
        864     """Return a nice text document describing the traceback."""
    --> 865     formatted_exception = self.format_exception_as_a_whole(etype, evalue, etb, number_of_lines_of_context,
        866                                                            tb_offset)
        868     colors = self.Colors  ## just a shorthand + quicker name lookup
        869     colorsnormal = colors.Normal  ## used a lot


    File D:\Python\lib\site-packages\IPython\core\ultratb.py:799, in VerboseTB.format_exception_as_a_whole(self, etype, evalue, etb, number_of_lines_of_context, tb_offset)
        796 assert isinstance(tb_offset, int)
        797 head = self.prepare_header(etype, self.long_header)
        798 records = (
    --> 799     self.get_records(etb, number_of_lines_of_context, tb_offset) if etb else []
        800 )
        802 frames = []
        803 skipped = 0


    File D:\Python\lib\site-packages\IPython\core\ultratb.py:854, in VerboseTB.get_records(self, etb, number_of_lines_of_context, tb_offset)
        848     formatter = None
        849 options = stack_data.Options(
        850     before=before,
        851     after=after,
        852     pygments_formatter=formatter,
        853 )
    --> 854 return list(stack_data.FrameInfo.stack_data(etb, options=options))[tb_offset:]


    File D:\Python\lib\site-packages\stack_data\core.py:546, in FrameInfo.stack_data(cls, frame_or_tb, options, collapse_repeated_frames)
        530 @classmethod
        531 def stack_data(
        532         cls,
       (...)
        536         collapse_repeated_frames: bool = True
        537 ) -> Iterator[Union['FrameInfo', RepeatedFrames]]:
        538     """
        539     An iterator of FrameInfo and RepeatedFrames objects representing
        540     a full traceback or stack. Similar consecutive frames are collapsed into RepeatedFrames
       (...)
        544     and optionally an Options object to configure.
        545     """
    --> 546     stack = list(iter_stack(frame_or_tb))
        548     ## Reverse the stack from a frame so that it's in the same order
        549     ## as the order from a traceback, which is the order of a printed
        550     ## traceback when read top to bottom (most recent call last)
        551     if is_frame(frame_or_tb):


    File D:\Python\lib\site-packages\stack_data\utils.py:98, in iter_stack(frame_or_tb)
         96 while frame_or_tb:
         97     yield frame_or_tb
    ---> 98     if is_frame(frame_or_tb):
         99         frame_or_tb = frame_or_tb.f_back
        100     else:


    File D:\Python\lib\site-packages\stack_data\utils.py:91, in is_frame(frame_or_tb)
         90 def is_frame(frame_or_tb: Union[FrameType, TracebackType]) -> bool:
    ---> 91     assert_(isinstance(frame_or_tb, (types.FrameType, types.TracebackType)))
         92     return isinstance(frame_or_tb, (types.FrameType,))


    File D:\Python\lib\site-packages\stack_data\utils.py:172, in assert_(condition, error)
        170 if isinstance(error, str):
        171     error = AssertionError(error)
    --> 172 raise error


    AssertionError: 


## 含树

### 不定长参数

加了星号```*```的参数会以元组(tuple)的形式导入，存放所有未命名的变量参数。

加了两个星号```**```的参数会以字典的形式导入


```python
## 可写函数说明
def printinfo(arg1, *vartuple):
    "打印任何传入的参数"
    print("输出: ")
    print(arg1)
    for var in vartuple:
        print(var)
    return


## 调用printinfo 函数
printinfo(10)
printinfo(70, 60, 50)
```

    输出: 
    10
    输出: 
    70
    60
    50



```python
## 可写函数说明
def printinfo(arg1, **vardict):
    "打印任何传入的参数"
    print("输出: ")
    print(arg1)
    print(vardict)


## 调用printinfo 函数
printinfo(1, a=2, b=3)
```

    输出: 
    1
    {'a': 2, 'b': 3}


### 强制位置参数

Python3.8 新增了一个函数形参语法```/```用来指明函数形参必须使用指定位置参数，不能使用关键字参数的形式。

在以下的例子中，形参```a```和```b```必须使用指定位置参数，```c```或```d```可以是位置形参或关键字形参，而```e```和```f```要求为关键字形参: 


```python
def f(a, b, /, c, d, *, e, f):
    print(a, b, c, d, e, f)
```

## 遍历技巧

在字典中遍历时，关键字和对应的值可以使用 items() 方法同时解读出来

在序列中遍历时，索引位置和对应值可以使用 enumerate() 函数同时得到

同时遍历两个或更多的序列，可以使用 zip() 组合

## 奇奇怪怪

- else if 等价于 elif

- while后也可跟else

- pass语句用于占位


## 虚拟环境

### venv模块

#### 创建虚拟环境

```bash
python -m venv myvenv
```

#### 激活虚拟环境

只需运行.\myvenv\Script\active.bat脚本即可

命令行下运行.\myvenv\Script\Active.ps1 进入虚拟环境后命令行前会有提示

#### 退出虚拟环境

只需运行.\myvenv\Script\deactive.bat脚本即可

## 参考资料

更多内容自己看吧 懒得抄了

[菜鸟教程](https://www.runoob.com/python3/python3-tutorial.html)

[廖雪峰网站](https://www.liaoxuefeng.com/wiki/1016959663602400)