---
title: 侯捷 C++学习笔记之面向对象高级编程（二）
tags:
  - 面向对象
categories:
  - C++语言
date: 2025-2-28 14:00:00
excerpt: C++、编程语言、面向对象高级编程
---
# 侯捷 C++学习笔记之面向对象高级编程（二）
## conversion function 转换函数
+ 转换函数（分数转为 `double`）程序示例：

```cpp
class Fraction
{
public：
	Fraction(int num , int den=1) //构造函数
	: m_numerator(num) , m_denominator(den) { }

	operator double() const //相当于操作符重载（double属于显式类型转换运算符）
	{
		return (double) (m_numerator/m_denominator);
	}

private:
	int m_numerator; //分子
	int m_denominator; //分母
}
```

+ **说明**：对于上述示例的转换函数，可以看到其相当于操作符重载（`double` 属于显式类型转换运算符），该转换函数不需要返回类型，因为本身转换为 `double` 类型，属于 `C++` 语言特性，能够避免返回类型与转换类型不一致的问题。

+ 上述示例的调用示例如下：

```cpp
Fraction f(3,5);
double d = 4+f; //调用operator double() const{}函数将f转为0.6
```

### non-explicit-one-argument 构造函数
+ `one-argument` 是指该构造函数只需要一个实参就足够了，其中以下示例的构造函数有 `den=1` 这个默认参数，因此是属于 `one-argument` 的。

```cpp
class Fraction
{
public：
	Fraction(int num , int den=1) //non-explicit-one-argument构造函数
	: m_numerator(num) , m_denominator(den) { }

	Fraction operator+(const Fraction& f)
	{
		return Fraction(......);
	}

private:
	int m_numerator; //分子
	int m_denominator; //分母
}
```

+ 上述示例的调用示例如下：

```cpp
Fraction f(3,5);
Fraction d2 = f+4; //调用 non-explicit-one-argument 构造函数将4转为Fraction类型然后执行operator+
```

### explicit（明确的） 构造函数
+ 将上述两种方式组合一下可能会造成歧义，来看下述例子：

```cpp
class Fraction
{
public：
	Fraction(int num , int den=1) //non-explicit-one-argument构造函数
	: m_numerator(num) , m_denominator(den) { }

	//Fraction转double
	operator double() const //相当于操作符重载（double属于显式类型转换运算符）
	{
		return (double) (m_numerator/m_denominator);
	}

	//int转Fraction
	Fraction operator+(const Fraction& f)
	{
		return Fraction(......);
	}

private:
	int m_numerator; //分子
	int m_denominator; //分母
}
```

+ 使用示例：
```cpp
Fraction f(3,5);
Fraction d2 = f+4; //报错[Error]，编译器提示ambiguous（存在歧义的）
```

+ 存在两种可能性：
	+ 一是将 `4` 转为 `Fraction` 类型进行相加；
	+ 二是将 `f` 转为 `double` 类型进行相加。
+ 两种错误就会导致编译器报错，提示 ambiguous（存在歧义的），解决方法是在构造函数前加入 `explicit` 关键字，如下示例：

```cpp
class Fraction
{
public：
	explict Fraction(int num , int den=1) //non-explicit-one-argument构造函数
	: m_numerator(num) , m_denominator(den) { }

	//Fraction转double
	operator double() const //相当于操作符重载（double属于显式类型转换运算符）
	{
		return (double) (m_numerator/m_denominator);
	}

	//int转Fraction
	Fraction operator+(const Fraction& f)
	{
		return Fraction(......);
	}

private:
	int m_numerator; //分子
	int m_denominator; //分母
}
```

+ 使用示例：
```cpp
Fraction f(3,5);
Fraction d2 = f+4; //报错[Error]，编译器提示需要从double转为Fraction
double d = 4+f; //调用operator double() const{}函数将f转为0.6，不报错
```

+ **说明**：加入 `explicit` 关键字后说明不让编译器自动将 `4` 转为 `Fraction` 类型。因此示例的第一个使用方法会报错，提示需要从 `double` 类型转为 `Fraction` 类型。而第二个使用方法能够将 `f` 转为 `double` 类型并相加就不会报错。

## pointer-like classes
### 智能指针
+ 较为简单的智能指针的示例（`shared_ptr`）:

![shared_ptr](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250301210258.png)

### 迭代器
+ 迭代器作为容器中的一种用法，以链表迭代器为例：

![链表迭代器 1](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250301211615.png)

![链表迭代器 2](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250301211659.png)

### 注意点
+ 关于 `C++` 语言中的指针，需要注意运算符 `.` 和 `->` 用法的区别，例子如下：

```cpp
#include <stdio.h>
#include <stdlib.h>
#include <iostream>
#include <memory>

using namespace std;

//主函数
int main()
{
    //智能指针：共享指针
    shared_ptr<string> name(new string("yugin"));
    cout << "*name:  " << *name << endl;
    cout << "(*name).size():  " << (*name).size() << endl;
    cout << "name->size():  " << name->size() << endl;
    
    cout << "---------------" << endl;
    cout << "name.use_count():  " << name.use_count() << endl;
    
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```

+ 输出结果：

![输出结果](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250301212429.png)

+ 从上述输出结果中不难看出，`(*name).size()` 与 `name->size()` 的输出结果一致，使用的是 `string` 这个对象里面的 `size()` 函数。
+ 而 `name.use_count()` 输出的是数值 1，使用的是智能指针 `shared_ptr<string>` 里面的 `use_count()` 函数。
+ `use_count()` 函数表示有多少个智能指针指向某个对象，主要用于调试目的。例如：

```cpp
#include <iostream>
using namespace std;

int main()
{
	shared_ptr<int> p1(new int(100));
	cout << p1.use_count() << endl; // 1
	
	shared_ptr<int> p2(p1);
	cout << p1.use_count() << endl; // 2
	
	shared_ptr<int> p3;
	p3 = p2;
	cout << p1.use_count() << endl; // 3
	cout << p3.use_count() << endl; // 3
	
	return 0;
}
```

## function-like classes，仿函数
+ 在 C++中，函数调用运算符（`operator()`）允许一个类的对象表现得像函数一样被调用。这种特性通过重载类的圆括号操作符 `operator()` 实现。
+ 注：小括号 `()` 就是函数调用操作符，`operator()` 就是对函数调用操作符进行重载。
+ 这对于实现类似于函数的对象非常有用，例如，你可以创建一个对象，该对象封装了一组操作，而这些操作可以通过调用对象来实现。
+ 例如创建一个简单的计算器类，该类可以执行加法和减法操作：

```cpp
#include <iostream>

class Calculator {
public:
    // 定义函数调用运算符，接受两个int参数并返回它们的和
    int operator()(int a, int b) {
        return a + b;
    }
    
    // 定义另一个函数调用运算符，接受两个int参数并返回它们的差
    int operator()(int a, int b, const char* op) {
        if (op == "subtract") {
            return a - b;
        } else {
            return a + b; // 默认返回和，如果op不是"subtract"
        }
    }
};

int main() {
    Calculator calc;
    std::cout << "Sum: " << calc(5, 3) << std::endl; // 使用第一个重载的operator()
    std::cout << "Difference: " << calc(5, 3, "subtract") << std::endl; // 使用第二个重载的operator()
    return 0;
}
```

## function template，函数模板
+ 编译器会对函数模板（`function template`）进行实参推导 ( `argument deduction` )。

![函数模板](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250306134454.png)

## member template，成员模板
+ 在模板类里面还有一份模板，成为成员模板：

![成员模板1](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250306164753.png)

+ 成员模板中的继承关系：

![成员模板2](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250306171840.png)

![成员模板3](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250306172314.png)

## specialization，模板特化
+ 全特化：特定类型的模板执行特定类型的函数或事情。

![模板特化1](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250306211554.png)

### 模板的偏特化
+ 个数的偏：

![模板个数的偏特化](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250306212509.png)

+ 范围的偏：

![模板范围的偏特化](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250306212806.png)

## template template parameter，模板模板参数
+ 模板模板参数，即允许一个模板接受另一个模板作为参数。
+ 可以使用 `template<template<...> class...>` 语法来定义一个模板模板参数。

![模板模板参数1](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250306214228.png)

![模板模板参数2](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250306215019.png)

+ 不是模板模板参数的情况：

![不是模板模板参数的情况](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250306215404.png)
