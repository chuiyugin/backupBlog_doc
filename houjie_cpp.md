---
title: 侯捷C++学习笔记
tags: 
categories: 
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