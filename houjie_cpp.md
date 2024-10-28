---
title: 侯捷C++学习笔记
tags: [C++语言]
categories: [C++语言]
date: 2024-10-27 22:20:00
excerpt: C++、编程语言
---
# 侯捷 C++学习笔记
## 头文件与类的声明
### class template 类（模板）简介
+ 例如我们定义下面的一个类，我们可以不把一个类里面定义的变量类型（如：`double`）直接就写死，而是可以定义一个模板 `template`，具体如下面的例子：

```cpp
template<typename T>
class complex
{
public:
	complex (T r = 0, T i = 0)
	: re (r), im (i)
	{ }
	complex& operator += (const complex&);
	T real () const { return re; }
	T imag () const { return im; }
private:
	T re, im;
	friend complex& __doapl (complex*, const complex&);
};
```
+ 当我们具体需要使用上面的的类并定义确定的变量类型的时候可以进行如下操作：

```cpp
complex<double> c1(2.5,1.5);
complex<int> c2(2,6);
```

### inline （内联）函数
+ 若函数在 `class` 本体内部**定义**（并非声明）完成，就会进入内联函数的候选，但是该函数是否真正变成内联函数由编译器所决定。
+ 内联函数具有执行效率比较高的特性，但并非所有定义在 `class` 本体内的函数都能由编译器编译为内联函数。

```cpp
template<typename T>
class complex
{
public:
	complex (T r = 0, T i = 0)
	: re (r), im (i)
	{ }
	//在class本体内声明的函数无法作为内联函数
	complex& operator += (const complex&);
	//下面两个定义在class本体里面的函数会成为内联函数的候选
	T real () const { return re; }
	T imag () const { return im; }
private:
	T re, im;
	friend complex& __doapl (complex*, const complex&);
};
```

+ 要么在外部定义函数的时候加上 `inline` 前缀，如：
```cpp
inline double 
imag(const complex& x) 
{ 
	return x.imag (); 
}
```

## 构造函数
+ 构造函数是用来创建对象的函数，其有如下特点：
	+ 不需要返回类型
	+ 具有初始列（`initialization list`），别的函数没有

```cpp
class complex
{
public:
	complex (double r = 0, double i = 0)//默认实参
		: re (r), im (i) //initialization list 初始列
	{ }
	complex& operator += (const complex&);
	double real () const { return re; }
	double imag () const { return im; }
private:
	double re, im;
	friend complex& __doapl (complex*, const complex&);
};
```

+ 使用构造函数创建对象，在创建对象的过程中自然而然地使用构造函数，例：

```cpp
complex c1(2,1);
complex c2;
complex* p = new complex(4);
...
```

### 函数重载（overloading）
+ 同名的函数可以有多个，其传入的参数不一样，称为函数重载（在编译器看来是不同名的），例如：

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202410281102136.png)

+ 从上图可以看出，当同名的函数传入的参数**类型，数量**等不一致时，编译器可以区分出这两个函数，称为函数重载。
+ 函数的重载也一般发生在构造函数中。
+ 但是例如上面方框中的两个构造函数，由于第一个函数存在默认值，当使用该构造函数创建对象，并且采用默认值的时候，编译器将无法确认使用哪个函数，因此上面的写法时**错误**的。

