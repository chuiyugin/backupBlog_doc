---
title: C语言系统学习
tags:
  - C
  - 系统观
categories:
  - C语言
date: 2025-3-26 13:30:00
excerpt: C语言系统学习
---
# C 语言系统学习
## C 语言编译过程
### 预处理
+ `#include` ：头文件包含（把头文件中的内容复制到指令所在的位置）
+ `#define N 5`：宏定义（简单的文本替换）
+ `#define FOO(x) (1 + (x) * (x))`：带参数的宏（宏函数）（文本替换：用“实参”替换“形参”）
+ 宏函数注意事项：

![宏函数注意事项](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250326143036.png)
+ 为什么要用宏函数？
	+ 避免函数调用的开销（频繁调用且简短的函数可以用宏函数）；
	+ 提供了一定的“宏编程”能力。

### 编译
+ 经过预处理器处理的文件会交给编译器进行编译；
+ 编译器会把程序翻译成对应平台的汇编代码。
### 汇编
+ 汇编器会把生成的汇编代码翻译成对应平台的机器代码（目标文件）；
+ 此时程序还不能运行，还需要经过最后一个步骤——链接。
### 链接
+ 在链接阶段，链接器会把由汇编器生成的目标代码和程序需要的其它附加代码整合在一起，生成最终可执行的程序；
+ 这些附加代码包括程序用到的库函数（如 `printf` 函数）。
### 汇总
+ 整个过程如下图所示：

![C语言的编译过程](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250326142719.png)
## 虚拟内存空间
+ 程序经过预处理、编译和链接，最终生成可执行文件；
+ 可执行文件被操作系统加载到内存，程序得以运行，运行中的程序称为**进程**（`process`）；
+ 每个进程都有自己的虚拟内存空间，如下图所示：

![进程的虚拟内存空间](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250326151449.png)

## 变量
+ 变量要绑定一个值：

![变量](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250326151924.png)

## 输入输出模型
+ 输入输出模型是为了平衡内存和外部设备的速度差异。

![输入输出模型](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250326153610.png)

### printf
+  `printf = print + format` 打印＋格式化；
+ 作用：打印格式串中的内容，并用后面表达式的值替换转换说明（即 `%d` 、`%f` 等占位符）。

![printf](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250326155245.png)

+ 格式串：

![格式串](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250326155451.png)

### scanf
+ 原理：从左到右，依次匹配格式串中的每一项；

![scanf](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250326160511.png)

+ `scanf` 返回匹配成功的转换说明的个数；
+ 能够匹配任意多个**空白字符**；
+ 如果有一项匹配不成功，`scanf` 会立刻返回。

![scanf](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250326160605.png)

## 整数的编码
### 有/无符号整数
+ 无符号整数：

![无符号整数](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250326202710.png)

+ 有符号整数：

![有符号整数](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250326202741.png)
+ 两个性质：

![两个性质](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250326203540.png)

+ 原码、补码、反码
	+ 正数的原码、反码、补码均相同。  
	+ 原码：用最高位表示符号位，其余位表示数值位的编码称为原码。其中，正数的符号位为 0，负数的符号位为 1。  
	+ 负数的反码：把原码的符号位保持不变，数值位逐位取反，即可得原码的反码。  
	+ 负数的补码：在反码的基础上加 1 即得该原码的补码。    
	+ 例如：  
		+ +11 的原码为: 0000 1011  
		+ +11 的反码为: 0000 1011  
		+ +11 的补码为: 0000 1011  
  
		+ -7 的原码为：1000 0111  
		+ -7 的反码为：1111 1000  
		+ -7 的补码为：1111 1001  
  
	+ 注意，对补码再求一次补码操作就可得该补码对应的原码。

## 浮点数
+ 浮点数是不精确的

![浮点数](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250326204701.png)

## 字符
+ 字符的编码为 ASCII：

![ASCII](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250326210204.png)

+ 类型通用的两个特征：

![类型通用的两个特征](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250326210319.png)

+ 表示值：

![表示值](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250326210426.png)

+ 支持哪些操作：

![支持哪些操作](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250326210509.png)

+ 读/写（和用户操作）

![读/写（和用户操作）](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250326210602.png)

+ 惯用法：

![惯用法](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250326210830.png)

## 类型转换
+ 隐式转换：不要将有符号整数和无符号整数混合运算

![隐式转换](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250326211751.png)

+ 强制转换

![强制转换](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250326212431.png)

## 定义别名
+ 定义别名的作用

![定义别名](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250326213123.png)

## sizeof 运算符
+ `sizeof()` 运算符

![sizeof()运算符](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250326213219.png)

## void 类型
+ void 是一个类型
	+ 空类型
	+ 没有值

## 表达式
+ 表达式：计算某个值的公式
+ 最简单的表达式：变量和常量
+ 运算符：连接表达式，创建更复杂的表达式
+ 属性：优先级、结合性

## 位运算符
+ `i<<n`：如果没有溢出，左移 $n$ 位相当于乘以 $2^{n}$；
+ `i>>n`： 右移 $n$ 位，相当于除以 $2^{n}$（向下取整）；
	+ 如果 `i` 是无符号值或者非负值，则在左边补 `0`；
	+ 如果 `i` 是负值，其结果是由实现定义的，有可能在左边补 `0`，也有可能在左边补 `1`。
+ 为了可移植性，最好仅对无符号数进行移位运算。

![位运算符](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250329145055.png)

### 问题一
+ 判断一个数是否奇数：
	+ 需要注意 `-1 % 2 == -1` ，因此不能使用 `n % 2 == 1` 来判断。

```c
bool isOdd(int n)
{
	return (n % 2 != 0);
}
```

+ 采用二进制的形式：

```c
bool isOdd(int n)
{
	return (n & 0x1);//奇数的最后一位一定是1，掩码（mask）
}
```

### 问题二
+ 如何判断一个非 `0` 整数是否为 `2` 的幂 `（1，2，4，8，16，......）` ？
	+ 从二进制中能够看到只有一位为 `1`，其余都是 `0`。

![问题二](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250329150927.png)

+ 代码：

```c
bool isPowerOf2(int n)
{
	return (n & n-1) == 0;
}
```