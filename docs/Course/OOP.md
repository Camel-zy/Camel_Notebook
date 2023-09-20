---
title: Object Oriented Programming
subtitle: 面向对象程序设计
---

# oop的部分题目摘录

<!--more-->

## Template

类模板的参数错误的说法是：	B
A.可以有多个
B.可以有0个
C.不能有基本数据类型
D.参数不能给初值


### 静态成员

模板类中静态成员的特殊之处：每种类型的实例的静态成员相互独立，不共享。例如：

```cpp
template <typename T>
void test(const T&x) {
    static int count = 0;
    cout << "x = " << x << " count = " << count << endl;
    ++count;
    return;
}
 
void main() {
    test<int>(2);
    test<int>(2);
    test<double>(2.2);
}
// output:
x = 2 count = 0
x = 2 count = 1
x = 2.2 count = 0
```

### 类模板显式具体化

类模板有两种特化：全特化和偏特化。

```cpp
template<class T1,class T2>
class MyClass {…};

template<>
class MyClass<int,int>{…};      //全特化

template<class T1>
class MyClass<T1,int>{…};       //偏特化
```

## STL

## vector

```cpp
vector<int> a;
for(int i : a){
    // 遍历a中所有元素
}
```

## string

```c++
s.substr(pos, n); 
// 返回一个string，包含s中从pos开始的n个字符的拷贝（pos的默认值是0，n的默认值是s.size() - pos，即不加参数会默认拷贝整个s）
```

### pair

- pair类模板定义头文件utility中
- 创建pair<>对象可使用make_pair()函数

## Exception

- catch模块数量没有限制。

  ```cpp
  How many catch blocks can a single try block can have? D
  A.Only 1
  B.Only 2
  C.Maximum 127
  D.As many as required
  ```

- 基类和派生类中写catch块时，应当先定义派生类的catch块（先处理派生类的异常），否则派生类不会处理内部异常。

- 捕获到多个异常：

  ```cpp
  If a catch block accepts more than one exceptions then __________________ B
  A.The catch parameters are not final
  B.The catch parameters are final
  C.The catch parameters are not defined
  D.The catch parameters are not used
  ```
