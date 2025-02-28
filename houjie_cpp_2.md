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