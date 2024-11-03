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
+ 同名的函数可以有多个，其传入的参数不一样，称为函数重载（在编译器看来是不同名的）。
+ 例如：

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202410281102136.png)

+ 从上图可以看出，当同名的函数传入的参数**类型，数量**等不一致时，编译器可以区分出这两个函数，称为函数重载。
+ 函数的重载也一般发生在构造函数中。
+ 但是例如上面方框中的两个构造函数，由于第一个函数存在默认值，当使用该构造函数创建对象，并且采用默认值的时候，编译器将无法确认使用哪个函数，因此上面的写法时**错误**的。

## 参数传递与返回值
### 常量成员函数（const member functions）
+ `const` 定义这个参数的内容是不会改变的，在函数里面表示这个函数将不会改变任何参数。
+ 例如：

```cpp
class complex
{
public:
	complex (double r = 0, double i = 0)
		: re (r), im (i) 
	{ }
	complex& operator += (const complex&);
	double real () const { return re; }//加入const，表示将不会改变re的值
	double imag () const { return im; }//加入const，表示将不会改变im的值
private:
	double re, im;
	friend complex& __doapl (complex*, const complex&);
};
```

+ 对于上述类的创建和函数的调用，可能有以下两种情况：
1.  没加 `const`，运行没问题：
```cpp
{ 
	complex c1(2,1); 
	cout << c1.real(); 
	cout << c1.imag(); 
}
```
2. 加了 `const`，但是如果在类的函数中没加 const，将会编译失败。
```cpp
{ 
	const complex c1(2,1); 
	cout << c1.real(); 
	cout << c1.imag(); 
}
```

### 参数传递
+ 参数传递尽量都传引用，当所传的参数不希望被函数所修改可以在引用前面加上 `const`，当所传参数可能被函数所修改时就不在前面加上 `const`。
+ 例如：
```cpp
class complex
{
public:
	complex (double r = 0, double i = 0)//值传递
		: re (r), im (i) 
	{ }
	complex& operator += (const complex&);//引用传递
	double real () const { return re; }
	double imag () const { return im; }
private:
	double re, im;
	friend complex& __doapl (complex*, const complex&);
};
```

+ 引用传递的调用例子：
```cpp
{
	complex c1(2,1);
	complex c2;
	c2 += c1;
	cout << c2;
}
```

### 返回值传递
+ 与参数传递相似，返回值传递也**尽量**传引用，但返回值的传递有时候将无法传引用。
+ 例如：
```cpp
class complex
{
public:
	complex (double r = 0, double i = 0)
		: re (r), im (i) 
	{ }
	complex& operator += (const complex&);//返回值采用引用传递
	double real () const { return re; }//返回值采用值传递
	double imag () const { return im; }//返回值采用值传递
private:
	double re, im;
	friend complex& __doapl (complex*, const complex&);
};
```

#### 无法使用引用传递返回值的情况
+ 要返回的参数是在函数内部创建的将不能采用引用传递。
+ 例如：
```cpp
int my_add(int& a,int& b)
{
	int temp;
	temp = a + b;
	return temp;
}
```
+ 例如上面的例子，`temp` 是在函数内部创建的，因此不能够使用**引用传递来返回值**！
	+ 因为当这个函数运行结束后，`temp` 变量将会被释放。

#### 可以使用引用传递返回值的情况
+ 只要返回的参数不是在函数内部创建的，是在外部就有该参数空间的都可以采用引用传递来返回值。
+ 例如：
```cpp
inline complex& __doapl(complex* ths, const complex& r) 
{ 
	ths ->re += r.re ; 
	ths - >im += r.im ; 
	return *ths ; 
}
```
+ 从上面的例子可以看到，`ths` 是从函数外部传入的参数，其空间本来在函数外部就存在，因此可以使用**引用传递来返回值**。

### 友元（friend）
+  友元可以令表示的函数能够访问该类的**私有成员**，包括**私有变量和私有函数**。
+ 例如：
```cpp
class complex
{
public:
	complex (double r = 0, double i = 0)
		: re (r), im (i) 
	{ }
	complex& operator += (const complex&);
	double real () const { return re; }
	double imag () const { return im; }
private:
	double re, im;
	friend complex& __doapl (complex*, const complex&);//友元
};
```

+ 友元函数调用的例子：
```cpp
inline complex& 
__doapl (complex* ths, const complex& r) 
{ 
	ths - >re += r.re ; 
	ths - >im += r.im ; 
	return *ths ; 
}
```
+ 能够自由取得 `friend` 的 `private` 成员。

### 相同 class 的各个 objects 互为 friends（友元）
+ 如题，在创建了相同 `class` 的各个 `objects` 是互为友元的。
```cpp
class complex
{
public:
	complex (double r = 0, double i = 0)
		: re (r), im (i)
	{ }
	//并没有加friend
	int func(const complex& param)
		{ return param.re + param.im; }
		
private:
	double re, im;
};
```
+ 调用情况如下：
```cpp
{
	complex c1(2,1);
	complex c2;
	c2.func(c1);//没有加friend可以调用函数取private的内容
}
```
+ 解释如下：在创建了相同 `class` 的两个 `objects` （是 `c1` 和 `c2`）是互为友元的。

## 操作符重载与临时对象
### 操作符重载之一：成员函数（this）
+ 在 `class` 中实现操作符的重载：
```cpp
inline complex&
__doapl(complex* ths, const complex& r)
{
	ths->re += r.re;
	ths->im += r.im;
	return *ths;
}
inline complex&
complex::operator += (const complex& r)
{
	return __doapl (this, r);
}
```
+ 需要注意，`this` 是隐藏参数，不需要写在函数参数传递的过程中：

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411031557563.png)

#### return by reference 语法分析
+ 传递者无需知道接收者是以引用的形式接收

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411031618944.png)

### 操作符重载之二：非成员函数（无 this）
+ 为了应付用户的三种用法，对应开发三种函数；
+ 与**成员函数**方式的区别在于**非成员函数**采用的是全局函数（`global`）：

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411031718666.png)

+ 可以看出，上面的函数绝对不可以采用**返回引用**的形式，因为它们返回的必定是个本地对象（`local object`）。
+ 同时，`typename()` 创建的是**临时对象**，小括号中没有参数就是默认值。例如 `int(5)` 等，表明其是临时的，生命周期很短，在下一行便结束。在上述的例子中也有体现：

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411031738262.png)

+ `“+”`表示正，`“-”`表示负的情况：

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411031742123.png)

+ 判断两个复数是否相等（两种情况`==`和`!=`）：

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411031746875.png)

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411031746434.png)

+ 输出运算符 `“<<”` 也称为流插入运算符，是一个二目运算符，该操作符的重载具有特殊用法：

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411031802200.png)


