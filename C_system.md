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




