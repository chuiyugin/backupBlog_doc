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
  
	+ 注意：
		+ 对补码再求一次补码操作就可得该补码对应的原码。
		+ 在计算机系统中，数值一律用补码来表示（存储）。
		+ 使用补码，可以将符号位和其他位统一处理;

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

### 问题三
+ 给定一个值不为 `0` 的整数，请找出值为 `1` 的最低有效位（`last set bit`）。

![问题三](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250407160135.png)
+ 代码：

```cpp
int lastSetBit(int n)
{
	return n & -n;
}
```

+ 原理：
	+ `n` 的原码（补码）：`0001 1000`
	+ `-n` 的补码：`0110 1000`
	+ `n & -n` 的码：`0000 1000`
	+ 在计算机系统中，数值一律用补码来表示（存储）。

### 问题四
+ 给定两个不同的整数 a 和 b，请交换它们两个的值（要求不使用中间临时变量）。
	+ 关键是找到一对逆运算，如 `a=a+b-b;`
+ 代码：

```cpp
void swap(int a,int b)
{
	a = a + b;
	b = a - b;
	a = a - b;
}
```

+ 但是使用加法容易造成溢出的情况，可以使用异或这个逆运算。

```cpp
void swap(int a,int b)
{
	a = a ^ b;
	b = a ^ b;
	a = a ^ b;
}
```

### 问题五
+ 一个数组中有一种数出现了奇数次，其他数都出现了偶数次，怎么找到并打印这种数？
+ 关键部分：

```cpp
//采用异或运算找到数组中出现奇数次的数
int yihuo_find(vector<int>& a)
{
    int eor = 0;
    for(int i=0;i<a.size();i++)
    {
        eor = eor ^ a[i];//用eor与数组中的所有元素相异或
    }
    return eor;
}
```

+ 代码：

```cpp
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <vector>
using namespace std;

//采用异或运算找到数组中出现奇数次的数
int yihuo_find(vector<int>& a)
{
    int eor = 0;
    for(int i=0;i<a.size();i++)
    {
        eor = eor ^ a[i];//用eor与数组中的所有元素相异或
    }
    return eor;
}

int main()
{
    vector<int>  vec1= {1, 1, 3, 3, 3}; //3是奇数个
    int ans = yihuo_find(vec1);
    printf("%d",ans);
    system("pause"); // 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```

+ 证明：
	+ 1、具有 `a^a=0` 和 `a^0=a` 的性质；
	+ 2、偶数个数据累积异或后是 `0`，奇数个数据累积异或后就剩下它自身。

### 问题六
+ 一个数组中有两个不相等的数出现了奇数次，其他数都出现了偶数次，怎么找到并打印这两个数？
+ 关键代码：

```cpp
//采用异或运算找到数组中出现奇数次的两个数a、b
void yihuo_find(vector<int>& a,int& ans_1,int& ans_2)
{
    int eor = 0,eor_2 = 0;
    for(int i=0;i<a.size();i++)
    {
        eor = eor ^ a[i];//找到eor = a^b
    }
    //找到eor = a^b中的最右侧的1（问题三的函数）
    int OnlyOne = lastSetBit(eor);
    //由于eor = a^b中的最右侧为1，a和b在这一位一定不相等
    //只需要在数组中找到这一位只为1的数再做一次eor即可把a或者b找出来
    for(int i=0;i<a.size();i++)
    {
        if((OnlyOne & a[i]) != 0)
        {
            eor_2 = eor_2 ^ a[i];//找到eor_2 = a或者eor_2 = b
        }
    }
    ans_1 = eor_2;
    ans_2 = eor_2 ^ eor;
}
```

+ 代码：

```cpp
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <vector>
using namespace std;

//把int类型最右侧的1找出来
int lastSetBit(int n)
{
	return n & -n;
}

//采用异或运算找到数组中出现奇数次的两个数a、b
void yihuo_find(vector<int>& a,int& ans_1,int& ans_2)
{
    int eor = 0,eor_2 = 0;
    for(int i=0;i<a.size();i++)
    {
        eor = eor ^ a[i];//找到eor = a^b
    }
    //找到eor = a^b中的最右侧的1
    int OnlyOne = yihuo_last_one(eor);
    //由于eor = a^b中的最右侧为1，a和b在这一位一定不相等
    //只需要在数组中找到这一位只为1的数再做一次eor即可把a或者b找出来
    for(int i=0;i<a.size();i++)
    {
        if((OnlyOne & a[i]) != 0)
        {
            eor_2 = eor_2 ^ a[i];//找到eor_2 = a或者eor_2 = b
        }
    }
    ans_1 = eor_2;
    ans_2 = eor_2 ^ eor;
}

int main()
{
    int ans_1=0,ans_2=0;
    vector<int>  vec1= {1, 1, 3, 3, 3, 6}; 
    yihuo_find(vec1,ans_1,ans_2);
    printf("%d、%d\n",ans_1,ans_2);
    system("pause"); // 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```

## 语句
+ 语句的分类：

![语句的分类](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250407194813.png)

## 数组
### 数组的内存模型
+ 连续的一片内存空间，并且会划分成大小相等的小空间。
+ 使用同一类型使得能够随机访问元素。

![数组的内存模型_1](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250407200257.png)

+ 数组的索引一般是从 `0` 开始的：

![数组的内存模型_2](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250407200440.png)

+ 数组效率一般高于链表效率：
	+ 空间利用率更高；
	+ 空间局部性（数组是连续的）。

### 数组的定义
+ 数组的定义：
	+ 数组的操作只有取下标。

![数组的定义_1](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250407201301.png)

![数组的定义_2](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250407201707.png)

+ 求数组的大小的宏函数：

```cpp
#define SIZE(a) (sizeof(a) / sizeof(a[0]))
```

+ 类型的判断：
	+ 先看标识符；
	+ 从标识符表示往右看；
	+ 再从标识符往左看得到完整的类型信息，例：

![类型的判断](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250407201452.png)

### 多维数组
+ 注意：C 语言只有一维数组，多维数组本质上也是一维数组。

![多维数组](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250407203805.png)

### 常量数组
+ 常量数组：元素不能够修改。
	+ 安全，用于存储静态数组；
	+ 效率高，编译器能够对常量数组做一些优化。

![常量数组](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250407204317.png)

## 函数
### 函数的特征
+ 数学上的函数：有返回值，没有副作用；
+ C 语言的函数：可以没有返回值，可以有副作用。

### 函数的准则
+ 函数的功能应该越单一越好（能够复用），函数的实现越高效越好；
+ 函数是 C 语言的“基本构建组件”，C 语言的本质就是函数之间的调用。

### 函数相关概念
+ 函数定义（隐含了声明）：`void foo(int a) {...}`
+ 函数声明：`void foo(int a);`
+ 函数调用：`foo(3);`
+ 函数指针：`foo` 或 `&foo`

#### 参数传递
+ 参数传递的内存调用示例：

![参数传递的内存调用](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250409141732.png)

+ 特例：数组作为参数传递时，会退化成指向第一个元素的指针！

![参数传递的特例-数组](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250409142358.png)

#### 局部变量和外部变量
+ 作用域：编译时变量名可以被引用的文本范围；
+ 存储期限：运行时变量绑定的值可以被引用的时间长度：

![存储期限](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250409151729.png)

+ 局部变量：定义在函数里面的变量；
	+ 作用域：块作用域，从定义开始到块的末尾（块：大括号 `{}` ）。
	+ 存储期限：
		+ 默认——自动存储期限-栈
		+ `static` ——静态存储期限-数据段
+ 默认存储期限示例：

![默认存储期限](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250409153043.png)

+ 静态存储期限示例：

![静态存储期限](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250409153131.png)

+ 外部变量（全局变量）：定义在函数外面的变量。
	+ 作用域：文件作用域，从定义开始到文件末尾。
	+ 存储期限：静态存储期限。

##### 相关问题
+ 静态存储期限的局部变量和外部变量有什么区别？
	+ 答：作用域不同！
+ 静态储存期限的局部变量有什么作用？（使用场景）
	+ 答：能够存储上一次函数调用的状态！

![静态储存期限的局部变量使用场景](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250409153759.png)

## 递归
### 递归的含义
+ 递归的含义：

![递归的含义](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250409154119.png)

+ 具有递归结构的问题，不一定要用递归求解。
+ 递归可能存在大量重复计算的问题。
+ 可以采用**动态规划**来避免重复计算的问题：顺序求解子问题，并将子问题的解保存起来，从而避免重复计算，最终求解到大问题。

### 汉诺塔问题
+ 汉诺塔问题：

![汉诺塔问题_1](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250409195250.png)

![汉诺塔问题_2](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250409195403.png)

### 约瑟夫环问题
+ 约瑟夫环问题：

![约瑟夫环问题_1](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250409200726.png)

![约瑟夫环问题_2](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250409200946.png)

+ 更一般的约瑟夫环问题：

![更一般的约瑟夫环问题](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250409201409.png)

### 递归三问
+ 递归三问：什么时候使用递归？

![递归三问](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250409201536.png)

## 指针基础
### 内存模型
+ 指针的内存模型：
	+ 计算机的最小寻址单位——字节
	+ 变量的地址——变量首字节的地址
	+ 指针——地址
	+ 指针变量——存储地址值的变量

![指针的内存模型](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250411131416.png)
### 指针的操作
+ 指针的定义：
	+ 变量名：`p`
	+ 类型：`int*`
	+ 注意事项：声明指针变量时，需要指定它指向对象的类型

![指针的定义](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250411131533.png)

+ 指针的两个基本操作：
	+ 取地址运算符：`&` —— `&i`
	+ 解引用运算符：`*` ——  `*p` 
		+ 指针变量 `p` 指向对象的别名
		+ 间接引用——访问内存两次

![指针的两个基本操作](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250411131714.png)

### 野指针
+ 野指针：不知道指向哪块数据。
	+ 对野指针进行解引用操作，是未定义行为。

![野指针](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250411155133.png)

+ 如何给指针变量赋值？
	+ `int *p = &i;`
	+ `int *q = p;` —— 指针变量 `p`，`q` 指向同一个对象。
	+ `p = NULL;`：空指针，不指向任何对象指针。

+ 指针赋值示例：

![指针赋值示例_1](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250411155759.png)

![指针赋值示例_2](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250411155946.png)

### 指针的应用
+ 作为参数——在被调函数中修改主调函数的值。

![指针的应用_1](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250411160139.png)

+ 指针作为返回值。
	+ 教训：不要返回指向当前栈帧的指针！

![指针的应用_2](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250411160254.png)

### 指针常量和常量指针
+ 指针变量 `p` 对内存 `1`，内存 `2` 都有写权限：

![指针变量](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416133938.png)

+ 常量指针，对内存 `1` 有写权限：

![常量指针](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416134812.png)

+ 指针常量，对内存 `2` 有写权限：

![指针常量](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416134930.png)

+ 两边加 `const` 修饰符，对内存 `1`，内存 `2` 都没有写权限：

![两边加const](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416135053.png)

+ 本质：限制变量的写权限！

### 传入参数和传出参数
+ 传入参数：在函数里面，不会（不能够）通过指针变量修改它指向的对象：

![传入参数](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416135658.png)

+ 传出参数：在函数里面，可以通过指针变量修改它指向的对象！

![传出参数](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416140224.png)

### 指针的算术运算
+ 画图表现数组和指针：

![数组和指针的画图表示](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416141535.png)

+ 指针加上一个整数，结果还是一个指针：
	+ 指针加上一个整数 `n`，其含义在于指针向右偏移了 `n` 个单位；
	+ 单位表示的是对象类型的大小。

![指针加上整数](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416141818.png)

+ 指针减去一个整数 `n`，其含义在于指针向左偏移了 `n` 个单位：

![指针减去整数](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416142057.png)

+ 两个指针相减，结果是一个整数：

![两个指针相减](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416142407.png)

+ 根据指针减法，可以定义指针的比较运算：

![指针的比较运算](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416142618.png)

### 指针和数组的关系
+ 采用指针处理数组：

![指针处理数组](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416143530.png)

+ `*` 和 `++` 的组合，`--` 同理：

![指针运算的组合](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416144140.png)

+ 指针运算的组合的示例：

![指针运算的组合的示例](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416144222.png)

+  在必要的时候，数组可以退化成指向它索引为 `0` 元素的指针：

![数组退化示例](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416144603.png)

+ 数组退化的几种情况总结
	+ 数组作为参数：`fun(arr);`
	+ 数组在赋值表达式的右边：`int* p = arr;`
	+ 数组参与算术运算：`arr+3;`

 + 指针也支持取下标运算：

![指针取下标运算](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416145337.png)

+ 下标可以放到中括号 `[]` 前面：

![下标可以放到中括号前面]( https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416145643.png )

## 字符串
### 字符串字面值
+ 三种书写方式：
	+ 如第三种方式，如果两个字符串字面值之间仅有空白字符，可以认为这两个字符串字面值是相邻的，相邻的字符串字面值在编译的时候就会拼接成一个字符串字面值。

![字符串字面值的三种书写方式](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416150744.png)

+ 内存模型：
	+ 字符串字面值在代码段——不可被修改的。
	+ 可以把字符串字面值看作常量数组。

![字符串字面值的内存模型](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416151840.png)

+ 字符串字面值支持的操作：
	+ 常量数组能支持的操作，字符串字面值都支持，例：
	+ 十进制转换为十六进制的函数：

![十进制转换为十六进制的函数](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416152559.png)

### 字符串变量
+ 总纲：
	+ C 语言中没有字符串类型—— `string`、`String`
	+ C 语言中的字符串依赖字符数组存在！
	+ C 语言字符串是一种“逻辑类型”。

![字符串和字符数组](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416154414.png)

+ 注意事项：
	+ 字符串必须以 `'\0'` 结尾！
	+ 在 C 语言中求字符串的长度不是 $O(1)$ 的时间复杂度，不能写成下面的例子：

![不能写成该形式](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416154620.png)

#### 声明和初始化
+ 注意数组的初始化和字符串的字面值。

![声明和初始化](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416191810.png)

#### 读和写（和用户交互）
+ `printf()+%s` 输出字符串

![输出字符串](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416192436.png)

+ `scanf()+%s` 读字符串
	+ `%s` 的匹配规则：
		+ 忽略前置空白的字符，读取字符填入字符数组；
		+ 遇到空白字符结束。
	+ 缺点：
		+ 不能够存储空白字符；
		+ 不会检查数组越界。

![scanf()+%s 读字符串](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416192753.png)

+ `puts()` 函数：`puts(str);` 等价于 `printf("%s\n",str);`

![puts() 函数](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416195429.png)

+ `gets()` 函数
	+ 从 `stdin`（标准输入流） 中读取一行数据，传入字符数组，并将 `'\n'` 替换为 `'\0'`；
	+ 缺陷：不会检查数组越界。

![gets() 函数](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416200223.png)

+ `fgets()` 函数
	+ 和 `gets()` 函数的区别：
		+ 会检查是否越界；
		+ 会存储 `'\n'` 符，并在后面添加 `'\0'`。

![fets() 函数](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416200843.png)

### 字符串库函数
+ 需要引入 `#include <string.h>` 库。
#### strlen
+ `strlen()` 的实现和遍历字符串的惯用法：

![遍历字符串的惯用法](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416204136.png)

+ `strlen()` 不会计算 `'\0'`：

![计算字符串的长度](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416204226.png)

#### strcpy() 和 strncpy() 函数
+ `strcpy()` 函数不检查数组越界，`strncpy()` 函数要设定最大拷贝的数量。

![strncpy()函数](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416205020.png)

+ `strcpy()` 函数的实现和惯用法：
	+ 将赋值运算符右侧的"表达式”的值赋给左侧的变量，赋值表达式的值就是被赋值的变量的值。
	+ 例如：`a=(b=5)` 相当于 `b=5` 和 `a=b` 两个赋值表达式，因此 `a` 的值等于 `5`，整个赋值表达式的值也等于 `5`。

![strcpy()的实现和惯用法](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416210657.png)

+ `strncpy()` 函数的实现：

![strncpy()的实现](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416211333.png)

#### strcat() 和 strncat() 函数
+ `strcat()` 函数不检查数组越界，`strncat()` 函数要设定最大拷贝的数量。

![strcat() 和 strncat() 函数的使用](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416212013.png)

+ `strcat()` 函数的实现：

![strcat()函数的实现](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416212212.png)

+ `strncat()` 函数的实现：

![strncat()函数的实现](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416212331.png)

#### strcmp()函数
+ 字符串的比较：`strcmp()` 函数
+ 比较规则：字典序（`ASCII`）

![字符串的比较](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416212727.png)

+ `strcmp()` 函数的实现：

![strcmp()函数的实现](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250416213027.png)

### 字符串数组
#### 字符串数组
+ 字符数组的数组——二维字符数组
	+ 缺陷：
		+ 浪费内存空间；
		+ 交换字符串不方便。
	+ 初始化的时候是数组初始化式。

![字符串数组](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250417152319.png)

#### 字符指针数组
+ 储存的是字符指针，比较灵活，不会造成内存空间的浪费；
+ 直接初始化的时候是字面值，字符串存储在代码段，无法修改；
+ 需要能够修改的字符串要先定义字符串变量再合并到字符指针数组。

![字符指针数组](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250417153006.png)


