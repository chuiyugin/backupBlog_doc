---
title: 算法笔记
tags:
  - 数据结构
  - 算法
categories:
  - 数据结构
date: 2023-11-5 20:00:00
excerpt: 算法笔记的学习与分享总结!
---
# 算法笔记

## C/C++快速入门

### 头文件

* 当我们忘记函数包含在哪个头文件下时或者头文件包含较多时，可以使用这个万能头文件来代替。但这个头文件也有缺点，最明显的是使用后**编译时间太长**。另外，由于 `include＜bits/stdc++.h＞`不是C++的标准头文件，所以会**有少部分编译器不支持**。因此建议使用**标准头文件**！

### 主函数

* 主函数是一个程序的入口位置，整个程序从主函数开始执行，而且一个程序最多只能有一个主函数。

### 基本数据类型

#### 变量的定义 

* 变量是在程序运行过程中**其值可以改变的量**，需要在**定义**之后才可以使用。

#### 变量的类型

##### 基本数据类型

* 基本数据类型分为**整型、浮点型、字符型和布尔型**。

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202310102009603.png)

* 注意在计算机系统中不管正数与负数的表示和存储都是以**补码**的形式。
* **原码**的表示为：第一位表示符号（0为正，1为负），其余位表示数值。
* **反码**的表示方法分为正数和负数两种：
  - 正数的反码等于原码本身。
  - 负数的反码是在其原码的基础上，符号位不变（即首位不变），其余各位按位取反。
* **补码**的表示方法同样分为正数和负数两种：
  - 正数的补码是其原码本身。
  - 负数的补码是在其原码的基础上，符号位不变，其余各位按位取反后加1（即在反码的基础上加1）。

##### 整型(int)
* 对于整型`int`而言，一个整数占**32bit**，即**4个Byte**，一般绝对值在$10^9$范围以内的整数都可以定义为**int型**。

##### 长整型(long long)

+ 对于长整型`long long`而言，一个整数占**64bit**，即**8个Byte**，如果需要的整数取值范围超过**2147483647**(超过$10^{10}$)就需要使用**长整型**。

##### 浮点型

* `%f`是**单精度浮点型**(`float`)和**双精度浮点型**(`double`)的输出格式
* 对于浮点型而言，一般不需要使用`float`，碰到浮点型都应该使用`double`来进行存储。

##### 字符型

###### 字符变量和字符常量

```cpp
char c;
char c = 'e';
```

+ 从上面的程序中可以看出来，第一段的`c`被成为**字符变量**，对于带单引号的`‘e’`则被称为**字符常量**，而且必须是**单个字符**。
+ **小写字母**比**大写字母**的**ASCII码值**大**32**。
+ `%c`是`char`型的输出格式。

###### 转义字符

- **ASCII码**中有一部分是**控制字符**，是**不可显示**的。

+ 比较常用的转义字符：

> \n 表示换行
>
> \0 表示空字符NULL，其ASCII码为0，要注意 \0 不是空格

###### 字符串常量

字符串常量可以作为初值赋给字符串数组，并且使用`%s`的格式输出。

```cpp
#include <cstdio>
using namespace std;
int main(){
    char str[25] = "this is the char test";
    printf("%s",str);
    return 0;
}
```

输出结果：

```
this is the char test
```

##### 布尔型

布尔型变量只能是**true(真、非零)**和**false(假、零)**。

#### 强制类型转换

强制类型转换的格式如下：

> (新类型名)变量名

#### 符号常量和const常量

* 符号常量通俗而言就是替换，也称为“宏定义”。

```cpp
#define 标识符 常量
#define pi 3.14
```

* 另一种定义常量的办法是const常量。

```cpp
const 数据类型 变量名 = 常量;
const double pi = 3.14;
```

> 这两种写法都被称为常量，一旦确定其值后将无法改变。

#### 运算符

##### 算术运算符

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202310122043030.png)

##### 关系运算符

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202310122044294.png)

##### 逻辑运算符

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202310122045903.png)

##### 条件运算符

```cpp
A : B ? C
```

+ 如果A为真，执行并返回B的结果；如果A为假，那么执行并返回C的结果。

##### 位运算符

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202310122048285.png)

### 顺序结构

#### 使用scanf和printf输入/输出

##### scanf格式符

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202310131437646.png)

+ 注意上表中最后一行，数组名称本身就代表了这个数组第一个元素的地址，所以不需要加取地址运算符。因此在`scanf`中，除了`char`数组整个输入的情况不加`&`之外，其他变量类型都需要加`&`。

+ 注意字符数组使用`%s`读入的时候以**空格**和**换行**为读入结束的标志。

##### printf格式符

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202310131513505.png)

+ 对于`double`类型的变量，其在`printf`中的输出格式变成了`%f`，而在`scanf`中却是`%lf`。

##### 三种实用的输出格式

###### %md

+ `%md`可以使不足**m**位的`int`型变量以**m**位进行右对齐输出，其中高位用**空格**补齐，如果变量本身超过**m**位，则保持原样。

```cpp
#include <cstdio>
using namespace std;
int main(){
    int a = 123;
    int b = 12345678;
    printf("%5d\n",a);
    printf("%5d\n",b);
    return 0;
}
```

+ 输出：

```
  123
12345678
```

###### %0md

+ `%0md`只是在`%md`中间多加了**0**。和`%md`的唯一不同在于当变量不足**m**位时，将在前面补足够数量的**0**而不是空格。

```cpp
#include <cstdio>
using namespace std;
int main(){
    int a = 123;
    int b = 12345678;
    printf("%05d\n",a);
    printf("%05d\n",b);
    return 0;
}
```

+ 输出：

```
00123
12345678
```

###### %.mf

+ %.mf可以让浮点数保留m位小数输出，精度是“四舍六入五成双”，具体而言为：
  + 5前为奇数，舍5入1；
  + 5前为偶数，舍5不进（0是偶数）。

#### 使用getchar()和putchar()输入/输出字符

+ `getchar()`用来输入单个字符，`putchar()`用来输出单个字符。
+ `getchar()`可以识别并读入换行符。

#### typedef

+ `typedef`能够给复杂的数据类型起一个别名，这样在使用过程中就可以使用别名来替换原来的写法。

```cpp
#include <cstdio>
using namespace std;
typedef long long LL;
int main(){
    LL a = 123456789123454321;
    printf("%lld\n",a);
    return 0;
}
```

+ 输出：

```
123456789123454321
```

### 选择结构

#### if语句

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202310171623893.png)

#### if语句的嵌套

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202310171624875.png)

#### switch语句

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202310171624551.png)

### 循环结构

#### while语句

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202310171628441.png)

* 在while语句中，只要条件A成立就一直执行省略号里面的内容。

#### do...while语句

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202310171630802.png)

* do...while语句会先执行省略号中的内容一次，然后才判断**条件A**是否**成立**，如果**条件A**成立，就继续反复执行省略号中的内容，直到某一次条件A**不再成立**，则退出循环。

#### for语句

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202310171634133.png)

+ for语句的具体格式如下：

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202310171634463.png)

#### break和continue语句

+ `break`语句不仅可以强制退出`switch`语句，而且break同样可以退出循环语句，即可以在需要的条件下直接退出循环。
+ `continue`语句的作用和`break`语句的作用有点相似，它可以在需要的地方临时结束循环的**当前轮回**，然后进入**下一轮回**。

### 数组

#### 一维数组

+ **数组**就是把**相同数据类型**的变量组合在一起而产生的**数据集合**，**数组**就是从某个地址开始**连续若干个位置**形成的元素集合。（*数组的地址是连续存放的*）
+ 一维数组的定义格式如下：

```
数据类型 数组名[数组大小]；
```

+ 数组大小必须是**整数常量**，不可以是变量。

#### 冒泡排序

+ 冒泡的本质是在于**交换**，即每次通过交换的方式把**当前剩余元素**的**最大值**移动到一端，而**当剩余元素**减少为**0**时，排序结束。

```cpp
#include <cstdio>
#include <math.h>
using namespace std;
int main(){
    int temp = 0;
    int a[7] = {3,6,10,9,4,8,7};//n=7
    for(int i=1;i<=6;i++)//整个过程执行n-1趟
    {
        //每一趟中将左边元素与右边相邻元素依次对比，若大的数在左边，则交换这两个数
        //当该趟结束的时候，该趟的最大数被移到了该趟剩余数的最右边
        for(int j=0;j<7-i;j++)
        {
            if(a[j]>a[j+1])
            {
                temp = a[j];
                a[j] = a[j+1];
                a[j+1] = temp;
            }
        }
    }
    for(int i=0;i<=6;i++)
    {
        printf("%d ",a[i]);
    }
    return 0;
}
```

#### 二维数组

+ 二维数组是一位数组的扩展：

```cpp
数据类型 数组名[第一维大小][第二维大小];
```

+ `int a[5][6]`数组的直观理解：

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202310182107408.png)

+ **特别提醒：**如果数组的大小较大，大概在$10^6$的级别，则**需要定义在主函数外面**，否则会使得程序异常退出，原因是函数内部申请的局部变量来自**系统栈**，所允许申请的**空间较小**；而函数外部申请的全局变量来自**静态存储区**，允许申请的**空间较大**。

#### memset——对数组中每个元素赋相同的初值

+ **需要注意的是**：`memset`使用的是按**字节**赋值，即对**每个字节**赋相同的值，这样的话，在`int`型数组中每个数据的**四个字节**都会被分配为**相同的值**，因此为了避免出错，只建议对非`char`型的数组赋值为**0**和**-1**；
+ 使用`memset`对数组赋值时需要用`#include<string.h>`头文件；

+ `memset`函数的格式为：

```cpp
memset(数组名，赋的数值，sizeof(数组名));
```

#### 字符数组

##### 字符数组的初始化

+ 和普通数组一样，字符数组也可以采用循环的方法初始化；
+ 除此之外，字符数组也可以通过**直接赋值字符串**来进行初始化（**仅限于初始化**，程序的其他位置不允许这样直接赋值整个字符串）

```cpp
#include <cstdio>
using namespace std;
int main(){
    char str[10] = "YUGIN!";
    for(int i=0; i<6;i++)
    {
        printf("%c",str[i]);
    }
    return 0;
}
```

+ 输出：

```
YUGIN!
```

##### 字符数组的输入输出

###### scanf输入，printf输出

+ `scanf`和`printf`对字符类型有`%c`和`%s`两种格式，其中`%c`用来输入**单个字符**，`%s`用来输入**一个字符串**并存在**字符数组**中。
+ `%c`格式能够识别**空格**和**换行符**并将其输入，`%s`通过**空格**或**换行符**来识别**一个字符串的结束**。

+ `scanf`在使用`%s`时，后面对应的数组名是不需要加`&`**取地址运算符**的。

```cpp
#include <cstdio>
using namespace std;
int main(){
    char str[10];
    scanf("%s",str);
    printf("%s",str);
    return 0;
}
```

+ 输入输出：

```
输入：test test test
输出：test
```

###### getchar输入，putchar输出

+ `getchar`和`putchar`分别用来输入和输出**单个字符**；
+ 输入和输出示例：

```cpp
#include <cstdio>
using namespace std;
int main(){
    char a;
    a=getchar();
    getchar();//可以用作吸收某些字符
    putchar(a);
    putchar('\n');
    return 0;
}
```

###### gets输入，puts输出

+ `gets`用来输入**一行字符串**（即**一个一维数组**，只有遇到`\n`时结束）
+ `puts`用来输出一行字符串（即一个一维数组，只有遇到`\n`时结束）

```cpp
#include <cstdio>
using namespace std;
int main(){
    char a[20];
    char b[4][10];
    gets(a);
    for(int i=0;i<2;i++)
    {
        gets(b[i]);
    }
    puts(a);
    for(int i=0;i<2;i++)
    {
        puts(b[i]);
    }
    return 0;
}
```

+ 输入输出示例：

```
输入：
this is 
yugin's
blog
输出：
this is 
yugin's
blog
```

##### 字符数组的存放方式

+ 由于**字符数组**是由若干个`char`类型的元素组成，因此**字符数组**的每一位都是一个`char`字符。
+ 在**一维数组**（或是**二维数组的第二维**）的末尾都有一个**空字符**`\0`，用于表示存放的**字符串的结尾**。

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202310221521845.png)

+ 特别注意：**空字符**`\0`的**ASCII**码为**0**，即空字符`NULL`，会占用一个**字符位**，因此在初始化的时候**数组长度**至少比**字符串长度**多一个长度。
+ 如果不是使用`scanf`函数的`%s`格式或`gets`函数输入字符串（例如使用`getchar`），则需要手动在字符数组最后加入`\0`，否则输出字符串会因为无法识别字符串末尾而输出**乱码**。

#### string.h头文件

+ `string.h`头文件包含了许多用于字符数组的函数。

##### strlen()函数

+ `strlen()`函数可以得到字符数组中第一个`\0`前的字符的个数并返回，其格式如下：

```cpp
len = strlen(字符数组)；
```

##### strcmp()函数

+ strcmp函数返回两个字符串大小的比较结果，比较原则是字典序，其格式如下：

```cpp
cmp = strcmp(字符数组1，字符数组2);
```

+ 字典序的解释：

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202310221535589.png)

##### strcpy()函数

+ `strcpy()`函数可以把一个字符串复制给另一个字符串，其格式如下：

```cpp
strcpy(字符数组1,字符数组2);
puts(字符数组1);
```

+ 注意：是把**字符数组2**复制给**字符数组1**，包括**结束符**`\0`；

##### strcat()函数

+ `strcat()`可以把一个字符串接到另一个字符串后面，其格式如下：

```cpp
strcpy(字符数组1,字符数组2);
puts(字符数组1);
```

+ 注意：是把**字符数组2**接到**字符数组1**后面；

##### sscanf()和sprintf()

+ `sscanf()`和`sprintf()`是处理字符串问题的利器！

+ `sscanf()`和`sprintf()`的使用格式如下：

```
sscanf(str,"%d",&n);
sprintf(str,"%d",n);
```

+ 上面`sscanf()`写法的作用是把字符数组`str`的中的内容以`"%d"`的格式写到`n`中（**从左到右**）。

```cpp
#include <cstdio>
#include <string.h>
using namespace std;
int main(){
    char a[20] = "123";
    int n=0;
    sscanf(a,"%d",&n);
    printf("%d",n);
    return 0;
}
```

+ 输出：

```
123
```

+ 上面`sprintf()`写法的作用是把`n`以`"%d"`的格式写到`str`字符数组中（**从右到左**）。

```cpp
#include <cstdio>
#include <string.h>
using namespace std;
int main(){
    char a[20];
    int n=123433;
    sprintf(a,"%d",n);
    printf("%s",a);
    return 0;
}
```

+ 输出：

```
123433
```

+ 上面的仅仅是简单的应用，实际上`sscanf()`和`sprintf()`可以进行更加复杂的字符串处理：

```cpp
#include <cstdio>
#include <string.h>
using namespace std;
int main(){
    char str[100];
    int n=520;
    double db=2002.080512121;
    char str2[20]="yugin!";
    char str3[20]="I";
    sprintf(str,"%s%d%s,%.4f",str3,n,str2,db);
    printf("%s",str);
    return 0;
}
```

+ 输出：

```
I520yugin!,2002.0805
```

+ 最后指出，`sscanf()`和`sprintf()`也可以支持正则表达式，则许多字符串问题将迎刃而解。

### 函数

+ 函数是一个实现一定功能的语句的集合，并在需要时可以反复调用而不必每次都重新写一遍。
+ 函数的基本语法格式：

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202310221718986.png)

#### 全局变量

+ 全局变量是指在定义之后的所有程序段内都有效的变量（即定义在所有函数之前）

#### 局部变量

+ 与全局变量相对，局部变量定义在函数内部，且只在函数内部生效，函数结束时局部变量便销毁。

#### 再谈main()函数

+ 主函数对一个程序而言只有一个，且无论主函数写在哪个位置，整个程序一定是从主函数的第一个语句开始执行，然后在需要时再调用其他函数。
+ `main()`函数的结构：

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202310221723044.png)

#### 以数组作为函数的参数

+ 函数的参数可以是数组，且数组作为参数时，参数中数组的第一维不需要填写长度（如果是二维数组，则**第二维需要填写长度**）
+ 数组作为参数时，在函数中对数组元素的修改就**等同于对原素组进行修改**（与普通的局部变量不同）

```cpp
#include <cstdio>
#include <string.h>
using namespace std;
//函数
void changStr(int a[],int b[][3])
{
    a[0]=1;
    a[1]=3;
    b[1][2]=5;
}
//主函数
int main(){
    int inter[5]={0};
    int in[2][3]={0};
    changStr(inter,in);
    printf("%d\n",inter[0]);
    printf("%d\n",inter[1]);
    printf("%d",in[1][2]);
    return 0;
}
```

+ 输出：

```
1
3
5
```

+ 注意：虽然数组可以作为参数，但是却不允许作为返回类型出现。

#### 函数的嵌套调用

+ 函数的嵌套调用是指在一个函数中调用另一个函数，调用方式和`main()`函数调用其他函数一样。

#### 函数递归调用

+ 函数递归调用是指一个函数调用该函数本身；

+ 类似下面计算n的阶乘的代码：

```cpp
#include <cstdio>
#include <string.h>
using namespace std;
//函数
int F(int n)
{
    if(n==0) return 1;
    else return F(n-1)*n;
}
//主函数
int main(){
    int a=3;
    printf("%d",F(a));
    return 0;
}
```

+ 输出：

```
6
```

### 指针

#### 什么是指针

+ 在C语言中，**指针**就是**内存地址**，**指针变量**是指用来**存放内存地址的变量**。
+ 在C/C++语言中，**指针**一般被认为是**指针变量**，指针变量的内容存储的是**其指向的对象的首地址**，指向的对象可以是**变量**（指针变量也是变量），**数组**，**函数**等**占据存储空间的实体**。
+ 只要在变量前面加上`&`，就表示变量的地址。
+ 指针是一个`unsigned`类型的函数。

#### 指针变量

+ 指针变量是用来存放指针（或者可以理解为地址）。
+ 在某种数据类型后加`*`来表示这是一个指针变量，定义如下：

```cpp
int *p;
double *p;
char *p;
```

+ 给指针变量赋值的方式一般是把变量的地址取出来，然后赋给对应类型的指针变量：

```cpp
int a;
int *p = &a;
```

+ 如果`p`是指针（即`p`保存的是某个数据类型的地址），则`*p`就是这个地址所存放的元素：

```cpp
#include <cstdio>
#include <string.h>
using namespace std;
//主函数
int main(){
    int a;
    int *p = &a;
    a=233;
    printf("%d",*p);
    return 0;
}
```

+ 输出：

```
233
```

+ 指针变量也可以进行加减法，其中**减法**的结果是两个地址偏移的距离。
+ 例如，对于`int*`类型的指针变量`p`而言，`p+1`是指`p`所指的int型变量的下一个`int`型变量地址，这个所谓的“下一个”是跨越了一整个`int`型（即**4Byte**）。
+ 指针变量也支持自增和自减的操作，示例如下：

```cpp
#include <cstdio>
#include <string.h>
using namespace std;
//主函数
int main(){
    int a;
    int *p = &a;
    a=233;
    printf("%d\n",p);
    printf("%d\n",p+1);
    p++;
    printf("%d",p);
    return 0;
}
```

+ 输出：

```
113245364
113245368
113245368
```

#### 指针与数组

+ **数组名称**作为**首地址**使用，因此`a == &a[0]`和`a+i == &a[i]`成立。

```cpp
#include <cstdio>
#include <string.h>
using namespace std;
//主函数
int main(){
    int a[10]={1,2,4,5,7};
    int *p = a;
    int *q;
    printf("%d\n",p);
    q=&a[5];
    printf("%d\n",q);
    printf("%d",q-p);
    return 0;
}
```

+ 输出：

```
1241512688
1241512708
5
```

+ `&a[0]`和`&a[5]`之间相差5个`int`（**4个Byte**），因此输出5。

#### 使用指针变量作为函数参数

+ 指针类型也可以作为**函数参数**的类型，这时视为把**变量的地址**传入函数。如果在函数中对这个地址中的元素进行改变，原先的数据就会确实地被改变。
+ 使用指针编写交换数据地函数：

```cpp
#include <cstdio>
#include <string.h>
using namespace std;
//交换函数
void my_swap(int *a,int *b)
{
    int temp;
    temp = *a;
    *a = *b;
    *b = temp;
}
//主函数
int main(){
    int a=1;
    int b=2;
    my_swap(&a,&b);
    printf("a=%d b=%d",a,b);
    return 0;
}
```

+ 输出：

```
a=2 b=1
```

#### 引用

##### 引用的含义

+ 引用是**C++**中一个强有力的语法，引用不产生**副本**，而是给原变量起了个**别名**。
+ 因此**对引用变量操作就是对原变量操作**。
+ 引用使用方法只需要在参数类型后面变量名前面加`&`就行，例子如下：

```cpp
#include <cstdio>
#include <string.h>
using namespace std;
//引用示例函数
void change(int &x)
{
    x=5;
}
//主函数
int main(){
    int b=88;
    change(b);
    printf("b=%d",b);
    return 0;
}
```

+ 输出：

```
b=5
```

+ 注意要把**引用**的`&`和取**地址运算符**`&`区分开来，引用并不是取地址的意思。

##### 指针的引用

+ 通过引用和函数来更改变量指针的地址：

```cpp
#include <cstdio>
#include <string.h>
using namespace std;
//交换函数
void my_swap(int* &p1,int* &p2)
{
    int* temp = p1;
    p1 = p2;
    p2 = temp;
}
//主函数
int main(){
    int a=1;
    int b=2;
    int* p_a = &a;
    int* p_b = &b;
    my_swap(p_a,p_b);//必须用指针变量传入，引用不可以使用常量。
    printf("a=%d b=%d",*p_a,*p_b);
    return 0;
}
```

+ 输出：

```
a=2 b=1
```

+ 需要强调的是，**引用**是产生**变量的别名**，因此**常量不可使用引用**，上述代码不可写成`my_swap(&a,&b);`，必须用**指针变量**进行传入。

### 结构体(struct)的使用

#### 结构体的定义

+ 定义一个结构体的基本格式如下：

```cpp
struct Name{
  //一些基本的数据结构或者自定义的数据类型
};
```

+ 结构体可以这样定义：

```cpp
struct studentInfo{
	int id;
	char gender;//'F'or'M'
	char name[20];
	char major[20];
}Alice,Bob,stu[1000];
```

+ 其中`studentInfo`是这个结构体的名字，内部定义了相关的数据。**大括号外**定义了**结构体变量**和**结构体数组**。

+ 结构体同样能够像基本数据类型那样定义：

```cpp
studentInfo Alice;
studentInfo stu[1000];
```

+ 值得注意的是，结构体里面能够定义除了自己本身之外的任何数据类型。

```cpp
struct node{
	node n;//不能定义node型变量，因为和本身一致
	node* next;//可以定义node*型指针变量
};
```

+ 虽然不能定义自己本身，但是可以定义自身类型的指针变量。

#### 访问结构体内的元素

+ 访问结构体内的元素有两种方法：`"."`和`"->"`操作。
+ 如果把`studentInfo`定义成如下：

```cpp
struct studentInfo{
	int id;
	char gender;//'F'or'M'
	studentInfo* next;
}stu,*p;
```

+ 这样`studentInfo`中多了一个指针`next`用来指向下一个学生的地址，且结构体变量中定义了**普通变量**`stu`和**指针变量**`p`。

+ 因此访问`stu`中的变量的写法如下：

```cpp
stu.id
stu.gender
stu.next
```

+ 访问指针变量`p`中的元素的写法如下：

```cpp
(*p).id
(*p).gender
(*p).next
```

+ 还有一种访问结构体指针变量内元素更加简洁的写法：

```cpp
p->id
p->gender
p->next
```

#### 结构体的初始化

+ 结构体的初始化推荐使用**构造函数**的方法。
+ 构造函数的特点是**函数名与结构体名一致**而且**不需要写返回函数**。
+ 其中自己定义构造函数的格式如下：

```cpp
struct studentInfo{
	int id;
	char gender;//'F'or'M'
	//以下构造函数的参数用于对结构体内部变量进行赋值
	studentInfo(int _id,char _gender)
	{
		id = _id;
		gender = _gender;
	}
};
```

+ 根据上述代码，即可直接对结构体的变量进行赋值：

```cpp
studentInfo stu = studentInfo(20020805,'M');
```

+ 需要注意，如果**自己重新定义了构造函数**，则不能不经初始化就定义结构体变量，如下定义能够适应更多不同的场合：

```cpp
struct studentInfo{
	int id;
	char gender;//'F'or'M'
	//原始构造函数，用以不初始化就定义结构体变量
	studentInfo(){}
	//只初始化gender的构造函数
	studentInfo(char _gender)
	{
		gender = _gender;
	}
	//以下构造函数的参数用于对结构体内部两个变量进行赋值
	studentInfo(int _id,char _gender)
	{
		id = _id;
		gender = _gender;
	}
};
```

### 补充

#### cin和cout

+ cin和cout是C++的输入输出函数，需要添加头文件`#include <iostream>`和`using namespace std;`才能使用。

##### cin

+ `cin`采用输入运算符`">>"`来进行输入，例如

```cpp
cin >> n >> db >> c >> str;
```

+ 如果想读入一整行，则需要`getline`函数：

```cpp
char str[100];
cin.getline(str,100);
```

+ 如果是`string`容器，则需要使用以下方式输入：

```cpp
char str[100];
getline(cin,str);
```

##### cout

+ `cout`采用输出运算符`"<<"`来进行输出，例如

```cpp
cout << n << db << c << '\n' << str << endl;
```

+ `endl`和`'\n'`都是表示换行的意思。
+ 由于`cin`和`cout`在输入和输出大量数据时表现糟糕，因此不建议使用。

#### 浮点数的比较

+ 由于计算机中采用有限二进制编码，存储并不总是准确，因此需要需要引入极小数`eps`来对这种误差进行纠正。
+ 圆周率`pi`的表达式可以使用`acos(-1.0)`来进行表示。

```cpp
const double esp = 1e-8;
const double pi = acos(-1.0);

#define Equ(a,b) (fabs(a-b)<eps)
```

### 黑盒测试

#### 单点测试

+ 对于单点测试而言，单点测试只需要按照正常逻辑执行一遍程序即可，是“一次性”的写法，即程序只需要一组数据能够完整执行即可。

#### 多点测试

+ 对于多点测试，要求程序能够一次运行所有数据，并要求所有输出的结果都必须正确。

##### while...EOF型

+ 当题目没有说明有多少数据读入时，就可以利用`scanf`返回值是否为`EOF`来判断输入是否结束。

```cpp
while(scanf("%d",&n) != EOF){
	...
}
```

+ 如果读入的是字符串，其对应写法如下：

```cpp
while(scanf("%s",str) != EOF){
	...
}
while(gets(str) != NULL){
	...
}
```

## 入门模拟

### 再谈字符串输入输出

+ 在比较早的`C/C++`版本中，经常可以看到推荐使用`gets`函数来进行整行字符串的输入，就像下面这样的简单写法即可输入一整行：

```cpp
gets(str);
```

+ 但是当输入的字符串长度超过数组长度上限`MAX_LEN`时，`gets`函数会把超出的部分也一并读进来，并且会覆盖数组之外的内存空间，这就导致了一定的安全风险，因此`C++11`标准将`gets`函数废弃了，然后在`C++14`时将该函数移除，如果现在想要整行输入的话，推荐使用`cin.getline`函数（见下文）。

```cpp
cin.getline(str, MAX_LEN);
```

+ 例如下面一道例题：

```cpp
//题目：输入一行字符串，然后直接输出这行字符串本身。
//输入描述：一行由大小写字母或空格组成的字符串，至少一个字符，不超过50个字符。
//输出描述：原样输出输入的字符串。
//**************************样例**************************
//输入：Huo Zhe Bu Jiu Shi Cang Cu Na Li You De Liao Ni Wo
//输出：Huo Zhe Bu Jiu Shi Cang Cu Na Li You De Liao Ni Wo
//**************************代码**************************
#include <cstdio>
#include <iostream>
using namespace std;
const int MAX_LEN = 1000000;
//主函数
int main(){
    char str[MAX_LEN];
    cin.getline(str,MAX_LEN);//由gets(str);函数换成了cin.getline(str,MAX_LEN);
    puts(str);
    return 0;
}
```

### 再谈sscanf()和sprintf()

#### 关于sscanf()

+ `sscanf`是C语言标准库中的一个函数，用于从字符串中读取格式化输入。在C++中也可以使用`sscanf`函数，但更常用的是使用C++标准库中的`stringstream`类来进行字符串解析。

+ `sscanf`函数的原型如下：

```cpp
int sscanf(const char* str, const char* format, ...);
```

+ 其中，`str`是要解析的字符串，`format`是格式化字符串，用于指定解析的规则，`...`是可变参数列表，用于接收解析出来的数据。

+ 与之相似的函数还有`scanf`和`fscanf`。其中，`scanf`从标准输入（通常是键盘）读取数据，而`fscanf`从文件中读取数据。

在使用`sscanf`函数时，需要注意以下几点：

- `format`字符串中可以包含格式说明符，如 `%d`, `%f`, `%s`, `%c`, `%x`, `%o`, `%u`, `%e`, `%g`, `%p`, `%n`, 等等。
- `format`字符串中可以包含空格、制表符、换行符等空白字符，用于跳过输入字符串中的空白字符。
- `format`字符串中可以包含方括号 `[]`，用于指定一个字符集合。例如，`%[a-z]` 表示匹配小写字母。
- `format`字符串中可以包含星号 `*`，表示跳过该项输入。
- `sscanf()` 函数**返回成功匹配并赋值的个数**。如果返回值小于参数个数，则表示解析失败。

基于最后一条性质可以实现下述例题：

+ 题目描述：

```
给定一个字符串，它可能是以下三种格式中的一种：

A is greater than B
A is equal to B plus C
No Information
其中前两种情况中的A、B、C均为正整数，而第三种情况中没有数字。请确认字符串代表的信息是否从算术上成立，如果成立，那么输出Yes；否则输出No；如果是第三种情况，那么输出三个问号（即???）。

注：

1、请将字符串整行读入后使用sscanf函数进行处理
```

+ 输入描述：

```
一行满足题意的字符串，其中A、B、C为不超过100的正整数。
```

+ 输出描述：

```
根据题意输出Yes、No或???。
```

+ 样例：

```
*******************样例1*****************
输入:
10 is greater than 5
输出:
Yes
*******************样例2*****************
输入:
6 is equal to 1 plus 3
输出:
No
*******************样例3*****************
输入:
No Information
输出:
???
```

+ 实现代码：

```cpp
#include <cstdio>
#include <iostream>
#include <string.h>
using namespace std;
const int MAX_LEN = 1000;
//主函数
int main(){
    int A = 0,B = 0,C = 0;
    char str[MAX_LEN];
    cin.getline(str,MAX_LEN);
    if(sscanf(str,"%d is greater than %d",&A,&B) == 2)//利用sscanf() 函数返回成功匹配并赋值的个数。
    {
       if(A>B)
       {
           printf("Yes");
       }
       else
       {
           printf("No");
       }
    }
    else if(sscanf(str,"%d is equal to %d plus %d",&A,&B,&C) == 3)//利用sscanf() 函数返回成功匹配并赋值的个数。
    {
        if(A==B+C)
        {
            printf("Yes");
        }
        else
        {
            printf("No");
        }
    }
    else
    {
        printf("???");
    }
    return 0;
}
```

+ 总结：利用`sscanf()` 函数返回成功匹配并赋值的个数，从而能够很好地解决问题。

#### 关于sprintf()

+ `sprintf`是C语言标准库中的一个函数，用于将格式化的数据写入字符串中。在C++中也可以使用`sprintf`函数，但更常用的是使用C++标准库中的`ostringstream`类来进行字符串解析。

+ `sprintf`函数的原型如下：

```c
int sprintf(char *str, const char *format, ...);
```

+ 其中，`str`是要写入的字符串，`format`是格式化字符串，用于指定写入的规则，`...`是可变参数列表，用于接收要写入的数据。

+ 与之相似的函数还有`printf`和`fprintf`。其中，`printf`将输出写入标准输出（通常是屏幕），而`fprintf`将输出写入文件。

在使用`sprintf`函数时，需要注意以下几点：

- `format`字符串中可以包含格式说明符，如 `%d`, `%f`, `%s`, `%c`, `%x`, `%o`, `%u`, `%e`, `%g`, `%p`, `%n`, 等等。
- `format`字符串中可以包含空格、制表符、换行符等空白字符，用于控制输出格式。
- `format`字符串中可以包含方括号 `[]`，用于指定一个字符集合。例如，`%[a-z]` 表示匹配小写字母。
- `sprintf()` **函数返回成功写入的字符数。**如果返回值小于0，则表示写入失败。

例题：[sprintf函数](https://sunnywhy.com/sfbj/2/5/37)

+ 代码：

```cpp
#include <cstdio>
#include <iostream>
#include <string.h>
using namespace std;
const int MAX_LEN = 1000;
//主函数
int main(){
    char str[MAX_LEN];
    int year,month,day,hour,minute,second;
    scanf("%d %d %d %d %d %d",&year,&month,&day,&hour,&minute,&second);
    sprintf(str,"%04d-%02d-%02d %02d:%02d:%02d",year,month,day,hour,minute,second);//注意此处的ssprintf()函数注释将需要的字符串写入到字符串数组中
    printf("%s",str);//注意此处字符串数组需要采用printf()函数进行输出
    return 0;
}
```

+ 总结：

+ 注意代码中的`ssprintf()`函数注释**将需要的字符串写入到字符串数组**中；
+ 注意代码最后的输出字符串数组需要采用`printf()`函数进行输出。

### 再谈结构体与函数数组传参

+ 例题：[结构体与构造函数II](https://sunnywhy.com/sfbj/2/8/43)
+ 代码：

```cpp
#include <cstdio>
#include <string.h>
//结构体
struct Student {
    int id;
    char name[15];
    //构造函数
    Student(){}
    Student(int _id,char _name[]){//函数中的数组传参
        id = _id;
        strcpy(name,_name);//复制字符串数组
    }
};
//主函数
int main() {
    Student student;
    char name[15];
    int id;
    scanf("%d",&id);
    getchar();
    scanf("%s",name);//读入字符串。
    student = Student(id,name);
    printf("%d\n%s",student.id,student.name);
    return 0;
}
```

+ 总结：注意上述代码中的函数数组传参，以及字符串数组赋值；
+ 注意如何利用`scanf()`函数读入字符串。

### 再谈cin和cout

+ 例题：[cin与cout](https://sunnywhy.com/sfbj/2/9/45)
+ 代码：

```cpp
#include <cstdio>
#include <string.h>
#include <iostream>
#include <iomanip>//数据格式控制函数的头文件
const int MAX_LEN  = 200;
using namespace std;
//主函数
int main(){
    int a;
    double b;
    char str[MAX_LEN];
    cin >> a >> b;
    getchar();
    cin.getline(str,MAX_LEN);
    //fixed()函数与setprecision(int n)并用，可以控制小数点后面有n位。注意：setprecision()函数是控制有效数字的位数，而fixed()函数与setprecision(int n )函数结合使用是保留小数点后的位数，小数点的保留采用四舍五入！
    cout << a << endl << fixed << setprecision(2) << b << endl << str;
    return 0;
}
```

+ 总结：
+ `#include <iomanip>`是数据格式控制函数的头文件；
+ 在使用`cout`函数输出的时候`fixed()`函数与`setprecision(int n)`并用，可以控制小数点后面有**n位**。注意：`setprecision()`函数是控制有效数字的位数，而`fixed()`函数与`setprecision(int n )`函数结合使用是保留小数点后的位数，小数点的保留采用四舍五入！
+ 如果只使用`setprecision(int n)` 函数效果如下：

```cpp
cout << setprecision(3) << 0.12345 << endl;
cout << setprecision(3) << 1.23456 << endl;

输出：
0.123
1.23
```

+ 当要保留对应位数的小数(**四舍五入**)的时候，就需要采用`fixed()`函数，效果如下：

```cpp
cout << fixed << setprecision(3) << 0.12345 << endl;
cout << fixed << setprecision(3) << 1.23456 << endl;

输出：
0.123
1.235
```

### 再谈浮点精度比较

+ 例题：[浮点精度比较](https://sunnywhy.com/sfbj/2/9/46)
+ 代码：

```cpp
#include <cstdio>
#include <string.h>
#include <cmath>
using namespace std;
const double eps = 1e-8;
//主函数
int main(){
    int a,b,c,d;
    scanf("%d%d%d%d",&a,&b,&c,&d);
    double res1 = a* asin(sqrt(b) / 2);
    double res2 = c* asin(sqrt(d) / 2);
    if(res1 - res2 > eps)//公式1>公式2
    {
        printf("1");
    }
    else if(res2 - res1 > eps)//公式2>公式1
    {
        printf("2");
    }
    else
    {
        printf("0");
    }
    return 0;
}
```

+ 总结：一般为了避免计算机精度误差造成浮点数大小比较不准，采用浮点数常量大小为`const double eps = 1e-8;`的数据来进行区分。

### 再谈if语句

+ `if(a==b==0)`和i`f(a==0&&b==0)`的区别：
+ 这两个表达式的区别在于它们的运算顺序不同。
  + `if(a==b==0)`的运算顺序是先比较a和b是否相等(`a==b`)，然后再将**结果**与0比较。如果a和b都为0，但是`true`不等于0，所以表达式`a==b==0`为`false`。而当a和b**不相等**时，表达式`a==b==0`为`true`。
  + `if(a==0&&b==0)`的运算顺序是先判断a是否等于0，然后再判断b是否等于0。只有当a和b都等于0时，这个表达式的结果才为`true`；否则，结果为`false`。
  + 因此，这两个表达式的含义是不同的。需要特别注意！

### 简单模拟

+ 简单模拟的题目不涉及算法，一般完全根据题目描述来进行代码编写，考察的是**代码能力**！

例题：[2的幂](https://sunnywhy.com/sfbj/3/1/61)

+ 代码：

```cpp
#include <cstdio>
#include <string.h>
#include <cmath>
using namespace std;
const int m = 1007;
//主函数
int main(){
    int num;
    scanf("%d",&num);
    int res=1;
    for(int i=1;i<=num;i++)
    {
        res = ((res%m)*(2%m))%m;
    }
    printf("%d",res);
    return 0;
}
```

+ 总结：该题的**数据大小**远大于**C++**中的`long long`类型，因此不能直接进行计算，需要根据题目提示的公式**进行简化**，从而正确计算得到结果！

例题：[B1032 挖掘机技术哪家强](https://pintia.cn/problem-sets/994805260223102976/exam/problems/994805289432236032?type=7&page=0)

+ 代码：

```cpp
#include <cstdio>
#include <string.h>
using namespace std;
//主函数
int list_chengji[100010]={0};
int main(){
    int num;
    int max_chengji=-1;//注意此处最大成绩设置为-1，否则无法通过最大成绩就是为0的测试点。
    int xuhao,chengji,res_xuhao;
    scanf("%d",&num);
    for(int i=0;i<num;i++){
        scanf("%d %d",&xuhao,&chengji);
        list_chengji[xuhao]+=chengji;
    }
    for(int k=1;k<100010;k++)
    {
        if(list_chengji[k]>max_chengji)
        {
            max_chengji = list_chengji[k];
            res_xuhao = k;
        }
    }
    printf("%d %d\n",res_xuhao,max_chengji);
    return 0;
}
```

+ 总结：这道题目要**细心**，注意在代码中计算最大成绩的时候**初始值**要设置为`-1`，否则无法通过最大成绩就是为**0**的测试点。

### 查找元素

+ 查找元素类题目：给定一些元素，然后查找某个满足某条件的元素。
+ 一般而言，如果需要在一个比较小范围的数据集中查找，那么直接遍历每一个数据即可。
+ 如果需要查找的范围比较大，可以采用**二分查找**等算法来进行更快速的查找。

例题：[寻找元素对](https://sunnywhy.com/sfbj/3/2/64)

+ 代码：

```cpp
#include <cstdio>
#include <string.h>
using namespace std;
//主函数
int main(){
    int n;
    scanf("%d",&n);
    int list[1010];
    for(int i=0;i<n;i++)
    {
        scanf("%d",&list[i]);
    }
    int x,flag=0;
    scanf("%d",&x);
    for(int k=0;k<n-1;k++)
    {
        for(int j=k+1;j<n;j++)
        {
            if(x==list[k]+list[j])
            {
                flag++;
            }
        }
    }
    printf("%d",flag);
    return 0;
}
```

### 图形输出

+ 所谓图形，其实是由若干字符组成，因此只需要弄清楚规则就能编写代码，有以下两种方法：
  + 通过规律直接进行输出；
  + 定义一个二维字符数组，通过规律填充字符数组，最后再输出整个二维数组。

例题：[画X](https://sunnywhy.com/sfbj/3/3/67)

+ 代码：

```cpp
#include <cstdio>
#include <string.h>
using namespace std;
//主函数
int main(){
    int n;
    char list[101][101];
    memset(list,' ',sizeof(list));//初始化数组
    scanf("%d",&n);
    for(int i=0;i<n;i++)
    {
        for(int k=0;k<n;k++)
        {
            if(i<n/2||i>n/2)
            {
                if(k==i||k==n-1-i)
                {
                    list[i][k]='*';
                }
            }
            else if(i==n/2)
            {
                if(k==i)
                {
                    list[i][k]='*';
                }
            }
        }
    }
    for(int i=0;i<n;i++)
    {
        if(i<=n/2)
        {
            for(int k=0;k<n-i;k++)
            {
                printf("%c",list[i][k]);
            }
        }
        else
        {
            for(int k=0;k<=i;k++)
            {
                printf("%c",list[i][k]);
            }
        }
        printf("\n");
    }
    return 0;
}
```

+ 总结：这类型题目主要在于找到图案的规律，若图案比较复杂可以放在二维字符数组中进行输出，注意一下二维字符数组的初始化可以采用`memset(list,' ',sizeof(list));`函数！

### 日期处理

+ 日期处理问题主要考虑平年和闰年的关系(由此产生的二月天数之间的差别)、大月和小月的问题，细节比较繁杂！

例题：[周几](https://sunnywhy.com/sfbj/3/4/73)

+ 代码：

```cpp
#include <cstdio>
#include <string.h>
using namespace std;
//初始化平年闰年的数组
//0是平年，1是闰年
int year_list[2][13]={
        {0,31,28,31,30,31,30,31,31,30,31,30,31},
        {0,31,29,31,30,31,30,31,31,30,31,30,31}
};
//是否闰年判断函数
bool leap_year(int year)
{
    if(year%400==0||(year%4==0&&year%100!=0))
    {
        return true;
    }
    else
    {
        return false;
    }
}
//判断日期前后,如果day在day1之前就是true，否则为false
bool before_afer(int year,int month,int day,int year1,int month1,int day1)
{
    if(year1-year>0)
    {
        return true;
    }
    else if(year1-year<0)
    {
        return false;
    }
    else if(year1-year==0)
    {
        if(month1-month>0)
        {
            return true;
        }
        else if(month1-month<0)
        {
            return false;
        }
        else if(month1==month)
        {
            if(day1-day>0)
            {
                return true;
            }
            else if(day1-day<=0)
            {
                return false;
            }
        }
    }
}
//计算两个日期之间的天数差值,day在day1之前，采用日期减法
int count_days(int year,int month,int day,int year1,int month1,int day1)
{
    int num=0;
    if(year1==year&&month1==month&&day1==day)
    {
        return 0;
    }
    else
    {
        while(true)
        {
            day1--;
            if(day1<1)
            {
                month1--;
                if(month1<1)
                {
                    year1--;
                    month1=12;
                }
                day1=year_list[leap_year(year1)][month1];
            }
            num++;
            if(year1==year&&month1==month&&day1==day)
            {
                break;
            }
        }
        return num;
    }
}
//主函数
int main(){
    int year,month,day;
    scanf("%d-%d-%d",&year,&month,&day);
    int num=0,shengyu=0;
    //2021-05-02是周日，用0表示（用这个日期作为基准）
    bool b_a = before_afer(2021,5,2,year,month,day);
    if(b_a)
    {
        num=count_days(2021,5,2,year,month,day);
        shengyu=num%7;
        printf("%d",shengyu);
    }
    else
    {
        num=count_days(year,month,day,2021,5,2);
        shengyu=num%7;
        if(shengyu==0)
        {
            printf("%d",0);
        }
        else
        {
            printf("%d",7-shengyu);
        }
    }
    return 0;
}
```

+ 总结：虽然本题我采用了日期减法作为函数进行运算，但是和日期加法的想法相似，主要思想如下：

  + 直接给日期加上指定的天数并不是很容易的事情，所以我们可以换个思路，**每次只加1天**，**一直加到指定的天数为止**。这样我们就把问题转换为计算加1天之后的新日期，而这个问题就相对简单许多。
  + 假设当前日期的年、月、日分别是year、month、day，那么加一天之后 day 就变成了 day+1，之后我们需要判断这个新的day是否超过了当前月份month 所拥有的总天数，如果没超过，那么相安无事，算法结束；如果超过了，那么就需要令月份month 加 1、同时让day重置为 1（即把日期变为下一个月的 1 号）。接下来，如果加了 1 之后的月份 month 变为了 13 月，那么就需要令年份year加 1、同时置月份 month重置为 1（即把日期变为下一年的 1 月）。
  + 这个过程需要知道每个月有多少天，为了方便直接取出每个月的天数，不妨设置一个二维数组`int year_list[2][13]`，用来存放每个月的天数，其中第一维为 0 时表示平年，为 1 时表示闰年。至于平年和闰年的判断方式也很简单：年份是 400 的倍数时是闰年，年份是 4 的倍数但不是 100 的倍数时也是闰年，其他情况都是平年。
  + 代码如下：

  ```cpp
  #include <cstdio>
  #include <string.h>
  using namespace std;
  //初始化平年闰年的数组
  //0是平年，1是闰年
  int year_list[2][13]={
          {0,31,28,31,30,31,30,31,31,30,31,30,31},
          {0,31,29,31,30,31,30,31,31,30,31,30,31}
  };
  //是否闰年判断函数
  bool leap_year(int year)
  {
      if(year%400==0||(year%4==0&&year%100!=0))
      {
          return true;
      }
      else
      {
          return false;
      }
  }
  //主函数
  int main(){
      int year,month,day;
      scanf("%d-%d-%d",&year,&month,&day);
      int n;
      scanf("%d",&n);
      for(int i=1;i<=n;i++)
      {
          day++;
          if(day>year_list[leap_year(year)][month])
          {
              month++;
              day=1;
              if(month>12)
              {
                  year++;
                  month=1;
              }
          }
      }
      printf("%04d-%02d-%02d", year, month, day);   // 按格式输出年月日
      return 0;
  }
  ```

  + 最后，这道例题的思考方式如下：**首先确认一个基准日期**->**(2021-05-02是周日，用0表示)**->**计算输入的日期在基准日期之前或者之后**->**计算相差多少天**->**最后计算输入的日期是周几**！
  + 通过上述步骤，该题迎刃而解！

### 进制转换

对于一个p进制数需要转换为q进制数，一般需要分为以下两步：

+ p进制数x转十进制数y：
  ![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202310311125587.png)

  + 实现代码：

 ```cpp
    //p进制数x转10进制数y的函数
    int p_ten(int x,int p)
    {
        int y=0,product=1;
        while(x!=0)
        {
            y=y+(x%10)*product;
            x=x/10;
            product=product*p;
        }
        return y;
    }
 ```

+ 十进制数y转q进制数z的函数(除基取余法)：

    + 采用"除基取余法"，意思是每次将带转换的数除q，将得到的余数作为低位存储，而商继续除q并进行上面的操作，最后当商为0时，将所有位从高到低输出就可以得到z！
      
    + 例如十进制数**11**转换为**二进制**：
      
      ![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202310311132044.png)
      
    + 实现代码：

    + 代码中采用`do...while`而不是`while`的原因是如果十进制恰好是**0**会造成直接跳出循环导致结果出错，因此采用`do...while`语句。

```cpp
//十进制数y转q进制数z的函数(除基取余法)
int ten_q(int y,int q,int z_list[])
{
    int num=0,z=0;
    do {
        z_list[num]=y%q;
        num++;
        y=y/q;
    }while(y!=0);
    return num;
}
```

例题：[K进制转十进制](https://sunnywhy.com/sfbj/3/5/77)

+ 代码：

```cpp
#include <cstdio>
#include <string.h>
#include <cmath>
using namespace std;
//p进制数x转10进制数y的函数
int p_ten(int x,int p)
{
    int y=0,product=1;
    while(x!=0)
    {
        y=y+(x%10)*product;
        x=x/10;
        product=product*p;
    }
    return y;
}
//十进制数y转q进制数z的函数(除基取余法)
int ten_q(int y,int q,int z_list[])
{
    int num=0,z=0;
    do {
        z_list[num]=y%q;
        num++;
        y=y/q;
    }while(y!=0);
    return num;
}
//主函数
int main(){
    char str[10];//用于存储k进制串
    int k,str_len;
    int sum=0;
    scanf("%s %d",str,&k);
    str_len = strlen(str);
    for(int i=0;i<str_len;i++)
    {
        if(str[i]>='A'&&str[i]<='F')
        {
            sum+=(str[i]-'A'+10)*pow(k,str_len-1-i);
        }
        else
        {
            sum+=(str[i]-'0')*pow(k,str_len-1-i);
        }
    }
    printf("%d",sum);
    return 0;
}
```

+ 总结：这道例题无法直接使用上述两个函数，因此需要根据题意重新构造，但是难度不大，需要处理十进制以上的数据。

### 字符串处理

+ 字符串处理类题目可能实现逻辑比较麻烦，而且需要考虑许多细节和边界情况，因此是一种很好体现代码能力的题型。

例题：[单词倒序](https://sunnywhy.com/sfbj/3/6/79)

```cpp
#include <cstdio>
#include <string.h>
#include <iostream>
using namespace std;
const int MAXN = 1010;
//主函数   
int main(){
    char str[MAXN],str2[MAXN];
    cin.getline(str,MAXN);
    int str_len = strlen(str);
    int flag=0,m=0;
    for(int i=str_len-1;i>=0;i--)
    {
        flag++;
        if(str[i]==' ')
        {
            for(int j=i+1;j<=i+flag-1;j++)
            {
                str2[m]=str[j];
                m++;
            }
            str2[m]=' ';
            m++;
            flag=0;
        }
        else if(i==0)
        {
            for(int j=i;j<=i+flag-1;j++)
            {
                str2[m]=str[j];
                m++;
            }
            str2[m]=' ';
            m++;
            flag=0;
        }
    }
    str2[str_len]='\0';
    for(int k=0;k<str_len;k++)
    {
        printf("%c",str2[k]);
    }
    return 0;
}
```

+ 总结：细心分析，按照逻辑编写代码，问题即可迎刃而解。

例题：[公共前缀](https://sunnywhy.com/sfbj/3/6/83)

+ 代码：

```cpp
#include <cstdio>
#include <string.h>
#include <iostream>
#include <algorithm>
using namespace std;
const int MAXN = 55;
//主函数
int main(){
    char str[MAXN][MAXN];
    int n;
    int min_strlen=100,num=0,flag=0;
    scanf("%d",&n);
    getchar();//吸收换行符
    for(int i=0;i<n;i++)
    {
        cin.getline(str[i],MAXN);
        if(min_strlen>(int)strlen(str[i]))
        {
            min_strlen=(int)strlen(str[i]);
        }
    }
    for(int i=0;i<min_strlen;i++)
    {
        for(int k=0;k<n-1;k++)
        {
            if(str[k][i]!=str[k+1][i])
            {
                flag++;
            }
        }
        if(flag)
        {
            num=i-1;
            break;
        }
        num=i;
    }
    //printf("%d",min_strlen);
    for(int i=0;i<=num;i++)
    {
        printf("%c",str[0][i]);
    }
    return 0;
}
```

+ 总结：注意一下本题中在需要使用循环输入的时候要采用`getchar();`函数吸收一下换行符，否则换行符会输入至字符数组中！

## C++标准模板库(STL)介绍
### vector的常见用法详解
+ `vector`->变长数组，即"长度根据需要而自动改变的数组";
+ 要使用 `vector`，需要添加 `vector` 头文件，即 `#include <vector>`;
#### vector 的定义
+ 单独定义一个 `vector`：
```cpp
vector<typename> name;
```
+ 上面 `vector<typename> name` 的定义相当于一维数组 `typename name[size]` ，只是其长度可以根据需要进行变化，比较**节省空间->变长数组**。
+ 与一维数组一样，上述 `typename` 可以是**任何基本类型**，如 `int`、`double`、`char`、结构体等；
+ 也可以是 STL 标准容器，如 `vector`、`set`、`queue` 等；
+ 如果 `typename` 也是一个 STL 容器，定义的时候需要将 `>>` 变为 `> >`:
```cpp
vector<vector<int> > name;//>>要加上空格
```
+ 对于二维数组定义，有以下两种方法：
1. 第一种定义方法：
```cpp
vector<typename> Arrayname[arraySize];
vector<int> vi[100];
```
+ 这样 `Arrayname[0]` 至 `Arrayname[arraySize-1]` 中每一个都是一个 `vector` 容器。
2. 第二种定义方法：
```cpp
vector<vector<int> > Arrayname;//>>要加上空格
```
+ 与第一种定义方法不同，上述写法的一维长度已经固定为 `arraySize`，另一维才是“变长”的；
+ 而第二种写法两个维度都是“变长”的。
#### vector 容器内元素的访问
vector 一般有一下两种访问方式：
1. 通过下标访问
2. 通过迭代器访问
##### 通过下标访问
+ 与访问普通数组一样，对于一个定义为 `vector<int> vi;` 的 `vector` 的容器而言，直接访问 `vi[index]` 即可（如 `vi[0]`、`vi[1]`）。
+ 当然，下标是从 `0` 到 `vi.size ()-1`,否则访问超出这个范围内的元素可能会运行出错。
##### 通过迭代器访问
+ 迭代器 (iterator)可以理解为一种类似**指针**的东西，其定义如下：
```cpp
vector<typename>::iterator it;
```
+ 这样 `it` 就是一个 `vector<typename>::iterator` 型的变量，其中 `typename` 就是定义 `vector` 时填写的类型。
+ 得到迭代器 `it`，就可以通过 `*it` 来访问 `vector` 里的元素。
```cpp
vector<int> vi;
for(int i=1;i<=5;i++)
{
	vi.push_back(i);//vi.push_back(i);是在vi的末尾添加元素i，即添加1 2 3 4 5
}
```
+ 可以通过类似下标和指针访问数组的方式来访问容器内的元素：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <vector>
#include <iostream>
#include <algorithm>
#include <cmath>
using namespace std;
int main()
{
    vector<int> vi;
    for(int i=1;i<=5;i++)
    {
        vi.push_back(i);//vi.push_back(i);是在vi的末尾添加元素i，即添加1 2 3 4 5
    }
    //vi.begin()为取vi的首元素地址，而it指向这个地址
    vector<int>::iterator it = vi.begin();
    for(int i=0;i<5;i++)

    {
        printf("%d ",*(it+i));//输出vi[i]
    }
    system("pause");    // 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出结果：
```
1 2 3 4 5 
```
+ 从上述程序不难看出，`vi[i]`和`*(vi.begin()+i)`是等价的。
+ 关于 `vector` 两个函数的说明：
1. `vi.begin()` 函数的作用是为取 vi 的首元素地址；
2. `vi.end()` 函数的作用是取尾元素地址的**下一个地址**，`end()` 作为迭代器的末尾标志，不存储任何元素。
+ 除此之外，迭代器还实现了两种自加（自减操作同理）操作：
1. `++it` 和 `--it`
2. `it++`和 `it--`
+ 于是有另一种遍历 `vector` 中元素的写法：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <vector>
#include <iostream>
#include <algorithm>
#include <cmath>
using namespace std;
int main()
{
    vector<int> vi;
    for(int i=1;i<=5;i++)
    {
	    vi.push_back(i);//vi.push_back(i);是在vi的末尾添加元素i，即添加1 2 3 4 5
    }
    //vector的迭代器不支持it<vi.end()写法，因此循环条件只能使用it!=vi.end()
    vector<int>::iterator it = vi.begin();
    for(it=vi.begin();it!=vi.end();it++)
    {
        printf("%d ",*it);
    }
    system("pause");    // 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```
1 2 3 4 5 
```
+ 需要指出的是，在常用 STL 容器中，只有在 `vector` 和 `string` 中，才允许使用类似于 `vi.begin()+3` 这种迭代器加上**整数**的写法。
#### vector 常用函数实例解析
##### push_back()
+ 顾名思义，`push_back(x)` 就是在 vector 后面添加一个元素 `x`，时间复杂度为 $O(1)$。示例如下：
```cpp
vector<int> vi;
for(int i=1;i<=5;i++)
{
	vi.push_back(i);//vi.push_back(i);是在vi的末尾添加元素i，即添加1 2 3 4 5
```
##### pop_back()
+ 有添加就有删除，`pop_back()` 用以删除 vector 的尾元素，时间复杂度为 $O(1)$。示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <vector>
#include <iostream>
#include <algorithm>
#include <cmath>
using namespace std;
int main()
{
    vector<int> vi;
    for(int i=1;i<=5;i++)
    {
        vi.push_back(i);//vi.push_back(i);是在vi的末尾添加元素i，即添加1 2 3 4 5
    }
    vi.pop_back();//删除vi的尾元素5
    //vi.begin()为取vi的首元素地址，而it指向这个地址
    vector<int>::iterator it = vi.begin();
    for(int i=0;i<vi.size();i++)

    {
        printf("%d ",*(it+i));//输出vi[i]
    }
    system("pause");    // 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```
1 2 3 4
```
##### size()
+ `size()` 用来获取 `vector` 中的元素个数，时间复杂度为 $O(1)$。`size()` 返回的是 `unsigned` 类型，不过一般而言使用 `%d` 不会出现太大问题。这一点，对于所有 STL 容器都是一样的。示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <vector>
#include <iostream>
#include <algorithm>
#include <cmath>
using namespace std;
int main()
{
    vector<int> vi;
    for(int i=1;i<=5;i++)
    {
        vi.push_back(i);//vi.push_back(i);是在vi的末尾添加元素i，即添加1 2 3 4 5
    }
    printf("%d",vi.size());
    system("pause");    // 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```
5
```
##### clear()
+ `clear()` 用来清空 vector 中的所有元素，时间复杂度为 $O(N)$，其中 N 为 `vector` 中元素的个数。示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <vector>
#include <iostream>
#include <algorithm>
#include <cmath>
using namespace std;
int main()
{
    vector<int> vi;
    for(int i=1;i<=5;i++)
    {
        vi.push_back(i);//vi.push_back(i);是在vi的末尾添加元素i，即添加1 2 3 4 5
    }
    vi.clear();
    printf("%d",vi.size());
    system("pause");    // 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```
0
```
##### insert()
+ `insert(it,x)` 用来向 `vector` 的任意迭代器 `it` 处插入一个元素 `x`，时间复杂度 $O(N)$。示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <vector>
#include <iostream>
#include <algorithm>
#include <cmath>
using namespace std;
int main()
{
    vector<int> vi;
    for(int i=1;i<=5;i++)
    {
        vi.push_back(i);//vi.push_back(i);是在vi的末尾添加元素i，即添加1 2 3 4 5
    }
    vector<int>::iterator it = vi.begin();
    vi.insert(it+2,-1);
    for(int i=0;i<vi.size();i++)
    {
        printf("%d ",*(it+i));//输出vi[i],1 2 -1 3 4 5 
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```
1 2 -1 3 4 5 
```
##### erase()
+ `erase()` 有两种用法：删除单个元素和删除一个区间内的所有元素。时间复杂度为 $O(N)$。
1. 删除单个元素
+ `erase(it)` 即删除迭代器为 `it` 处的元素。示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <vector>
#include <iostream>
#include <algorithm>
#include <cmath>
using namespace std;
int main()
{
    vector<int> vi;
    for(int i=1;i<=5;i++)
    {
        vi.push_back(i);//vi.push_back(i);是在vi的末尾添加元素i，即添加1 2 3 4 5
    }
    vector<int>::iterator it = vi.begin();
    vi.insert(it+2,-1);
    for(int i=0;i<vi.size();i++)
    {
        printf("%d ",*(it+i));//输出vi[i],1 2 -1 3 4 5 
    }
    printf("\n");
    vi.erase(it+2);
    for(int i=0;i<vi.size();i++)
    {
        printf("%d ",*(it+i));//输出vi[i],1 2 3 4 5 
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```
1 2 -1 3 4 5 
1 2 3 4 5
```
2. 删除一个区间内的所有元素
+ `erase(first, last)` 即删除 `[first, last)` 内的所有元素。示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <vector>
#include <iostream>
#include <algorithm>
#include <cmath>
using namespace std;
int main()
{
    vector<int> vi;
    for(int i=1;i<=5;i++)
    {
        vi.push_back(i);//vi.push_back(i);是在vi的末尾添加元素i，即添加1 2 3 4 5
    }
    vector<int>::iterator it = vi.begin();
    vi.insert(it+2,-1);
    for(int i=0;i<vi.size();i++)
    {
        printf("%d ",*(it+i));//输出vi[i],1 2 -1 3 4 5 
    }
    printf("\n");
    vi.erase(it+2,it+4);
    for(int i=0;i<vi.size();i++)
    {
        printf("%d ",*(it+i));//输出vi[i],1 2 4 5 
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```
1 2 -1 3 4 5 
1 2 4 5
```
+ 由上面的内容可以知道，要删除 vector 内的所有元素，可以使用 `vi.erase(vi.begin(),vi.end())`;
+ 当然，最方便的方法是使用 `vi.clear()`。
#### vector 的常见用途
##### 存储数据
1. `vector` 本身可以作为数组使用，而且在一些元素个数不确定的场合可以很好地节省空间。
2. 有些场合需要根据一些条件把部分数据输出在同一行，数据中间用空格隔开。由于输出数据的个数是**不确定**的，为了更方便地处理最后一个满足条件的数据后面不输出额外的空格，可以先用 `vector` 记录所有需要输出的数据，最后**一次性输出**。
##### 用邻接表存储图
1. 使用 `vector` 实现邻接表可以让一些**对指针不太熟悉**的使用者有一个比较方便的写法。
#### vector 的例题
例题：[子集I](https://sunnywhy.com/sfbj/4/3/129)
+ 代码：
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <cmath>
#include <vector>
using namespace std;
vector<vector<int> > result;
vector<int> temp;
int n;

//递归输出子集1函数
void F(int index)
{
    //递归边界
    if(index == n+1)
    {
        result.push_back(temp);
    }
    //递归式
    else
    {
        temp.push_back(index);
        F(index+1);
        temp.pop_back();
        F(index+1);
    }
}

//比较函数，从小到大排序
bool cmp(vector<int> &a,vector<int> &b)//引用的写法
{
    if(a.size()!=b.size())
        return a.size()<b.size();
    else
        return a<b;
}

//主函数
int main(){
    scanf("%d",&n);
    F(1);
    sort(result.begin(),result.end(),cmp);
    for(int i=0;i<result.size();i++)
    {
        for(int j=0;j<result[i].size();j++)
        {
            printf("%d",result[i][j]);
            if(j!=result[i].size()-1)
                printf(" ");
        }
        printf("\n");
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 总结：这道题目巧妙应用了 `vector` 是变长数组的特性，并且使用递归的方法实现。注意对二维 `vector` 变量进行排序的时候可以使用**引用**的方法。

例题：[子集III](https://sunnywhy.com/sfbj/4/3/131)
+ 代码：
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <cmath>
#include <vector>
using namespace std;
vector<vector<int> > result;
vector<int> temp;
int num[15];
int n;

//递归输出子集1函数
void F(int index)
{
    //递归边界
    if(index == n+1)
    {
        result.push_back(temp);
    }
        //递归式
    else
    {
        temp.push_back(num[index]);
        F(index+1);
        temp.pop_back();
        F(index+1);
    }
}

//比较函数，从小到大排序
bool cmp(vector<int> &a,vector<int> &b)//引用的写法
{
    if(a.size()!=b.size())
        return a.size()<b.size();
    else
        return a<b;
}

//主函数
int main(){
    scanf("%d",&n);
    for(int i=1;i<=n;i++)
    {
        scanf("%d",&num[i]);
    }
    F(1);
    sort(result.begin(),result.end(),cmp);
    vector<vector<int> >::iterator it = result.begin();
    for(int i=0;i<result.size()-1;i++)
    {
        if(result[i].size()==result[i+1].size())
        {
            int flag=0;
            for(int j=0;j<result[i].size();j++)
            {
                if(result[i][j]!=result[i+1][j])
                {
                    flag++;
                }
            }
            if(!flag)
            {
                result.erase(it+i+1);
                i--;
            }
        }
    }
	//剔除重复部分
    for(int i=0;i<result.size();i++)
    {
        for(int j=0;j<result[i].size();j++)
        {
            printf("%d",result[i][j]);
            if(j!=result[i].size()-1)
                printf(" ");
        }
        printf("\n");
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 总结：这道题目相较于第一题多了**剔除重复部分**的代码。

例题：[单词排列](https://sunnywhy.com/sfbj/4/3/138)
+ 代码：
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <cmath>
#include <string>
#include <vector>
using namespace std;
const int MAX = 300;
int n;
bool hashTable[MAX] = {false};
string str,temp;
vector<string> result;

//递归输出单词排序数
void F(int index)
{
    //递归边界
    if(index == n)
    {
        result.push_back(temp);
    }
    //递归式
    for(int k=0;k<n;k++)
    {
        if(!hashTable[str[k]])
        {
            hashTable[str[k]]=true;
            temp.push_back(str[k]);
            F(index+1);
            hashTable[str[k]]=false;
            temp.pop_back();
        }
    }
}

//主函数
int main(){
    cin>>str;
    n=str.length();
    F(0);
    sort(result.begin(),result.end());//按照字典序排列
    for(int i = 0; i < result.size(); i++) {
        cout << result[i] << endl;
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 总结：这道题思路与**全排列**的题目是一致，需要注意的是 `string` 和 `vector` 的用法

### set 的常见用法详解
+ `set` 翻译为集合，是一个内部**自动有序**而且**不包含重复元素**的容器；
+ 当有可能出现需要去掉重复元素的情况，而且有可能因为这些元素比较大或者类型不是 `int` 型而不能直接开散列表；
+ 上述情况可以使用 `set` 来保留元素本身而不考虑它的个数，而且 `set` 提供了更为直观的接口，并且加入 `set` 之后可以实现自动排序；
+ 要使用 set，需要添加 set 头文件，即 `#include <set>`,并且需要加上`using namespace std;`

#### set 的定义
+ 单独定义一个 `set`：
```cpp
set<typename> name;
```
+ typename 依然可以是任何基本类型，如 `int`、`double`、`char`、结构体等，或者是 `STL` 标准容器，例如 `vector`、`set`、`queue` 等。
```cpp
set<int> num;
set<vector<int> > num;
```
+ `set` 数组定义和 vector 也相同：
```cpp
set<typename> Arrayname[arraySize];
set<int> a[100];
```
+ 这样 `Arrayname[0]` 到 `Arrayname[arraySize-1]` 中每一个都是一个 `set` 容器。

#### set 容器内元素的访问
+ set 只能通过迭代器 (`iterator`)访问：
```cpp
set<typename>::iterator it;
set<int>::iterator it;
set<char>::iterator it;
```
+ 这样就得到了迭代器 `it`，并且可以通过 `*it` 来访问 set 里的元素。
+ 值得注意的是，除了 vector 和 string 之外的 STL 容器都不支持 `*(it+i)` 的访问方式，因此只能按照以下方式枚举：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <set>
using namespace std;
//主函数
int main()
{
    set<int> st;
    st.insert(3);//insert(x)将x插入set中
    st.insert(5);
    st.insert(2);
    st.insert(3);
    //注意，不支持it < st.end()的写法
    for(set<int>::iterator it = st.begin();it!=st.end();it++)
    {
        printf("%d ",*it);
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```
2 3 5 
```
+ 可以发现，`set` 内的元素自动递增排序，且自动去除了重复元素。

#### set 常用函数实例解析
##### insert()
+ `insert(x)` 可以将 `x` 插入 `set` 容器中，并自动排序和去重，时间复杂度为 $O(logN)$，其中 `N` 为 `set` 内的元素个数。
##### find()
+ `find(value)` 返回 set 中对应值为 `value` 的迭代器，时间复杂度为 $O(logN)$，其中 `N` 为 `set` 内的元素个数。
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <set>
using namespace std;
//主函数
int main()
{
    set<int> st;
    st.insert(3);//insert(x)将x插入set中
    st.insert(5);
    st.insert(2);
    st.insert(3);
    //注意，不支持it < st.end()的写法
    set<int>::iterator it=st.find(2);
    printf("%d",*it);
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```
2
```
+ 注意当 `st.find(x)` 函数没有找到相对应的元素时，返回的是 `st.end()`。
例题：[set-find与erase迭代器](https://sunnywhy.com/sfbj/6/2/248)
+ 代码：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <set>
using namespace std;
//主函数
int main()
{
    set<int> st;
    int num,n,k;
    scanf("%d %d",&n,&k);
    for(int i=0;i<n;i++)
    {
        scanf("%d",&num);
        st.insert(num);
    }
    set<int>::iterator it = st.find(k);
    if(it!=st.end())//注意当 `st.find(x)` 函数没有找到相对应的元素时，返回的是 `st.end()`
        st.erase(it);
    for(set<int>::iterator it=st.begin();it!=st.end();it++)
    {
        if(it!=st.begin())
            printf(" ");
        printf("%d",*it);
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```

##### erase()
+ `erase()` 有两种用法：
1. 删除单个元素；
2. 删除一个区间内所有元素。
+ 删除单个元素有两种方法：
+ `st.erase(it)`，`it` 为所需要删除元素的迭代器。时间复杂度为 $O(1)$。可以结合 `find()` 函数来使用，示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <set>
using namespace std;
//主函数
int main()
{
    set<int> st;
    st.insert(3);//insert(x)将x插入set中
    st.insert(5);
    st.insert(2);
    st.insert(3);
    //注意，不支持it < st.end()的写法
    set<int>::iterator it;
    st.erase(st.find(2));
    for(set<int>::iterator it = st.begin();it!=st.end();it++)
    {
        printf("%d ",*it);
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```
3 5 
```
+ `st.erase(value)`，`value` 为所需要删除元素的值。时间复杂度度为 $O(logN)$，其中 `N` 为 `set` 内的元素个数。示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <set>
using namespace std;
//主函数
int main()
{
    set<int> st;
    st.insert(3);//insert(x)将x插入set中
    st.insert(5);
    st.insert(2);
    st.insert(3);
    //注意，不支持it < st.end()的写法
    //set<int>::iterator it;
    //st.erase(st.find(2));
    st.erase(2);
    for(set<int>::iterator it = st.begin();it!=st.end();it++)
    {
        printf("%d ",*it);
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```
3 5 
```
+ 删除一个区间内的所有元素：
+ `st.erase(first,last)` 可以删除一个区间内的所有元素，其中 `first` 为所需要删除区间的起始迭代器，而 `last` 则为所需要删除区间的末尾迭代器的下一个地址，也即为删除 `[first, last)`。时间复杂度为 $O(last-first)$。示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <set>
using namespace std;
//主函数
int main()
{
    set<int> st;
    st.insert(3);//insert(x)将x插入set中
    st.insert(5);
    st.insert(2);
    st.insert(3);
    st.insert(1);
    st.insert(4);
    //注意，不支持it < st.end()的写法
    set<int>::iterator it;
    st.erase(st.find(3),st.end());//删除元素3至set末尾之间的元素
    for(set<int>::iterator it = st.begin();it!=st.end();it++)
    {
        printf("%d ",*it);
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```
1 2 
```
##### size()
+ `size()` 用来获取 set 内的元素个数，时间复杂度为 $O(1)$。示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <set>
using namespace std;
//主函数
int main()
{
    set<int> st;
    st.insert(3);//insert(x)将x插入set中
    st.insert(5);
    st.insert(2);
    st.insert(3);
    st.insert(1);
    st.insert(4);
    printf("%d",st.size());
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```
5
```

##### clear()
+ `clear()` 用来清空 set 内的所有元素，时间复杂度为 $O(N)$，其中 `N` 为 `set` 内的元素个数。示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <set>
using namespace std;
//主函数
int main()
{
    set<int> st;
    st.insert(3);//insert(x)将x插入set中
    st.insert(5);
    st.insert(2);
    st.insert(3);
    st.insert(1);
    st.insert(4);
    st.clear();
    printf("%d",st.size());
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```
0
```

#### set 的常见用途
1. `set` 最主要的作用是**自动去重并按照升序排序**，因此碰到需要去重但是却不方便直接开数组的情况，可以尝试用 `set` 解决。
2. `set` 中元素是唯一的，如果需要处理**不唯一**的情况，则需要使用 `multiset`。
3. C++11 标准中还增加了 `unordered_set`,以散列代替 `set` 内部的红黑树（一种自平衡二叉查找树）实现，使其可以用来处理**只去重但不排序**的需求，速度比 `set` 快很多！

### string 的常见用法详解
+ 在 C 语言中，一般使用字符数组 `char str[]` 来存放字符串，但是使用字符数组有时会显得操作麻烦，而且容易因为经验不足产生错误。
+ 如果要使用 `string`，需要添加 `string` 头文件，即 `#include <string>`,除此之外，还需要加上 `using namespace std;`。

#### string 的定义
+ 定义 `string` 的方式跟基本数据类型相同，只需要在 `string` 后跟上变量名即可, 也可以直接给 `string` 类型变量赋值，示例如下：
```cpp
string str;
string str = "yugin chui!";
```

#### string 中内容的访问
1. 通过下标访问
- 一般而言，可以直接像字符数组那样去访问 `string`：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string>
using namespace std;
//主函数
int main()
{
    string str = "yugin chui!";
    for(int i=0;i<str.length();i++)
    {
        printf("%c",str[i]);//输出"yugin chui!"
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
yugin chui!
```
+ 如果要读入和输出整个字符串，则只能使用 `cin` 和 `cout`：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string>
#include <iostream>
using namespace std;
//主函数
int main()
{
    string str;
    cin >> str;
    cout << str;
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 上述代码对于任意字符串输入，都会有输出同样的字符串。
+ 同样，采用 `c_str()` 将 `string` 类型转换为字符数组进行输出，示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string>
#include <iostream>
using namespace std;
//主函数
int main()
{
    string str = "yugin chui!";
    printf("%s\n",str.c_str());//采用c_str()将string类型转换为字符数组进行输出
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
yugin chui!
```
2. 通过迭代器访问
+ 由于有些函数如 `insert()` 和 `erase()` 要求以迭代器为参数，因此需要学习。
+ 由于 `string` 不像其他 STL 容器那样需要参数，因此可以直接定义：
```cpp
string::iterator it;
```
+ 这样就得到了迭代器 `it`，并且可以通过 `*it` 来访问 `string` 中的每一位：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string>
#include <iostream>
using namespace std;
//主函数
int main()
{
    string str = "yugin chui!";
    for(string::iterator it = str.begin();it != str.end();it++)
    {
        printf("%c",*it);
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
yugin chui!
```
+ 最后指出，`string` 和 `vector` 一样，支持直接对迭代器进行加减某个数字，如 `str.begin()+3` 的写法是可行的。
#### string 常用函数实例解析
+ 因为 `string` 的函数有很多，但是有许多函数并不常用，因此只介绍几个常用函数。
##### getline(cin, str);
+ 使用这个函数能够读入一整行字符串，而不会在**空格**处中断！示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string>
#include <iostream>
using namespace std;
//主函数
int main()
{
    string str;
    getline(cin, str);//读入一整行字符串
    cout << str << endl;
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输入：
```text
good bad
```
+ 输出：
```text
good bad
```
##### operator+=
+ 这是 `string` 的加法，可以将两个 `string` 直接拼起来。，示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string>
#include <iostream>
using namespace std;
//主函数
int main()
{
    string str1 = "yugin chui!";
    string str2 = "Dear ";
    string str;
    str = str2 + str1;//将str2加str1,赋值给str
    cout << str << endl;
    str2 += str1;//直接将str1拼接到str2上
    cout << str2 << endl;
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
Dear yugin chui!
Dear yugin chui!
```
##### compare operator
+ 两个 `string` 类型可以直接使用 `==`、`!=`、`<=`、`>=`、`<`、`>`，比较规则是**字典序**，示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string>
#include <iostream>
using namespace std;
//主函数
int main()
{
    string str1 = "aa";
    string str2 = "aaa";
    string str3 = "abc";
    string str4 = "xyj";
    if(str1 < str2) 
        printf("OK1\n");
    if(str1 != str3) 
        printf("OK2\n");
    if(str4 >= str3) 
        printf("OK3\n");
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
OK1
OK2
OK3
```
##### length()/size()
+ `length()` 返回 `string` 的长度，即存放的字符数，时间复杂度为 $O(1)$。`size()` 和 `length()` 基本相同。
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string>
#include <iostream>
using namespace std;
//主函数
int main()
{
    string str1 = "aa";
    printf("%d %d\n",str1.length(),str1.size());
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
2 2
```
##### insert()
+ `string` 的 `insert()` 函数有许多种写法，时间复杂度 $O(N)$：
1. `insert(pos, string)`，在 `pos` 号位置插入字符串 `string`，示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string>
#include <iostream>
using namespace std;
//主函数
int main()
{
    string str1 = "aa";
    string str2 = "bb";
    str1.insert(1,str2);
    cout << str1 << endl;
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
abba
```
2. `insert(it,it2,it3)`, `it` 是原字符串的预插入位置，`it2` 和 `it3` 是待插字符串的首尾迭代器，用来表示字符串 `[it2, it3)` 将被插在 `it` 的位置上，示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string>
#include <iostream>
using namespace std;
//主函数
int main()
{
    string str1 = "aa";
    string str2 = "bb";
    str1.insert(str1.begin()+1,str2.begin(),str2.end());
    cout << str1 << endl;
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
abba
```
##### erase()
+ `erase()` 有两种用法：删除单个元素、删除一个区间内的所有元素。时间复杂度均为 $O(N)$。
1. 删除单个元素
+ `str.erase(it)` 用于删除单个元素，`it` 为需要删除元素的迭代器。示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string>
#include <iostream>
using namespace std;
//主函数
int main()
{
    string str1 = "aa";
    string str2 = "bb";
    str1.insert(str1.begin()+1,str2.begin(),str2.end());
    str1.erase(str1.begin()+1);
    cout << str1 << endl;
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
aba
```
2. 删除一个区间内的所有元素。
+ 删除一个区间内的所有元素有**两种**方法：
+ `str.erase(first,last)`，其中 `first` 为需要删除的区间的起始迭代器，而 last 则为需要删除的区间的末尾迭代器的下一个地址，也即为删除 `[first, last)`。示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string>
#include <iostream>
using namespace std;
//主函数
int main()
{
    string str1 = "aa";
    string str2 = "bbc";
    str1.insert(str1.begin()+1,str2.begin(),str2.end());
    str1.erase(str1.begin()+1,str1.begin()+3);
    cout << str1 << endl;
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
aca
```
+ `str.erase(pos,length)`，其中 `pos` 为需要开始删除的起始位置，`length` 为删除的字符个数，示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string>
#include <iostream>
using namespace std;
//主函数
int main()
{
    string str1 = "aa";
    string str2 = "bbc";
    str1.insert(str1.begin()+1,str2.begin(),str2.end());
    str1.erase(1,2);//删除从1号位开始的2个字符，即bb
    cout << str1 << endl;
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
aca
```
##### clear()
+ `clear()` 用以清空 `string` 中的数据，时间复杂度一般为 $O(1)$。示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string>
#include <iostream>
using namespace std;
//主函数
int main()
{
    string str1 = "aa";
    string str2 = "bbc";
    str1.insert(str1.begin()+1,str2.begin(),str2.end());
    str1.erase(1,2);//删除从1号位开始的2个字符，即bb
    str1.clear();
    cout << str1.size() << endl;
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
0
```
##### substr()
+ `substr(pos, len)` 返回从 `pos` 号位开始、长度为 `len` 的子串，时间复杂度为 $O(len)$。示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string>
#include <iostream>
using namespace std;
//主函数
int main()
{
    string str1 = "Dear yugin chui!";
    cout << str1.substr(5,5) << endl;
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
yugin
```
##### string::npos
+ `string::npos` 是一个常数，其本身的值为 `-1`，但由于是 `unsigned_int` 类型，因此实际上也可以认为是 `unsigned_int` 类型的最大值。`string::npos` 用以作为 `find` 函数失配时的返回值。例如在下面的实例中可以认为 `string::npos` 等于 `-1` 或者 `4294967295`。示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string>
#include <iostream>
using namespace std;
//主函数
int main()
{
    if(string::npos==-1)
        printf("-1 is true\n"); 
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
-1 is true
```
##### find()
+ `str.find(str2)`，当 `str2` 是 `str` 的子串时，返回其在 `str` 中第一次出现的位置，如果 `str2` 不是 `str` 的子串时，那么返回 `string::npos`；
+ `str.find(str2,pos)`，从 `str` 的 `pos` 号位开始匹配 `str2`，返回值与上面相同。
+ 时间复杂度为 $O(nm)$，其中 `n` 和 `m` 分别为 `str` 和 `str2` 的长度。
+ 示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string>
#include <iostream>
using namespace std;
//主函数
int main()
{
    string str = "Dear yugin chui!";
    string str1 = "yugin";//能够找到
    string str2 = "yugin!";//找不到
    if(str.find(str1)!=string::npos)
        cout << str.find(str1) << endl;
    if(str.find(str2)==string::npos)
        printf("%d\n",str.find(str2));
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
5
-1
```
##### replace()
+ `str.replace(pos,len,str2)` 把 `str` 从 `pos` 号位开始、长度为 `len` 的子串替换为 `str2`；
+ `str.replace(it1,it2,str2)` 把 `str` 的迭代器 `[it1, it2)` 范围的子串替换为 `str2`。
+ 时间复杂度为 $O(str.length())$，示例如下:
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string>
#include <iostream>
using namespace std;
//主函数
int main()
{
    string str = "Maybe you will turn around.";
    string str2 = "will not";
    string str3 = "Surely";
    cout << str.replace(10,4,str2) << endl;
    cout << str.replace(str.begin(),str.begin()+5,str3) << endl;
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
Maybe you will not turn around.
Surely you will not turn around.
```
#### string 的例题
例题：[A1060 Are They Equal](https://pintia.cn/problem-sets/994805342720868352/exam/problems/994805413520719872?type=7&page=0)
+ 代码：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string>
#include <iostream>
using namespace std;
int n;

//处理函数
string my_deal(string s,int& e)
{
    int k=0;//s的下标
    while(s.length()>0&&s[0]=='0')
    {
        s.erase(s.begin());//去掉s的前导0
    }
    if(s[0]=='.')//第一位是小数点说明s小于1
    {
        s.erase(s.begin());//去掉s的小数点
        while(s.length()>0&&s[0]=='0')
        {
            s.erase(s.begin());//去掉s的小数点后面的0
            e--;
        }
    }
    else//第一位不是小数点说明s大于1
    {
        while(k<s.length()&&s[k]!='.')//寻找小数点并且得到e
        {
            k++;
            e++;
        }
        if(k<s.length())//说明碰到了小数点
            s.erase(s.begin()+k);//去掉小数点
    }
    if(s.length()==0)//说明去除前导0后等于0
            e=0;
    int i=0;
    k=0;
    string res;
    while(i<n)//循环达到设定的精度
    {
        if(k<s.length())//只要还有数字，就往后面加
        {
            res+=s[k];
            k++;
        }
        else
            res+='0';//没有数字而且没到达精度就补0
        i++; 
    }
    return res;
}

//主函数
int main()
{
    string s1,s2,s3,s4;
    cin >> n >> s1 >> s2;
    int e1 = 0;
    int e2 = 0;
    s3 = my_deal(s1,e1);
    s4 = my_deal(s2,e2);
    if(s3==s4&&e1==e2)
    {
        cout<<"YES 0."<<s3<<"*10^"<<e1<<endl;
    }
    else
    {
        cout<<"NO 0."<<s3<<"*10^"<<e1<<" 0."<<s4<<"*10^"<<e2<<endl;
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 总结：`string` 类型能够方便处理上述题目，**思路**详见代码部分和《算法笔记》P 210-P 212。

### map 的常见用法详解
+ `map` 翻译为映射，也是常用的 STL 容器。
+ `map` 可以将任何基本类型（包括 STL 容器）映射到任何基本类型（包括 STL 容器）。
+ 要使用 `map`，需要添加 `map` 头文件 `#include <map>`,除此之外，还需要加上 `using namespace std;`。
#### map 的定义
+ 单独定义一个 `map`：
```cpp
map<typename1,typename2> mp;
```
+ `map` 和其他 STL 容器在定义上有点不一样，因为 `map` 需要确定映射前类型（键 key）和映射后类型（值 value），所有需要在 `<>` 内填写两个变量。
+ 其中第一个是键（key）的类型，第二个是值（value）的类型。
+ 如果是 `int` 型映射到 `int` 型，就相当于普通 `int` 型数组。
+ 如果是字符串（`string`）到整型（`int`）的映射，必须使用 `string` 而不能用 `char` 数组：
```cpp
map<string,int> mp;
```
+ 因为 `char` 数组作为数组，是不能被作为键值的。
+ 同样，`map` 的键和值也可以是 STL 容器，例如可以将一个 `set` 容器映射到一个字符串：
```cpp
map<set<int>,string> mp;
```
#### map 容器内元素访问
+ `map` 一般有两种访问方式：通过**下标**访问或通过**迭代器**访问。
1. 通过下标访问
+ 和访问普通的数组是一样的，例如对一个定义为 `map<char, int> mp` 的 `map` 而言，就可以直接使用 `mp['c']` 的方式来访问它对应的整数。
+ 于是，当建立映射时，就可以直接使用 `mp['c']=20;` 这样和普通数组一样的方式。
+ 但要注意 map 中的键（key）是唯一的：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <map>
#include <iostream>
using namespace std;
//主函数
int main()
{
    map<char,int> mp;
    mp['c']=20;
    mp['c']=30;
    printf("%d\n",mp['c']);
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
30
```
2. 通过迭代器访问
+ `map` 迭代器的定义和其他 STL 容器迭代器定义的方式相同：
```cpp
map<typename1,typename2>::iterator it;
```
+ `typename1` 和  `typename2` 就是定义 `map` 时填写的类型，这样就得到了迭代器 `it`。
+ 注意 `map` 迭代器的使用方式和其他 STL 容器的迭代器不同，因为 `map` 的每一对映射都有两个 `typename`，这决定了必须能通过一个 `it` 来同时访问键和值。
+ 事实上，`map` 可以使用 `it->first` 来访问键，使用 `it->second` 来访问值。
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <map>
#include <iostream>
using namespace std;
//主函数
int main()
{
    map<char,int> mp;
    mp['a']=20;
    mp['c']=30;
    mp['b']=40;
    for(map<char,int>::iterator it=mp.begin();it!=mp.end();it++)
    {
        printf("%c %d\n",it->first,it->second);
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
a 20
b 40
c 30
```
+ 现象：`map` 会以**键**从小到大的顺序自动排序，即 `a->b->c`。
+ 原理：由于 `map` 内部是使用**红黑树**实现的（`set` 也是），在建立映射的过程中会自动实现**从小到大**的排序功能。
#### map 常用函数实例解析
##### find()
+ `find(key)` 返回键为 `key` 的映射的迭代器，时间复杂度为 $O(logN)$，N 为 `map` 中映射的个数。示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <map>
#include <iostream>
using namespace std;
//主函数
int main()
{
    map<char,int> mp;
    mp['a']=20;
    mp['c']=30;
    mp['b']=40;
    map<char,int>::iterator it=mp.find('c');
    printf("%c %d\n",it->first,it->second);
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
c 30
```
+ 注意如果 `mp.find(key)` 如果找不到元素会返回 `mp.end()`，示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <map>
#include <iostream>
using namespace std;
//主函数
int main()
{
    map<char,int> mp;
    int n,num;
    scanf("%d",&n);
    char c;
    for(int i=0;i<n;i++)
    {
        getchar();
        scanf("%c %d",&c,&num);
        mp[c]=num;
    }
    getchar();
    scanf("%c",&c);
    map<char,int>::iterator it=mp.find(c);
    if(it!=mp.end())//如果找不到返回mp.end()
        printf("%d",it->second);
    else
        printf("-1");
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
##### erase()
+ `erase()` 有两种用法：删除单个元素、删除一个区间内的所有元素。
1. 删除单个元素
+ 删除单个元素有两种方法：
+ 第一种： `mp.erase(it)`，`it` 为需要删除的元素的迭代器。时间复杂度为 $O(1)$，示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <map>
#include <iostream>
using namespace std;
//主函数
int main()
{
    map<char,int> mp;
    mp['a']=20;
    mp['c']=30;
    mp['b']=40;
    map<char,int>::iterator it=mp.find('c');
    mp.erase(it);
    for(map<char,int>::iterator it=mp.begin();it!=mp.end();it++)
    {
        printf("%c %d\n",it->first,it->second);
    }
    // printf("%c %d\n",it->first,it->second);
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
a 20
b 40
```
+ 第二种：`mp.erase(key)`，`key` 为删除的映射键。时间复杂度为 $O(logN)$，N 为 `map` 中映射的个数。示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <map>
#include <iostream>
using namespace std;
//主函数
int main()
{
    map<char,int> mp;
    mp['a']=20;
    mp['c']=30;
    mp['b']=40;
    map<char,int>::iterator it=mp.find('c');
    mp.erase('c');
    for(map<char,int>::iterator it=mp.begin();it!=mp.end();it++)
    {
        printf("%c %d\n",it->first,it->second);
    }
    // printf("%c %d\n",it->first,it->second);
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
a 20
b 40
```
2. 删除一个区间内的所有元素
+ `mp.erase(first,last)`，其中 `first` 为需要删除的区间的起始迭代器，而 `last` 则为需要删除的区间的末尾迭代器的下一个地址，也即为删除左闭右开的区间 `[first, last)`。时间复杂度为 $O(last-first)$，示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <map>
#include <iostream>
using namespace std;
//主函数
int main()
{
    map<char,int> mp;
    mp['a']=20;
    mp['c']=30;
    mp['b']=40;
    map<char,int>::iterator it=mp.find('b');
    mp.erase(it,mp.end());
    for(map<char,int>::iterator it=mp.begin();it!=mp.end();it++)
    {
        printf("%c %d\n",it->first,it->second);
    }
    // printf("%c %d\n",it->first,it->second);
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
a 20
```
##### size()
+ `size()` 用来获得 `map` 中映射的对数，时间复杂度为 $O(1)$。示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <map>
#include <iostream>
using namespace std;
//主函数
int main()
{
    map<char,int> mp;
    mp['a']=20;
    mp['c']=30;
    mp['b']=40;
    printf("%d",mp.size());
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
3
```
##### clear()
+ `clear()` 用来清空 map 中的所有元素，复杂度为 $O(N)$，其中 N 为 `map` 中的元素个数，示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <map>
#include <iostream>
using namespace std;
//主函数
int main()
{
    map<char,int> mp;
    mp['a']=20;
    mp['c']=30;
    mp['b']=40;
    mp.clear();
    printf("%d",mp.size());
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
0
```

### queue 的常见用法详解
+ `queue` 翻译为队列，在 STL 中则是实现了一个**先进先出**的容器。
#### queue 的定义
+ 要使用 `queue`，应先添加头文件 `#include <queue>`，并在头文件下面添加 `using namespace std;`
+ `queue` 的定义写法和其他 STL 容器相同，`typename` 可以是任意基本数据类型或者容器：
```cpp
queue<typename> name;
```
#### queue 容器内元素的访问
+ 由于队列 `queue` 本身就是**先进先出**的限制性数据结构，因此在 STL 中只能通过 `front()` 来访问队首元素，或者是通过 `back()` 来访问队尾元素。示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <queue>
using namespace std;

//主函数
int main()
{
    queue<int> q;
    for(int i=1;i<=5;i++)
    {
        q.push(i);//q.push(i);用以将i压入队列，因此依次入队1 2 3 4 5
    }
    printf("%d %d\n",q.front(),q.back());//输出结果1 5
    system("pause");    // 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
1 5
```
#### queue 常用函数实例解析
##### push()
+ `push(x)` 将 `x` 进行入队，时间复杂度为 $O(1)$。
##### front()、back()
+ `front()` 和 `back()` 可以分别获得队首元素和队尾元素,时间复杂度为 $O(1)$。
##### pop()
+ `pop()` 令队首元素出队，时间复杂度为 $O(1)$。示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <queue>
using namespace std;

//主函数
int main()
{
    queue<int> q;
    for(int i=1;i<=5;i++)
    {
        q.push(i);//q.push(i);用以将i压入队列，因此依次入队1 2 3 4 5
    }
    for(int i=1;i<=3;i++)
    {
        q.pop();//出队首元素1 2 3
    }
    printf("%d %d\n",q.front(),q.back());
    system("pause");    // 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
4 5
```
##### empty()
+ `empty()` 检测 `queue` 是否为空，返回 `true` 则空，返回 `false` 则非空。时间复杂度为 $O(1)$。示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <queue>
using namespace std;

//主函数
int main()
{
    queue<int> q;
    if(q.empty() == true)//一开始队列中没有元素，所以为空
    {
        printf("EMPTY!\n");
    }
    else
    {
        printf("NOT EMPTY!\n");
    }
    for(int i=1;i<=5;i++)
    {
        q.push(i);//q.push(i);用以将i压入队列，因此依次入队1 2 3 4 5
    }
    if(q.empty() == true)
    {
        printf("EMPTY!\n");
    }
    else
    {
        printf("NOT EMPTY!\n");
    }
    system("pause");    // 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
EMPTY!
NOT EMPTY!
```
##### size()
+ `size()` 返回 queue 内元素的个数，时间复杂度为 $O(1)$。实例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <queue>
using namespace std;

//主函数
int main()
{
    queue<int> q;
    for(int i=1;i<=5;i++)
    {
        q.push(i);//q.push(i);用以将i压入队列，因此依次入队1 2 3 4 5
    }
    printf("%d\n",q.size());
    system("pause");    // 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
5
```
#### queue 的常见用途
+ 当需要实现广度优先搜索时，可以不用自己手动实现一个队列，而是用 `queue` 作为代替，以提高程序的准确性。
+ 需要注意的是，使用 `front()` 和 `pop()` 函数前，必须用 `empty()` 判断队列是否为空，否则可能因为队空而出现错误。
+ 延伸：STL 容器中还有两种容器与队列相关，分别是双端队列 (`deque`)和优先队列 (`priority_queue`)。
+ 双端队列 (`deque`)：首尾皆可插入和删除的队列；
+ 优先队列 (`priority_queue`)：使用堆实现的默认将当前队列最大元素置于队首的容器。

### priority_queue 的常见用法详解
+ `priority_queue` 又称为优先队列，其底层是用**堆**来进行实现的。
+ 在优先队列中，队首元素一定是当前队列中**优先级最高**的那一个。
+ 例如在队列中有如下元素，且定义好了优先级：
```text
桃子（优先级3）
梨子（优先级4）
苹果（优先级1）
```
+ 那么出队的顺序为梨子 (4) -> 桃子 (3) -> 苹果 (1)。
+ 当然，可以在任何时候让优先队列里面加入 (`push`)元素，而优先队列底层的数据结构**堆** (`heap`)会随时调整结构，使得**每次的队首元素都是优先级最大**。
+ 关于上述的优先级是**规定**出来的。

#### priority_queue 的定义
+ 要使用 `priority_queue`，应先添加头文件 `#include <queue>`，并在头文件下面添加 `using namespace std;`
+ 其定义的写法和其他 STL 容器相同，`typename` 可以是任意基本数据类型或容器：
```cpp
priority_queue<typename> name;
```
#### priority_queue 容器内元素访问
+ 和普通队列不一样的是，优先队列没有 `front()` 函数与 `back()` 函数，而只能通过 `top()` 函数来访问**队首**元素（也可以称为**堆顶**元素），也就是优先级最高的元素。示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <queue>
using namespace std;

//主函数
int main()
{
    priority_queue<int> q;
    q.push(1);
    q.push(2);
    q.push(4);
    q.push(3);
    printf("%d",q.top());
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
4
```
#### priority_queue 常用函数实例解析
##### push()
+ `push(x)` 将令 `x` 入队，时间复杂度为 $O(logN)$，其中 N 为当前优先队列中的元素个数。
##### top()
+ `top()` 可以获得队首元素（即堆顶元素），时间复杂度 $O(1)$。
##### pop()
+ `pop()` 令队首元素（即堆顶元素）出队，时间复杂度为 $O(logN)$，其中 N 为当前优先队列中的元素个数，示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <queue>
using namespace std;

//主函数
int main()
{
    priority_queue<int> q;
    q.push(1);
    q.push(2);
    q.push(4);
    q.push(3);
    printf("%d\n",q.top());
    q.pop();
    printf("%d\n",q.top());
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
4
3
```
##### empty()
+ `empty()` 检测优先队列是否为空，返回 `true` 则为空，返回 `false` 则为非空。时间复杂度为 $O(1)$，示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <queue>
using namespace std;

//主函数
int main()
{
    priority_queue<int> q;
    if(q.empty()==true)
    {
        printf("EMPTY!\n");
    }
    else
    {
        printf("NOT EMPTY!\n");
    }
    q.push(1);
    q.push(2);
    q.push(4);
    q.push(3);
    if(q.empty()==true)
    {
        printf("EMPTY!\n");
    }
    else
    {
        printf("NOT EMPTY!\n");
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
EMPTY!
NOT EMPTY!
```
##### size()
+ `size()` 返回优先队列内元素的个数，时间复杂度为 $O(1)$，示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <queue>
using namespace std;

//主函数
int main()
{
    priority_queue<int> q;
    q.push(1);
    q.push(2);
    q.push(4);
    q.push(3);
    printf("%d\n",q.size());
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
4
```
#### priority_queue 内元素优先级的设置
+ 如何定义优先队列内元素的优先级是运用好优先队列的关键，下面分别介绍基本数据类型（如 `int`、`double`、`char`）与结构体类型的优先级设置方法。
##### 基本数据类型的优先级设置
+ 此处的基本类型指的是 `int`、`double`、`char` 等可以直接使用的数据类型，优先队列对他们的优先级设置一般是数字大的优先级越高，因此队首元素就是优先队列内元素最大那个（如果是 `char` 型，则是字典序最大的）。对基本数据类型来说，下面两种优先队列的定义是**等价**的，以 `int` 为例：
```cpp
priority_queue<int> q;
priority_queue<int , vector<int>,less<int> > q;
```
+ 不难发现，第二种定义方式 `<>` 内多出了两个参数：
1. 一个是 `vector<int>`，该参数填写的是来承载底层数据结构堆 (`heap`)的容器，如果第一个参数是 `double` 型或 `char` 型，则此处只需要填写 `vector<double>` 或者 `vector<char>`；
2. 另一个是 `less<int>`，该参数是对第一个参数的比较类，`less<int>` 表示**数字越大优先级越大**，而 `greater<int>` 表示**数字越小优先级越大**。
+ 因此，如果想让优先队列总是把最小的元素放在队首，只需进行如下定义：
```cpp
priority_queue<int , vector<int>,greater<int> > q;
```
+ 示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <queue>
using namespace std;

//主函数
int main()
{
    priority_queue<int , vector<int>,greater<int> > q;//数字越小优先级越大
    q.push(1);
    q.push(2);
    q.push(4);
    q.push(3);
    printf("%d\n",q.top());
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
1
```
##### 结构体优先级设置
+ 本节的开头举了一个水果的例子，可以对水果的名称和价格建立一个结构体，示例如下：
```cpp
struct fruit{
	string name;
	int price;
};
```
+ 现在希望按水果的价格高的为优先级高，就需要重载（overload）小于号"<"。重载是指对已有的运算符进行重新定义。
+ 也就是说，可以改变小于号的功能（例如把它重载为大于号的功能）。目前暂时只需要知道其写法即可：
```cpp
struct fruit{
	string name;
	int price;
	friend bool operator < (fruit f1,fruit f2)
	{
		return f1.price < f2.price;
	}
};
```
+ 可以看到，`fruit` 结构体中增加了一个函数，其中 `friend` 是友元（自行查找资料了解）。
+ 后面的 `bool operator < (fruit f1, fruit f2)` 对 fruit 类型的操作符"<"进行了重载。
+ 重载大于号会编译错误，因为从数学上来说只需要重载小于号，即 `f1>f2` 等价于判断 `f2<f1`，而 `f1==f2` 则等价于判断 `!(f1<f2)&&!(f2<f1)`，函数内部为 `return f1.price < f2.price;`，因此重载后小于号还是小于号的作用。
+ 此时就可以直接定义 `fruit` 类型的优先队列，其内部就是以价格高的水果为优先级高，示例如下：
```cpp
priority_queue<fruit> q;
```
+ 同理，如果想要以价格低的水果优先级高，那么只需要把 `return` 中的小于号改为大于号即可，示例如下：
```cpp
struct fruit{
	string name;
	int price;
	friend bool operator < (fruit f1,fruit f2)
	{
		return f1.price > f2.price;
	}
};
```
+ 整体代码示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <queue>
#include <string>
#include <iostream>
using namespace std;

//结构体
struct fruit{
	string name;
	int price;
	friend bool operator < (fruit f1,fruit f2)
	{
		return f1.price > f2.price;//大于号表示价格低优先级高
	}
}f1,f2,f3;

//主函数
int main()
{
    priority_queue<fruit> q;
    f1.name="桃子";
    f1.price=3;
    f2.name="梨子";
    f2.price=4;
    f3.name="苹果";
    f3.price=1;
    q.push(f1);
    q.push(f2);
    q.push(f3);
    cout << q.top().name << " " << q.top().price << endl;
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
苹果 1
```
+ 不难发现，此处对小于号的重载与排序函数 `sort` 中的 `cmp` 函数有些相似，它们的参数都是两个变量，函数内部都是 `return` 了 `true` 或者 `false`。
+ 事实上，这两者的作用确实是类似的，只不过效果看上去是“相反”的。
+ 在**排序**中，如果是 `return f1.price > f2.price;`，那么则是按照价格**从高到低**排序。
+ 在**优先队列**中，则是把价格低的放到队首。原因在于，优先队列本身默认的规则就是优先级高的放队首，因此把小于号重载为大于号的功能时只是把这个规则反向了一下。
+ 最后，只需要记住**优先队列的这个函数与 sort 中的 cmp 函数的效果相反的即可**。
+ 把优先队列的比较函数放外面：只需要把 `friend` 去掉，把小于号改成一对小括号，然后把重载的函数写在结构体的外面，同时将其用 `struct` 包装起来，示例如下：
```cpp
struct cmp{
	bool operator() (fruit f1,fruit f2)
	{
		return f1.price > f2.price;
	}
};
```
+ 在这种情况下，需要用之前讲解的第二种定义方式来定义优先队列：
```cpp
priority_queue<fruit , vector<fruit> , cmp> q;
```
+ 可以看到，此处只是把 `greater<>` 部分换成了 `cmp`。示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <queue>
#include <string>
#include <iostream>
using namespace std;

//结构体
struct fruit{
	string name;
	int price;
}f1,f2,f3;

struct cmp{
	bool operator() (fruit f1,fruit f2)
	{
		return f1.price > f2.price;
	}
};

//主函数
int main()
{
    priority_queue<fruit , vector<fruit> , cmp> q;
    f1.name="桃子";
    f1.price=3;
    f2.name="梨子";
    f2.price=4;
    f3.name="苹果";
    f3.price=1;
    q.push(f1);
    q.push(f2);
    q.push(f3);
    cout << q.top().name << " " << q.top().price << endl;
    //q.pop();
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
苹果 1
```
+ 与此同时，我们应该联想到，即便是**基本数据类型或者其它 STL 容器**（例如 `set`），也可以通过同样的方式来定义优先级。
+ 最后指出，如果结构体内的数据较为庞大（例如出现了**字符串**或者**数组**），建议使用引用来提高效率，此时比较类的参数中需要加上 `"const"` 和 `"&"`，示例如下：
```cpp
struct fruit{
	string name;
	int price;
	friend bool operator < (const fruit &f1,const fruit &f2)
	{
		return f1.price > f2.price;//大于号表示价格低优先级高
	}
};
```
+ 以及：
```cpp
struct cmp{
	bool operator() (const fruit &f1,const fruit &f2)
	{
		return f1.price > f2.price;
	}
};

```
#### priority_queue 的常见用途
+ `priority_queue` 可以解决一些**贪心问题**，也可以对 **Dijkstra 算法**进行优化（因为优先队列的本质是堆）。
+ 需要注意的是，使用 `top()` 函数前，必须用 `empty()` 判断优先队列是否为空，否则可能因为**队列空**而出现错误！

### stack 的常见用法详解
+ `stack` 翻译为栈，是 STL 中实现的一个后进先出的容器。
#### stack 的定义
+ 要使用 `stack`，应该先添加头文件 `#include <stack>`，并在头文件下面加上 `using namespace std;`
+ 其定义的写法和其他 STL 容器相同，`typename` 可以是任意基本数据类型或容器：
```cpp
stack<typename> name;
```
#### stack 容器内元素的访问
+ 由于栈（`stack`）本身就是一种后进先出的数据结构，在 STL 的 `stack` 只能通过 `top()` 来访问栈顶元素。示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <stack>
#include <string>
#include <iostream>
using namespace std;

//主函数
int main()
{
    stack<int> st;
    for(int i=1;i<=5;i++)
    {
        st.push(i);
    }
    printf("%d\n",st.top());//st.top()取栈顶元素
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
5
```
#### stack 常用函数实例解析
##### push()
+ `push(x)` 将 `x` 入栈，时间复杂度为 $O(1)$。
##### top()
+ `top()` 获得栈顶元素，时间复杂度为 $O(1)$。
##### pop()
+ `pop()` 用以弹出栈顶元素，时间复杂度为 $O(1)$。示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <stack>
#include <string>
#include <iostream>
using namespace std;

//主函数
int main()
{
    stack<int> st;
    for(int i=1;i<=5;i++)
    {
        st.push(i);
    }
    for(int i=1;i<=3;i++)
    {
        st.pop();
    }
    printf("%d\n",st.top());//st.top()取栈顶元素
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
2
```
##### empty()
+ `empty()` 可以检测 `stack` 内是否为空，返回 `true` 则为空，返回 `false` 则为非空。时间复杂度为 $O(1)$，示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <stack>
#include <string>
#include <iostream>
using namespace std;

//主函数
int main()
{
    stack<int> st;
    if(st.empty()==true)
    {
        printf("EMPTY!\n");
    }
    else
    {
        printf("NOT EMPTY!\n");
    }
    for(int i=1;i<=5;i++)
    {
        st.push(i);
    }
    if(st.empty()==true)
    {
        printf("EMPTY!\n");
    }
    else
    {
        printf("NOT EMPTY!\n");
    }
    //printf("%d\n",st.top());//st.top()取栈顶元素
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
EMPTY!
NOT EMPTY!
```
##### size()
+ `size()` 返回 stack 内元素个数，时间复杂度为 $O(1)$，示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <stack>
#include <string>
#include <iostream>
using namespace std;

//主函数
int main()
{
    stack<int> st;
    for(int i=1;i<=5;i++)
    {
        st.push(i);
    }
    printf("%d\n",st.size());//st.top()取栈顶元素
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
5
```
#### stack 的常见用途
+ `stack` 用来模拟实现一些递归，防止程序对**栈内存的限制**而导致程序运行出错。
+ 一般来说，程序的栈内存空间很小，对有些题目来说，如果用普通的函数进行递归，一旦**递归层数过深**（不同机器不同，**约几千至几万层**），则会导致程序运行崩溃。
+ 如果用栈来模拟递归算法的实现，则可以避免这一问题，不过应用较少。

### pair 的常见用法详解
+ `pair` 是一个很实用的容器，当想要将两个元素绑在一起作为一个合成元素，又不想因此定义结构体时，使用 `pair` 可以很方便地作为一个替代品。
+ 也就是说，`pair` 实际上可以看作一个内部有两个元素的结构体，且这两个元素是可以指定的，如下面结构体的短代码所示：
```cpp
struct pair
{
	typename1 first;
	typename2 second;
};
```
#### pair 的定义
+ 要使用 `pair`，应先添加头文件 `#include <utility>`，并在头文件下面加上 `using namespace std;`
+ 注意，由于 `map` 的内部涉及 `pair`，因此添加 `map` 头文件时会自动添加 `utility` 头文件。
+ `pair` 有两个参数，分别对于 `first` 和 `second` 的数据类型，它们可以是任意基本数据类型或容器，示例如下：
```cpp
pair<typename1 , typename2> name;
pair<string , int> p;
```
+ 如果想在定义 `pair` 时进行初始化，只需要跟上一个小括号，里面填写两个想要初始化的元素即可：
```cpp
pair<string , int> p("yugin!",8);
```
+ 而如果想要在代码中临时构建一个 `pair`，有如下两种方法：
1. 将类型定义在前面，后面用小括号内两个元素的方式：
```cpp
pair<string , int>("yugin!",8);
```
2. 使用自带的 `make_pair` 函数：
```cpp
make_pair("yugin!",8);
```
+ 关于这两种用法见下述例子。
#### pair 中元素的访问
+  `pair` 中只有两个元素，分别是 `first` 和 `second`，只需要按照正常结构体的方式去访问即可，示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <stack>
#include <string>
#include <iostream>
#include <utility>
using namespace std;

//主函数
int main()
{
    pair<string , int> p;
    p.first = "yugin!";
    p.second = 5;
    cout << p.first << " " << p.second << endl;
    p = make_pair("chui yugin!",88);
    cout << p.first << " " << p.second << endl;
    p = pair<string , int>("chui yugin yep!",888);
    cout << p.first << " " << p.second << endl;
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
yugin! 5
chui yugin! 88
chui yugin yep! 888
```
#### pair 常用函数实例解析
##### 比较操作数
+ 两个 `pair` 类型数据可以直接使用 `==`、`!=`、`<`、`<=`、`>`、`>=` 比较大小，比较规则是先以 `first` 的大小作为标准，只有当 `first` 相等时才去判断 `second` 的大小。示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <stack>
#include <string>
#include <iostream>
#include <utility>
using namespace std;

//主函数
int main()
{
    pair<int , int> p1(5,10);
    pair<int , int> p2(5,15);
    pair<int , int> p3(10,5);
    if(p1 < p3)
        printf("p1 < p3\n");
    if(p1 <= p3)
        printf("p1 <= p3\n");
    if(p1 < p2)
        printf("p1 < p2\n");
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
p1 < p3
p1 <= p3
p1 < p2
```
#### pair 的常见用途
+ 关于 `pair` 有两个比较常见的例子：
1. 用来代替二元结构体及其构造函数，可以节省编码时间。
2. 作为 `map` 的键值对来进行插入，示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <stack>
#include <string>
#include <iostream>
#include <utility>
#include <map>
using namespace std;

//主函数
int main()
{
    map<string,int> mp;
    mp.insert(make_pair("yugin!",88));
    mp.insert(pair<string,int>("Mr.yugin!",8));
    for(map<string,int>::iterator it = mp.begin();it!=mp.end();it++)
    {
        cout << it->first << " " << it->second << endl;
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}

```
+ 输出：
```text
Mr.yugin! 8
yugin! 88
```

### algorithm 头文件下的常用函数
+ 使用 `#include <algorithm>` 头文件，需要在头文件下加一行`using namespace std;`
#### max()、min()和 abs()
+ `max(x,y)` 和 `min(x,y)` 分别返回 `x` 和 `y` 中的最大值和最小值，且参数必须是两个（可以是浮点数）。
+ 如果想要返回 `x`、`y`、`z` 三个数的最大值, 可以使用 `max(x,max(y,z))` 写法。
+ `abs(x)` 返回 `x` 的绝对值。
+ 注意：`x` 必须是整型，浮点型的绝对值用 `#include <math>` 头文件下的 `fabs` 函数。
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <stack>
#include <string>
#include <iostream>
#include <utility>
#include <map>
#include <algorithm>
using namespace std;

//主函数
int main()
{
    int x = 1,y = -2;
    printf("%d %d\n",max(x,y),min(x,y));
    printf("%d %d\n",abs(x),abs(y));
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
1 -2
1 2
```
#### swap()
+ `swap(x,y)` 用来交换 `x` 和 `y` 的值，示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <stack>
#include <string>
#include <iostream>
#include <utility>
#include <map>
#include <algorithm>
using namespace std;

//主函数
int main()
{
    int x = 1,y = -2;
    swap(x,y);
    printf("%d %d\n",x,y);
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
-2 1
```
#### reverse()
+ `reverse(it,it2)` 可以将数组指针在 `[it, it2)` 之间的元素或容器的迭代器在 `[it, it2)` 范围内的元素进行反转。示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <stack>
#include <string>
#include <iostream>
#include <utility>
#include <map>
#include <algorithm>
using namespace std;

//主函数
int main()
{
    int a[10] = {10,11,12,13,14,15};
    reverse(a,a+4);//将a[0]~a[3]反转
    for(int i=0;i<6;i++)
    {
        printf("%d ",a[i]);
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
13 12 11 10 14 15
```
+ 如果是对容器中的元素（例如 `string` 字符串）进行反转，结果也一样：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <stack>
#include <string>
#include <iostream>
#include <utility>
#include <map>
#include <algorithm>
using namespace std;

//主函数
int main()
{
    string str = "abcdef";
    reverse(str.begin(),str.begin()+4);//将str[0]~str[3]反转
    for(int i=0;i<str.length();i++)
    {
        printf("%c",str[i]);
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
dcbaef
```
#### next_permutation()
+ `next_permutation()` 给出一个序列在全排序的下一个序列。
+ 例如当 `n==3` 时的全排列为：
```text
123
132
213
231
312
321
```
+ 这样 `231` 的下一个序列就是 `312`，示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <stack>
#include <string>
#include <iostream>
#include <utility>
#include <map>
#include <algorithm>
using namespace std;

//主函数
int main()
{
    int a[10] = {1,2,3};
    //a[0]~a[2]之间的序列需要求解next_permutation()
    do
    {
        printf("%d%d%d\n",a[0],a[1],a[2]);
    }while(next_permutation(a,a+3));
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
123
132
213
231
312
321
```
+ 在上述代码中，使用循环是因为 `next_permutation()` 在已经到达全排列的最后一个时会返回 `false`，这样会方便退出循环。
+ 而使用 `do...while` 语句而不使用 `while` 语句是因为序列 `123` 本身也需要输出，如果使用 `while` 语句会直接跳到下一个序列再输出，这样结果会少一个 `123`。
#### fill()
+ `fill()` 可以把数组或容器中的某一段区间赋为某个相同的值。
+ 和 `memset` 不同，这里的赋值可以是**数组类型对应范围中的任意值**。示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <stack>
#include <string>
#include <iostream>
#include <utility>
#include <map>
#include <algorithm>
using namespace std;

//主函数
int main()
{
    int a[10] = {1,2,3,4,5};
    fill(a,a+5,888);//将a[0]~a[4]均赋值为888
    for(int i=0;i<5;i++)
    {
        printf("%d ",a[i]);
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
888 888 888 888 888
```
+ 对于二维数组的使用，示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <stack>
#include <string>
#include <iostream>
#include <utility>
#include <map>
#include <algorithm>
using namespace std;

//主函数
int main()
{
    int n=3,m=5;
    int a[n][m];
    int k;
    scanf("%d",&k);
    fill(a[0], a[0] + n * m, k);
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            printf("%d", a[i][j]);
            if (j < m - 1) {
                printf(" ");
            } else {
                printf("\n");
            }
        }
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
#### sort()
+ 由于排序题中大部分只需要得到排序的最终结果，而不需要去写完整的排序过程，因此推荐采用 `C++` 中的 `sort()` 函数进行处理。

##### 如何使用 sort ()函数排序

+ `sort()` 函数的使用必须加上头文件 `#include <algorithm>` 和 `using namespace std;`，其使用方式如下：

```cpp
sort(首元素地址(必填),尾元素地址的下一个地址(必填),比较函数(非必填));
```

##### 如何实现比较函数 cmp

###### 基本数据类型数组的排序

+ 若比较函数不填，则默认按照从小到大的顺序排序。
+ 例如：

```cpp
#include <cstdio>
#include <string.h>
#include <iostream>
#include <algorithm>
using namespace std;
//主函数
int main(){
    int a[5]={1,2,3,4,5};
    sort(a,a+5);
    for(int i=0;i<5;i++)
    {
        printf("%d ",a[i]);
    }
    return 0;
}
```

+ 输出：

```
1 2 3 4 5 
```

+ 如果想要实现从大到小来排序，则需要编写 cmp (比较函数)：

```cpp
#include <cstdio>
#include <string.h>
#include <iostream>
#include <algorithm>
using namespace std;
bool cmp(int a,int b)
{
    return a>b;//可以理解为当a>b时，把a放在b前面
}
//主函数
int main(){
    int a[5]={1,2,3,4,5};
    sort(a,a+5,cmp);
    for(int i=0;i<5;i++)
    {
        printf("%d ",a[i]);
    }
    return 0;
}
```

+ 输出：

```
5 4 3 2 1 
```

+ **记忆方法**：
  + 数据“从小到大”就用 `“<”`，因为 `a<b` 是**左小右大**
  + 数据“从大到小”就用 `“>”`，因为 `a>b` 是**左大右小**

###### 结构体数组排序

+ 一级排序

```cpp
bool cmp(node a,node b)
{
	return a.x>b.x;
}
```

+ 二级排序

```cpp
bool cmp(node a,node b)
{
	if(a.x!=b.x)
	{
		return a.x>b.x;
	}
	else
	{
		return a.y<b.y;
	}
}
```
###### 容器排序
+ 在 STL 标准容器中，只有 `vector`、`string`、`deque` 是可以使用 `sort` 的。因为像 `set` 和 `map` 这样的容器是采用**红黑树**实现的，元素本身有序，故不允许 `sort` 排序。
+ 下面以 `vector` 为例：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <stack>
#include <string>
#include <iostream>
#include <utility>
#include <map>
#include <algorithm>
#include <vector>
using namespace std;

//比较函数
bool cmp(int a,int b)//vector中的元素为int型
{
    return a>b;//从大到小
}

//主函数
int main()
{
    vector<int> vi;
    vi.push_back(2);
    vi.push_back(3);
    vi.push_back(1);
    sort(vi.begin(),vi.end(),cmp);
    for(int i=0;i<3;i++)
    {
        printf("%d ",vi[i]);
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}

```
+ 输出：
```text
3 2 1 
```
+ 以 `string` 为例：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <stack>
#include <string>
#include <iostream>
#include <utility>
#include <map>
#include <algorithm>
#include <vector>
using namespace std;

//主函数
int main()
{
    string str[3] = {"bbbb","cc","aaa"};
    sort(str,str+3);//将string型数组按照字典序从小到大排序
    for(int i=0;i<3;i++)
    {
        cout<<str[i]<<endl;
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}

```
+ 输出：
```text
aaa
bbbb
cc
```
+ 在上述例子中，如果需要按照字符串长度从小到大排序，可以见如下示例：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <stack>
#include <string>
#include <iostream>
#include <utility>
#include <map>
#include <algorithm>
#include <vector>
using namespace std;

//比较函数
bool cmp(string a,string b)//vector中的元素为int型
{
    return a.length()<b.length();//从小到大
}

//主函数
int main()
{
    string str[3] = {"bbbb","cc","aaa"};
    sort(str,str+3,cmp);//将string型数组按照字典序从小到大排序
    for(int i=0;i<3;i++)
    {
        cout<<str[i]<<endl;
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
cc
aaa
bbbb
```
#### lower_bound()和 upper_bound()
+ `lower_bound()` 和 `upper_bound()` 需要用在一个有序数组或容器中。
+ `lower_bound(first,last,val)` 用来寻找在数组或容器的 `[first, last)` 范围内第一个值**大于等于** `val` 元素的位置，如果是数组，则返回该位置的**指针**；如果是容器，则返回该位置的迭代器。
+ `upper_bound(first,last,val)` 用来寻找在数组或容器的 `[first, last)` 范围内第一个值**大于** `val` 元素的位置，如果是数组，则返回该位置的**指针**；如果是容器，则返回该位置的迭代器。
+ 显然，如果数组或容器中没有需要寻找的元素，则 `lower_bound()` 和 `upper_bound()` 均返回可以插入该元素的位置的指针或迭代器（即假设存在该元素时，该元素应当在的位置）。
+ `lower_bound()` 和 `upper_bound()` 的复杂度均为 $O(log(last-first))$。
+ 示例如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <stack>
#include <string>
#include <iostream>
#include <utility>
#include <map>
#include <algorithm>
#include <vector>
using namespace std;

//主函数
int main()
{
    int a[10] = {1,2,2,3,3,3,5,5,5,5};
    //寻找-1
    int *lowerPos = lower_bound(a,a+10,-1);
    int *upperPos = upper_bound(a,a+10,-1);
    printf("%d %d\n",lowerPos-a,upperPos-a);
    //寻找1
    lowerPos = lower_bound(a,a+10,1);
    upperPos = upper_bound(a,a+10,1);
    printf("%d %d\n",lowerPos-a,upperPos-a);
    //寻找3
    lowerPos = lower_bound(a,a+10,3);
    upperPos = upper_bound(a,a+10,3);
    printf("%d %d\n",lowerPos-a,upperPos-a);
    //寻找4
    lowerPos = lower_bound(a,a+10,4);
    upperPos = upper_bound(a,a+10,4);
    printf("%d %d\n",lowerPos-a,upperPos-a);
    //寻找6
    lowerPos = lower_bound(a,a+10,6);
    upperPos = upper_bound(a,a+10,6);
    printf("%d %d\n",lowerPos-a,upperPos-a);

    
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}

```
+ 输出：
```text
0 0
0 1
3 6
6 6
10 10
```
+ 显然，如果只是想获得欲查元素的下标，就可以不使用临时指针，而**直接令返回值减去数组首地址**即可：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <stack>
#include <string>
#include <iostream>
#include <utility>
#include <map>
#include <algorithm>
#include <vector>
using namespace std;

//主函数
int main()
{
    int a[10] = {1,2,2,3,3,3,5,5,5,5};
    //寻找3
    printf("%d %d\n",lower_bound(a,a+10,3)-a,upper_bound(a,a+10,3)-a);
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}

```
+ 输出：
```text
3 6
```
## 算法初步

### 排序

+ 本章先介绍**两种**基础的排序算法：**选择排序**与**插入排序**。
#### 选择排序
+ **简单选择排序**：对于一个序列`A`中的元素`A[1]-A[n]`，令`i`从`1`到`n`枚举，进行`n`趟操作，每趟从待排序部分`[i,n]`中选择最小元素，令其与待排序部分的第一个元素`A[i]`进行交换，这样元素`A[i]`就会与当前有序区间`[1,i-1]`形成新的有序区间`[1,i]`。

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202311011059055.png)

+ 总共需要进行n趟操作(1<=i<=n)，每趟操作选出待排序部分`[i,n]`中的最小元素，令其与`A[i]`交换。总复杂度为$O(n^2)$

+ 实现代码：

```cpp
//选择排序函数
void select_sort(int list[],int num)
{
    int min_num,k,temp;
    for(int i=0;i<num;i++)
    {
        min_num=list[i];
        k=i;
        for(int j=i;j<num;j++)
        {
            if(list[j]<min_num)
            {
                min_num = list[j];
                k=j;
            }
        }
        temp=list[i];
        list[i]=min_num;
        list[k]=temp;
    }
}
```
#### 插入排序
+ **直接插入排序**：对于一个序列`A`中的元素`A[1]-A[n]`，令`i`从`1`到`n-1`枚举，进行`n-1`趟操作。假设某一趟时，序列`A`的前`i-1`个元素`A[1]-A[i-1]`已经有序，而范围`[i,n]`还未有序，那么该趟从范围`[1,i-1]`中寻找某个位置`j`，使得将`A[i]`插入位置`j`后(此时`A[j]-A[i-1]`会后移一位至`A[j+1]-A[i]`)，范围`[1,i]`有序。
+ 思想如下图所示：

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202311011319809.png)

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202311011319528.png)
+ 实现代码：
```cpp
//插入排序函数
void insert_sort(int list[],int num)
{
    int temp,j;
    for(int i=1;i<num;i++)//进行n-1趟
    {
        temp=list[i];
        j=i;
        while(j>0&&temp<list[j-1])//只要temp小于前一个元素list[j-1]
        {
            list[j]=list[j-1];//把list[j-1]移到list[j]
            j--;
        }
        list[j]=temp;//插入位置为j
    }
}
```

#### 排序题与sort函数的应用

+ 由于排序题中大部分只需要得到排序的最终结果，而不需要去写完整的排序过程，因此推荐采用`C++`中的`sort()`函数进行处理。

##### 如何使用sort()函数排序

+ `sort()`函数的使用必须加上头文件`#include <algorithm>`和`using namespace std;`，其使用方式如下：

```cpp
sort(首元素地址(必填),尾元素地址的下一个地址(必填),比较函数(非必填));
```

##### 如何实现比较函数cmp

###### 基本数据类型数组的排序

+ 若比较函数不填，则默认按照从小到大的顺序排序。
+ 例如：

```cpp
#include <cstdio>
#include <string.h>
#include <iostream>
#include <algorithm>
using namespace std;
//主函数
int main(){
    int a[5]={1,2,3,4,5};
    sort(a,a+5);
    for(int i=0;i<5;i++)
    {
        printf("%d ",a[i]);
    }
    return 0;
}
```

+ 输出：

```
1 2 3 4 5 
```

+ 如果想要实现从大到小来排序，则需要编写cmp(比较函数)：

```cpp
#include <cstdio>
#include <string.h>
#include <iostream>
#include <algorithm>
using namespace std;
bool cmp(int a,int b)
{
    return a>b;//可以理解为当a>b时，把a放在b前面
}
//主函数
int main(){
    int a[5]={1,2,3,4,5};
    sort(a,a+5,cmp);
    for(int i=0;i<5;i++)
    {
        printf("%d ",a[i]);
    }
    return 0;
}
```

+ 输出：

```
5 4 3 2 1 
```

+ **记忆方法**：
  + 数据“从小到大”就用`“<”`，因为`a<b`是**左小右大**
  + 数据“从大到小”就用`“>”`，因为`a>b`是**左大右小**

###### 结构体数组排序

+ 一级排序

```cpp
bool cmp(node a,node b)
{
	return a.x>b.x;
}
```

+ 二级排序

```cpp
bool cmp(node a,node b)
{
	if(a.x!=b.x)
	{
		return a.x>b.x;
	}
	else
	{
		return a.y<b.y;
	}
}
```

例题：[考场排名](https://sunnywhy.com/sfbj/4/1/97)

+ 代码：

```cpp
#include <cstdio>
#include <string.h>
#include <iostream>
#include <algorithm>
using namespace std;
const int MAXN = 1100;
//结构体
struct stu{
    char name[20];
    int score;
    int kaochang;
    int paiming;
};

//比较函数
bool cmp1(stu a,stu b)
{
    return a.score>b.score;
}

bool cmp2(stu a,stu b)
{
        return strcmp(a.name,b.name)<0;

}

//主函数
int main(){
    int n,num_kaochang,sum=0,num[15];
    scanf("%d",&n);
    stu stu[MAXN];
    for(int i=0;i<n;i++)
    {
        scanf("%d",&num_kaochang);
        for(int k=sum;k<num_kaochang+sum;k++)
        {
            scanf("%s",stu[k].name);
            scanf("%d",&stu[k].score);
            stu[k].kaochang=i;
        }
        num[i]=num_kaochang;
        sort(stu+sum,stu+sum+num_kaochang, cmp1);
        stu[sum].paiming=1;
        for(int m=sum;m<sum+num_kaochang;m++)
        {
            if(stu[m].score==stu[m-1].score)
            {
                stu[m].paiming=stu[m-1].paiming;
            }
            else
            {
                stu[m].paiming=m+1-sum;//局部排名数值要减去sum
            }
        }
        sum+=num_kaochang;
    }
    sort(stu,stu+sum, cmp2);
    for(int i=0;i<sum;i++)
    {
        printf("%s %d %d\n",stu[i].name,stu[i].score,stu[i].paiming);
    }
    return 0;
}
```

+ 总结：注意一下**字符串数组的比较函数**的写法，以及这道题目局部（考场）排名的大小需要减去`sum`值。

例题：[A1025 PAT Ranking](https://pintia.cn/problem-sets/994805342720868352/exam/problems/994805474338127872?type=7&page=0)

+ 代码：

```cpp
#include <cstdio>
#include <string.h>
#include <iostream>
#include <algorithm>
using namespace std;
const int MAXN = 51000;
//结构体
struct stu{
    char name[20];
    int score;
    int kaochang;
    int paiming;
    int final_rank;
};

//比较函数
bool cmp1(stu a,stu b)
{
    return a.score>b.score;
}

bool cmp2(stu a,stu b)
{
    if(a.final_rank==b.final_rank)
    {
        return strcmp(a.name,b.name)<0;
    }
    else
    {
        return a.final_rank<b.final_rank;
    }

}
//主函数
int main(){
    int n,num_kaochang,sum=0;
    scanf("%d",&n);
    stu stu[MAXN];
    for(int i=0;i<n;i++)
    {
        scanf("%d",&num_kaochang);
        for(int k=sum;k<num_kaochang+sum;k++)
        {
            scanf("%s",stu[k].name);
            scanf("%d",&stu[k].score);
            stu[k].kaochang=i+1;
        }
        //排local_rank
        sort(stu+sum,stu+sum+num_kaochang, cmp1);
        stu[sum].paiming=1;
        for(int m=sum;m<sum+num_kaochang;m++)
        {
            if(stu[m].score==stu[m-1].score)
            {
                stu[m].paiming=stu[m-1].paiming;
            }
            else
            {
                stu[m].paiming=m+1-sum;//局部排名数值要减去sum
            }
        }
        sum+=num_kaochang;
    }
    //按照成绩重新进行排名，不区分考场
    sort(stu,stu+sum, cmp1);
    stu[0].final_rank=1;
    for(int m=1;m<sum;m++)
    {
        if(stu[m].score==stu[m-1].score)
        {
            stu[m].final_rank=stu[m-1].final_rank;
        }
        else
        {
            stu[m].final_rank=m+1;
        }
    }
    //按照字典序排输出
    sort(stu,stu+sum, cmp2);
    //输出
    printf("%d\n",sum);
    for(int i=0;i<sum;i++)
    {
        printf("%s %d %d %d\n",stu[i].name,stu[i].final_rank,stu[i].kaochang,stu[i].paiming);
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```

+ 总结：可以在**结构体数组**中把对应要输出的内容提前定义，这样在运算赋值之后就可以直接输出。

### 散列

#### 散列的定义和整数散列

+ 散列(Hash)，简单而言，就是将**元素**通过一个**函数**转换为**整数**，使得该整数可以尽量**唯一地**代表这个元素。
+ 其中把这个转换函数称为**散列函数H**，也就是说，如果元素在转换前为`Key`，那么转换后为一个整数`H(Key)`。

+ 常用的散列函数：**直接定址法**、**平方取中法**、**除留余数法**等......
+ 如果两个不同的元素`Key1`和`Key2`，它们的Hash值`H(Key1)`和`H(Key2)`是相同的话，就称为**冲突**。
+ 解决冲突的主要办法有：**线性探查法**、**平方探查法**、**链地址法(拉链法)**
+ 其中第一种和第二种都计算了新的**Hash值**，称为**开放定址法**
+ 散列表的特点是能够使用空间来换取时间

#### 字符串Hash初步

+ **字符串Hash**是指将一个字符串`Str`映射成一个整数，使得该整数可以尽可能唯一地代表字符串`Str`。
+ 为了讨论问题方便，先假设字符串均有大写字母`'A'-'Z'`组成，在此基础上，不妨把大写字母`'A'-'Z'`看成`0-25`。
+ 由此便可以将字符串映射为整数(注意：转换成整数最大为 $26^{1en}-1$ 其中 `len` 为字符串长度)
+ 代码如下：

```cpp
int HashFunc(char Str[],int len)//Hash函数，将字符串Str转换为整数
{
	int id=0;
	for(int i=0;i<len;i++)
	{
		id=id*26+(Str[i]-'A');//转换为整数
	}
	return id;
}
```

+ 如果字符串中出现了小写字母，那么可以把大写字母`'A'-'Z'`看成`0-25`，把小写字母`'a'-'z'`看成`26-51`，其余相同。

+ 代码：

```cpp
int HashFunc(char Str[],int len)//Hash函数，将字符串Str转换为整数
{
	int id=0;
	for(int i=0;i<len;i++)
	{
		if(Str[i]>='A'&&Str[i]<='Z')
		{
			id=id*52+(Str[i]-'A');//转换为整数
		}
		else if(Str[i]>='a'&&Str[i]<='z')
		{
			id=id*52+(Str[i]-'a')+26;//转换为整数
		}
	}
	return id;
}
```

+ 再增加不同的字符代码编写方式**同理**；

例题：[字符串出现次数](https://sunnywhy.com/sfbj/4/2/105)

+ 代码：

```cpp
#include <cstdio>
#include <string.h>
#include <iostream>
#include <algorithm>
using namespace std;
const int MAXN = 1010;
char str[MAXN][5];
int hashTable[26*26*26+10]={0};
//字符串转Hash函数
int HashFunc(char s[],int len)
{
    int id=0;
    for(int i=0;i<len;i++)
    {
        id=id*26+(s[i]-'A');
    }
    return id;
}

//主函数
int main(){
    int n,m;
    scanf("%d",&n);
    for(int i=0;i<n;i++)
    {
        scanf("%s",str[i]);
        hashTable[HashFunc(str[i],3)]++;
    }
    scanf("%d",&m);
    for(int i=0;i<m;i++)
    {
        scanf("%s",str[i]);
        printf("%d",hashTable[HashFunc(str[i],3)]);
        if(i!=m-1)
        {
            printf(" ");
        }
    }
    return 0;
}
```

+ 总结：该题直接给出字符串散列的处理方法！重点掌握字符串转整数函数的编写和应用。

例题：[2-SUM-hash](https://sunnywhy.com/sfbj/4/2/104)

+ 代码：

```cpp
#include <cstdio>
#include <string.h>
#include <iostream>
#include <algorithm>
using namespace std;
const int MAXN = 1000001;
int num[MAXN]={0},hashTable[MAXN]={0};
//主函数
int main(){
    int n,k;
    scanf("%d %d",&n,&k);
    for(int i=0;i<n;i++)
    {
        scanf("%d",&num[i]);
        hashTable[num[i]]++;
    }
    int ans=0;
    for(int i=0;i<n;i++)
    {
        if(k-num[i]>=0&&hashTable[k-num[i]])
        {
            ans++;
        }
    }
    printf("%d",ans/2);
    return 0;
}
```

+ 总结：这道题目的巧妙之处在于通过用求和值`k`减去`a`后的值`b`是否还在**哈希表**中来判断是否满足条件。这样巧妙利用了**空间换时间**的思想，只用一次循环即可完成！最后注意最终结果需要再`÷2`！

### 递归

分治

+ 分治->"分而治之"
+ 分治法将原问题划分成若干个**规模较小**而**结构**与原问题**相同**或者**相似**的子问题，然后分别解决这些子问题，最后**合并**子问题的解，即可得到原问题的解。
  + 分解：将原问题划分成若干个**规模较小**而**结构**与原问题**相同**或者**相似**的子问题；
  + 解决：递归求解所有子问题。如果存在子问题的规模足够小就可以直接解决；
  + 合并：将子问题的解合并为原问题的解。
+ 分治法分解成的子问题应该是相互独立的、没有交叉的。
+ 分治法作为一种算法思想，既可以使用**递归**的手段去实现，也可以通过**非递归**的手段去实现。

递归

+ 递归在于**反复调用自身函数**，但是每次把**问题范围缩小**，直到范围缩小到可以直接得到边界数据的结果，然后再在返回的路上求出对应的解。
+ 递归很适合用来实现分治思想；
+ 递归的逻辑中一般有两个重要概念：
  + 递归边界
  + 递归式（或称递归调用）
+ 递归式是将原问题分解为若干个子问题的手段；
+ 递归边界是分解的尽头。
+ 例题->递归求解n的阶乘：

```cpp
#include <cstdio>
#include <string.h>
using namespace std;
//函数
int F(int n)
{
    if(n==0) return 1;//递归边界
    else return F(n-1)*n;//递归式
}
//主函数
int main(){
    int a=3;
    printf("%d",F(a));
    return 0;
}
```

+ 输出：

```
6
```

+ 其实现过程如下：

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202311041745796.png)

+ 例题->递归求解斐波那契数列的第n项：

```cpp
#include <cstdio>
#include <string.h>
#include <iostream>
#include <algorithm>
using namespace std;
//斐波那契数列递归函数（输出第n项的值）
int F(int n)
{
    if(n==0||n==1)
        return 1;//递归边界
    else return F(n-1)+F(n-2);//递归式
}
//主函数
int main(){
    int n=4;
    printf("%d",F(n));
    return 0;
}
```

+ 输出：

```
5
```

+ 其实现过程如下：

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202311041755805.png)

+ 例题->**全排列问题**：

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202311041758963.png)

+ **思路**：
+ 从递归的角度考虑，把问题描述成："输出**1 - n**这**n**个整数的全排列"，那么它就可以分解成若干个子问题：
  + 输出以1开头的全排列：`(1,2,3)`、`(1,3,2)`;
  + 输出以2开头的全排列：`(2,1,3)`、`(2,3,1)`;
  + 输出以3开头的全排列：`(3,1,2)`、`(3,2,1)`;
  + 以此类推......直到第n个。
+ 由此，不妨设定一个数组`p[MAXN]`用于存放当前的排列;
+ 再设定一个散列数组`bool HashTable[MAXN]={false};`用于指示当前元素k是否在数组`p`中，
  + 如果已经存在于`p`中时`HashTable[k]=true;`
  + 如果不存在于`p`中时`HashTable[k]=false;`
+ 因为要按照**字典序**对全排列进行输出，我们需要按顺序往数组`p`中第1位到n位中填入数字。
+ 不妨假设我们当前已经填好了`p[1]-p[index]`部分的数字，下一步需要填`P[index+1]`这个位置的数字。
+ 显然需要从1-n中枚举有哪些数字还没有在`p[1]-p[index]`部分，即满足`HashTable[k]==false`这个条件，那么就将该数字填入`p[index]`中。
+ 然后将`HashTable[k]=true`，表示`k`这个数据已经填入了数组`p`中。
+ 然后可以像上述步骤一样处理`index+2`的数据，即`p[1]-p[index+1]`已经填好，即进行递归->重复执行`Full_permutation(index+1);`直到后续**递归完成**。
+ 当递归完成后，需要再将`HashTable[k]=false`，以便后续能够继续使用这个数据。
+ 最后**递归边界**显然是当`index`到达`n+1`时，说明数组`p`中的第**1 - n**位都已经填好了，只需要按顺序进行输出即可。
+ 下面是当`n=3`时候的代码：

```cpp
#include <cstdio>
#include <string.h>
#include <iostream>
#include <algorithm>
using namespace std;
//全排列递归函数变量定义
const  int MAXN = 20;
int n;//输出index-n的全排列
int p[MAXN];
bool HashTable[MAXN]={false};
//全排列递归函数
void Full_permutation(int index)
{
    //递归边界
    if(index==n+1)
    {
        for(int i=1;i<=n;i++)
        {
            printf("%d",p[i]);
        }
        printf("\n");
        return ;
    }
    //递归式
    for(int k=1;k<=n;k++)
    {
        if(!HashTable[k])//HashTable[k]==false->说明该元素还没有被用上
        {
            p[index]=k;//处理这一种情况
            HashTable[k] = true;//到这里说明假设1到index已经排好
            //递归进入函数再排index+1之后的部分
            Full_permutation(index+1);
            //递归返回结束后循环还没有结束，继续处理下一循环的问题
            HashTable[k] = false;//已经处理完p[index]=k;这一种情况，还原状态
        }
    }
}

//主函数
int main(){
    n=3;
    Full_permutation(1);//index从1开始
    return 0;
}
```

+ 输出：

```
123
132
213
231
312
321
```

+ 例题->**n皇后问题**：

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202311051551338.png)

+ 思路：

+ 根据题意很容易想到**每行**和**每列**只能放置一个皇后，只需要将**n列**或者**n行**皇后的位置写出即可代表一种情况。
+ 例如将皇后的行号写出，图4-4a的序号为`24135`，图4-4b的序号为`35142`。
+ 按照这个思路只需要枚举**1 - n**的所有排列，并且查看每个排列对应的放置方案是否合法，统计合法的方案即可，总共只有`n!`个排列。
+ 可以参考全排列的方法，生成一段排列序号后，在递归边界判断序号是否合法，代码如下：

```cpp
#include <cstdio>
#include <string.h>
#include <iostream>
#include <algorithm>
#include <cmath>
using namespace std;

//n皇后函数变量定义
const  int MAXN = 20;
int n;//输出index-n的全排列
int p[MAXN];
bool HashTable[MAXN]={false};
int my_count = 0;//记录合法的皇后排列个数

//n皇后问题递归函数
void n_queens(int index)
{
    //递归边界
    if(index==n+1)
    {
        bool flag = true;
        for(int i=1;i<n;i++)
        {
            for(int j=i+1;j<=n;j++)
            {
                if(abs(i-j)==abs(p[i]-p[j]))
                {
                    flag = false;
                    break;
                }
            }
        }
        if(flag)
        {
            my_count++;
        }
        return ;
    }
    //递归式
    for(int k=1;k<=n;k++)
    {
        if(!HashTable[k])//HashTable[k]==false->说明该元素还没有被用上
        {
            p[index]=k;//处理这一种情况
            HashTable[k] = true;//到这里说明假设1到index已经排好
            //递归进入函数再排index+1之后的部分
            n_queens(index+1);
            //递归返回结束后循环还没有结束，继续处理下一循环的问题
            HashTable[k] = false;//已经处理完p[index]=k;这一种情况，还原状态
        }
    }
}

//主函数
int main(){
    n=8;
    n_queens(1);//index从1开始
    printf("%d",my_count);
    return 0;
}
```

+ 输出：

```
92
```

+ 总结：
+ 上述方法在序列完成时再判断该序列是否合法，未使用任何优化方法，称为**暴力法**。
+ 事实上，可以发现当已经放置了一部分皇后以后（对应生成了排列的一部分），如果后续皇后无论怎么放置都冲突的话，即可中止递归了。
+ 一般而言，如果在到达**递归边界**前的某层，由于一些事实导致已经不需要再往任何一个子问题递归了，就可以直接返回上一层，一般这种做法称为**回溯法**。
+ 代码如下：

```cpp
#include <cstdio>
#include <string.h>
#include <iostream>
#include <algorithm>
#include <cmath>
using namespace std;
//n皇后函数变量定义
const  int MAXN = 20;
int n;//输出index-n的全排列
int p[MAXN];
bool HashTable[MAXN]={false};
int my_count = 0;//记录合法的皇后排列个数

//n皇后问题递归函数
void n_queens(int index)
{
    //递归边界,到达递归边界都是合法序列
    if(index==n+1)
    {
        my_count++;
        return ;
    }
    //递归式
    for(int k=1;k<=n;k++)
    {
        if(!HashTable[k])//HashTable[k]==false->说明该元素还没有被用上
        {
            p[index]=k;//处理这一种情况
            HashTable[k] = true;//到这里说明假设1到index已经排好
            bool flag = true;
            for(int pre=1;pre<index;pre++)
            {
                if(abs(index-pre)==abs(p[index]-p[pre]))
                {
                    flag = false;
                    break;
                }
            }
            if(flag)
            {
                //递归进入函数再排index+1之后的部分
                n_queens(index+1);
            }
            //递归返回结束后循环还没有结束，继续处理下一循环的问题
            HashTable[k] = false;//已经处理完p[index]=k;这一种情况，还原状态
        }
    }
}

//主函数
int main(){
    n=8;
    n_queens(1);//index从1开始
    printf("%d",my_count);
    return 0;
}
```

+ 输出：

```
92
```

例题：[反转字符串](https://sunnywhy.com/sfbj/4/3/113)

+ 方法一：

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202311051954513.png)

+ 方法一代码：

```cpp
//递归求字符串逆函数1
void rev_1(char* str)
{
    char temp;
    int len;
    temp = *str;
    len = strlen(str);
    *str = *(str+len-1);
    *(str+len-1)='\0';
    if(strlen(str+1)>=2)
    {
        rev_1(str+1);
    }
    *(str+len-1)=temp;
}
```

+ 方法二：

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202311051958349.png)

+ 方法二代码：

```cpp
//递归求字符串逆函数2
void rev_2(char* str,int left,int right)
{
    char temp;
    temp = str[left];
    str[left] = str[right];
    str[right] = temp;
    if(left+1<right-1)
    {
        rev_2(str,left+1,right-1);
    }
}
```

+ 总代码：

```cpp
#include <cstdio>
#include <string.h>
#include <iostream>
#include <algorithm>
#include <cmath>
using namespace std;
const int MAXN = 110;
char str[MAXN],rev_str[MAXN];
int n;
//递归求字符串逆函数1
void rev_1(char* str)
{
    char temp;
    int len;
    temp = *str;
    len = strlen(str);
    *str = *(str+len-1);
    *(str+len-1)='\0';
    if(strlen(str+1)>=2)
    {
        rev_1(str+1);
    }
    *(str+len-1)=temp;
}

//递归求字符串逆函数2
void rev_2(char* str,int left,int right)
{
    char temp;
    temp = str[left];
    str[left] = str[right];
    str[right] = temp;
    if(left+1<right-1)
    {
        rev_2(str,left+1,right-1);
    }
}

//主函数
int main(){
    scanf("%s",str);
    //rev_1(str);
    rev_2(str,0,strlen(str)-1);
    printf("%s", str);
    return 0;
}
```

+ 总结：上述介绍的两种方法可以多深入体会。

例题：[上楼](https://sunnywhy.com/sfbj/4/3/118)

+ 代码：

```cpp
#include <cstdio>
#include <string.h>
#include <iostream>
#include <algorithm>
#include <cmath>
using namespace std;

//递归判断上楼梯方式数
int F(int n)
{
    if(n<=1)
        return 1;
    else
        return F(n-1)+F(n-2);//最后只有加一级或者两级，方案是固定的，所以只需要求出还需要一级到达的总方式数和还需要两级到达的总方式数即可
}

//主函数
int main(){
    int n;
    scanf("%d",&n);
    printf("%d",F(n));
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```

+ 总结：最后要到达最高级只有加**一级**或者**两级**，方案是固定的，所以只需要求出还需要一级到达的总方式数和还需要两级到达的总方式数即可。

例题：[汉诺塔](https://sunnywhy.com/sfbj/4/3/119)

+ 代码：

```cpp
#include <cstdio>
#include <string.h>
#include <iostream>
#include <algorithm>
#include <cmath>
using namespace std;
int my_count=0;
//汉诺塔问题
void hanoi(int n,char from,char to,char mid)
{
    if(n==1)
    {
        printf("%c->%c\n",from,to);
    }
    else
    {
        hanoi(n-1,from,mid,to);//要想移动n级汉诺塔需要先移动n-1级汉诺塔到另一边
        printf("%c->%c\n",from,to);//把最后最大的一块移动到目的位置
        hanoi(n-1,mid,to,from);//最后把剩下n-1级的汉诺塔移动到目标位置
    }
}

//主函数
int main(){
    int n;
    scanf("%d",&n);
    printf("%d\n",(int)pow(2,n)-1);
    hanoi(n,'A','C','B');
    return 0;
}
```

+ 总结：要想移动`n`级汉诺塔需要先移动`n-1`级汉诺塔到另一边，然后把最后最大的一块移动到目的位置，最后把剩下`n-1`级的汉诺塔移动到目标位置，从而形成递归。

例题：[棋盘覆盖问题](https://sunnywhy.com/sfbj/4/3/120)

+ 说明：这道题目是一道典型的二维**分治问题**。
+ 思路：要想采用**分治**的方法并且使用**递归**来进行求解，就需要划分成相同方案的子问题，划分的思路如下：

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202311071535777.png)

+ 以此类推，在划分到只剩下2×2大小的方块后就很容易地采用骨牌进行填充。
+ 代码如下：

```cpp
#include <cstdio>
#include <string.h>
#include <iostream>
#include <algorithm>
#include <cmath>
using namespace std;
const int MAXN = 256*256;
int my_index = 0;

//建立坐标结构体
struct point
{
    int x , y;
    //原始构造函数，不需要初始化变量
    point(){}
    //传参构造函数
    point(int _x,int _y)
    {
        x=_x;
        y=_y;
    }
};
//定义存储的点数组
point arr[MAXN];

//递归获取棋盘覆盖函数
/*
x,y是左下角方格坐标，代表原点
cx,cy是黑点坐标
size是传入此函数时整体方格的大小
*/
void chees_cover(int x,int y,int cx,int cy,int size)
{
    int h;
    if(size == 1)
        return;
    h = size/2;
    //黑色方格在左上角
    if(cy>=y+h&&cx<x+h)//假如黑色方块在左上角
    {
        arr[my_index++]=point(x+h,y+h-1);//从黑色方块在左上角
        //确认骨牌的原点在右下角
        //以下的三个if语句同理
        chees_cover(x,y+h,cx,cy,h);
    }
    else
    {
        chees_cover(x,y+h, x+h-1,y+h,h);//
    }
    //黑色方格在右上角
    if(cy>=y+h&&cx>=x+h)
    {
        arr[my_index++]=point(x+h-1,y+h-1);
        chees_cover(x+h,y+h,cx,cy,h);
    }
    else
    {
        chees_cover(x+h,y+h,x+h,y+h,h);
    }
    //黑色方格在左下角
    if(cy<y+h&&cx<x+h)
    {
        arr[my_index++]=point(x+h,y+h);
        chees_cover(x,y,cx,cy,h);
    }
    else
    {
        chees_cover(x,y,x+h-1,y+h-1,h);
    }
    //黑色方格在右下角
    if(cy<y+h&&cx>=x+h)
    {
        arr[my_index++]=point(x+h-1,y+h);
        chees_cover(x+h,y,cx,cy,h);
    }
    else
    {
        chees_cover(x+h,y,x+h,y+h-1,h);
    }
}

//定义排序比较函数
int cmd(point px,point py)
{
    if(px.x==py.x)
    {
        return px.y < py.y;
    }
    else
    {
        return px.x < py.x;
    }
}

//主函数
int main(){
    int k,cx,cy,size;
    scanf("%d%d%d",&k,&cx,&cy);
    size = (int)pow(2,k);
    chees_cover(1,1,cx,cy,size);
    sort(arr,arr+my_index,cmd);
    for(int i=0;i<my_index;i++)
    {
        printf("%d %d\n",arr[i].x,arr[i].y);
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```

例题：[谢尔宾斯基地毯](https://sunnywhy.com/sfbj/4/3/123)

+ 代码：

```cpp
#include <cstdio>
#include <string.h>
#include <iostream>
#include <algorithm>
#include <cmath>
using namespace std;
const int MAXN = 3*3*3*3*3*3*3;
char arr[MAXN][MAXN];

//定义谢尔宾斯基地毯递归函数
void cover(int n,int x,int y)
{
    int unit = (int)pow(3.0,n-2);
    if(n==1)
    {
        arr[x][y]=' ';
    }
    else
    {
        cover(n-1,x,y);
        cover(n-1,x,y+unit);
        cover(n-1,x,y+2*unit);
        cover(n-1,x+unit,y);
        for(int i=x+unit;i<x+2*unit;i++)
        {
            for(int j=y+unit;j<y+2*unit;j++)
            {
                arr[i][j]='X';
            }
        }
        cover(n-1,x+unit,y+2*unit);
        cover(n-1,x+2*unit,y);
        cover(n-1,x+2*unit,y+unit);
        cover(n-1,x+2*unit,y+2*unit);
    }
}

//主函数
int main(){
    int n,my_unit;
    scanf("%d",&n);
    my_unit = pow(3.0,n-1);
    for(int i=0;i<my_unit+2;i++)
    {
        for(int j=0;j<my_unit+2;j++)
        {
            if(i==0||i==my_unit+1||j==0||j==my_unit+1)
            {
                arr[i][j]='+';
            }
            else
                arr[i][j]=' ';
        }
    }
    cover(n,1,1);
    //printf("%d %d",n,my_unit);
    for(int i=0;i<my_unit+2;i++)
    {
        for(int j=0;j<my_unit+2;j++)
        {
            printf("%c",arr[i][j]);
        }
        printf("\n");
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```

+ 总结：这种题目主要找准递归的**起始位置**，根据**起始位置**即可输出完整图形。

例题：[有限制的选数II](https://sunnywhy.com/sfbj/4/3/141)
+ 代码：
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <cmath>
#include <string>
#include <vector>
using namespace std;
const int MAX = 15;
int n,k,ans=0;
int num[MAX];

//递归函数求解有限制的选数
void F(int index,int sum)
{
    //递归边界
    if(index==n)
    {
        if(sum==k)
            ans++;
        return;
    }
    //递归式
    for(int i=0;i<=(k-sum)/num[index];i++)//在循环中加上了本身的数据
    {
        F(index+1,sum+i*num[index]);//根据加上了本身的数据再继续往后递归
    }
}

//主函数
int main(){
    scanf("%d %d",&n,&k);
    for(int i=0;i<n;i++)
    {
        scanf("%d",&num[i]);
    }
    F(0,0);
    printf("%d",ans);
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 总结：这道题目的巧妙之处在于设计了在循环中加上了**本身的数据**，随后根据加上了本身的数据再继续往后递归。

例题：[有限制的选数III](https://sunnywhy.com/sfbj/4/3/142)
+ 代码：
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <cmath>
#include <string>
#include <vector>
using namespace std;
const int MAX = 15;
const int MAXN = 150;
int n,k,ans=0,m;
int num[MAX],p[MAX];
int hashTable[MAXN] = {0};

//递归函数求解有限制的选数
void F(int index,int sum)
{
    //递归边界
    if(index==m)
    {
        if(sum==k)
            ans++;
        return;
    }
    //递归式
    for(int i=0;i<=min((k-sum)/p[index],hashTable[p[index]]);i++)//去除重复数据
    {
        F(index+1,sum+i*p[index]);
    }
}

//主函数
int main(){
    scanf("%d %d",&n,&k);
    //去除重复数据输入并且写入重复的数量到哈希表中
    m=n;
    int t[MAX];
    for(int i=0;i<n;i++)
    {
        scanf("%d",&t[i]);
        hashTable[t[i]]++;
        if(i>0&&t[i]==t[i-1])
        {
            m--;
        }
    }
    int l=0;
    for(int i=0;i<MAXN;i++)
    {
        if(hashTable[i]!=0)
        {
            p[l]=i;
            l++;
        }
    }
    // printf("%d\n",m);
    // for(int i=0;i<m;i++)
    // {
    //     printf("%d ",p[i]);
    // }
    F(0,0);
    printf("%d",ans);
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 总结：这道题目的与上一道题目思路类似，巧妙之处同样在于设计了在循环中加上了本身的数据，随后根据加上了本身的数据再继续往后递归，不同点在于所能加上自身的数据从小于等于 `k` 变成了小于等于 `k` 和**输入的数据本身的数量**。

例题：[背包问题](https://sunnywhy.com/sfbj/4/3/143)
+ 代码：
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <cmath>
#include <string>
#include <vector>
using namespace std;
const int MAX = 15;
const int MAXN = 10000110;
int n,k;
int num[MAX],value[MAXN];
int max_value = -1;

//递归函数求解有限制的选数
void F(int index,int sum,int temp_value)
{
    //递归边界
    if(index==n)
    {
        max_value = max(max_value,temp_value);
        //printf("%d %d\n",sum,max_value);
        return;
    }
    //递归式
    if(sum+num[index]<=k)//在进入增加总容量的递归前判断是否超过了背包容量，不一定能够正好达到背包最大容量
    {
        F(index+1,sum+num[index],temp_value+value[index]);
    }
    F(index+1,sum,temp_value);
}

//主函数
int main(){
    scanf("%d %d",&n,&k);
    for(int i=0;i<n;i++)
    {
        scanf("%d",&num[i]);
    }
    for(int i=0;i<n;i++)
    {
        scanf("%d",&value[i]);
    }
    F(0,0,0);
    printf("%d",max_value);
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 总结：这道题目与上面的[有限制的选数](https://sunnywhy.com/sfbj/4/3/140)题目思路基本类似，主要不同点在于提供的数据**不一定能够达到背包的最大容量**，因此需要在进入增加总容量的递归前判断是否**超过**了背包容量。

例题：[生成括号对](https://sunnywhy.com/sfbj/4/3/144)
+ 思路：
+ n 个括号对，也就是说一共 2 n 个字符，我们可以枚举 n 个`'('`分别放在什么位置，剩下的自然就是`')'`了。看起来很有道理，但是有一个问题，就是这个思路并没有办法通过循环直接实现。这其实已经进化成了一个搜索问题了，我们要搜索所有可以摆放括号的可能性。
+ 对于搜索问题而言，这已经很简单了，我们搜索的空间是明确的，2 n 个位置，搜索的内容，对于每个位置我们可以摆放`'('`也可以摆放`')'`。
+ 我们来思考一个问题：什么情况会出现右括号 `')'` 遇不到左括号 `'('` 呢？只有一种情况，就是当前出现右括号的个数超过了左括号，也就是说我们遍历一下字符串，如果中途出现右括号数量超过左括号的情况，那么就说明这个字符串是非法的。
+ 看起来没毛病对吧，但是有问题，我们为什么不在枚举的时候就判断呢，如果左括号放入的数量已经等于右括号了，那么就不往里放置右括号，这样不就可以保证搜索到的一定是合法的字符串吗？
+ 代码：
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <cmath>
#include <string>
#include <vector>
#include <set>
using namespace std;

//变量
int n;
set<string> st;

//递归函数生成括号对
void F(int pos,int left,int right,string cur_str)
{
    //pos:当前字符串长度
    //left:左括号数量
    //right:右括号数量
    //cur_str:当前字符串
    if(pos==2*n)
    {
        st.insert(cur_str);
    }
    if(left<n)
    {
        F(pos+1,left+1,right,cur_str+'(');
    }
    if(right<n&&right<left)
    {
        F(pos+1,left,right+1,cur_str+')');
    }
}

//主函数
int main(){
    scanf("%d",&n);
    F(0,0,0,"");
    for(set<string>::iterator it = st.begin();it!=st.end();it++)
    {
        cout << *it << endl;
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 总结：在 `right < n` 后面加入了一个 `right < left` 这个条件。看似只有一个条件，但是这个条件起到的作用至关重要。整个算法的效率有了质的提升，实际上这也是**效率最高**的算法。

例题：[加号之和](https://sunnywhy.com/sfbj/4/3/145)
+ 思路：以分解 **361** 为例：

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20231125141921.png)

+ 代码：
```cpp
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <cmath>
#include <string>
#include <vector>
#include <set>
using namespace std;

//变量
const int MAXN = 9;//字符串的最大长度
char data[MAXN];//存储数字
int n;

//函数
//head:开始字符的下标
//tail:结尾字符的下一个位置
int get_num(int head,int tail){
    int sum=0;
    for(int i=head;i<tail;i++){
        sum=sum*10+data[i]-'0';
    }
    return sum;
}

int sum_num(int head,int tail,int len){
    if(head+1==tail){
        return get_num(head,tail);
    }else{
        //printf("head=%d tail=%d len=%d##\n",head,tail,len);
        int sum=get_num(head,tail);
        //printf("sum=%d#\n",sum);
        for(int i=head+1;i<tail;i++){
            sum+=get_num(head,i)*pow(2,tail-i-1)+sum_num(i,tail,tail-i);
            //printf("get_num=%d#\n",get_num(head,i));
        }
        return sum;
    }
}

//主函数
int main (){ 
    scanf("%s",data);
    n=strlen(data);
    printf("%d",sum_num(0,n,n));
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 总结（关键思想）：
+ 上述代码左边字符为什么使用 `get_num(head,i)*pow(2,tail-i-1)`，而不是继续进行递归，而右边继续进行递归，主要是因为：
+ 第一步：361 分解为 3 和 61 时，61 总共有 2^1=2 种划分，即单独的{61}以及{6,1}，和左边在一起求和时，即：3+61 以及 3+6+1，3 出现了 61 的划分次数 2，即所乘的 `pow (2,tail-i-1)` 。
+ 第二步：361分解为36,1时，因为在划分为3,61情况时，已经出现过3,6,1这种情况，为了避免重复出现3,6,1的情况，将36不在进行划分（因为其对应的所有划分情况在上一步已经出现过了），即直接使用 `get_num(head,i)`！

#### 一种递归式的非零自然数全分解方法

+ 在开始讲之前，首先介绍一下这个方法针对的问题背景：一个非零自然数(1,2,3,……)既不重复也不遗漏地任意分解为非零自然数(如：3=1+1+1=1+2)，在本篇暂且称为非零自然数的全分解。
+ 在非零自然数的全分解中，总共有多少种分解方法，并列出所有分解方法，在本篇暂且称为非零自然数的全分解问题。

##### 基本概念

1. **分解末项**
   + 一个分解中的最后一项称为分解末项。如“3=1+2”中分解末项为“2”，再如“3=1+1+1”中分解末项为“1”。
2. **分解基数B**
  + 分解基数，在数值上定义为分解末项的前一项，举个例子：“5=1+4”称为分解基数B=1的一个分解，“5=1+2+2”称为分解基数B=2的一个分解。
  + 我们也可以把“5=1+4”到“5=1+2+2”的过程理解为一个将分解末项“4”按分解基数B=2的分解。实际上这种理解更为重要，因为在本方法中，我们本质上也是针对分解末项的分解。

##### 分解规则

1. 关于分解基数
     + **分解基数单调不减**。如：“7=2+5=2+1+4”为一个错误的分解过程，因为第一级分解基数为2，第二级分解基数为1，违反分解基数单调不减原则。所以，“7=2+5=2+2+3”才是一个正确的分解过程。
2. 关于分解末项
     + **分解末项应不小于分解基数**。如：“5=1+1+3”为一个正确的分解，“5=1+3+1”为一个错误的分解。

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202311102020016.jpeg)

+ 根据前述的两条分解规则，对7的全分解过程如上图所示，可以看到总共有14种分解方法。实际上，7的全分解就是这14种分解方法。

例题：[自然数分解之方案数](https://sunnywhy.com/sfbj/4/3/125)

+ 代码：

```cpp
#include <cstdio>
#include <string.h>
#include <iostream>
#include <algorithm>
#include <cmath>
using namespace std;

//递归求解自然数分解方案数量函数
int func(int pre,int now)
{
    int temp=0;
    for(int i=1;2*i<=now;i++)
    {
        if(i>=pre)
        {
            temp+=func(i,now-i);
            temp++;
        }
    }
    return temp;
}

//主函数
int main(){
    int n,num;
    scanf("%d",&n);
    num = func(0,n);
    printf("%d",num);
    return 0;
}
```

+ 总结：
  + **递归边界**是：当我们需要拆分的数为1时，表示无法拆分，因此返回0。
  + 总而言之，`func(pre,now)`所返回的整数表示该组合后续能够拆分的总数。

例题：[自然数分解之最大积](https://sunnywhy.com/sfbj/4/3/124)

+ 代码：

```cpp
#include <cstdio>
#include <string.h>
#include <iostream>
#include <algorithm>
#include <cmath>
using namespace std;

//递归求解自然数分解之最大积
int func(int pre,int now)
{
    int my_max=-1;
    for(int i=1;2*i<=now;i++)
    {
        if(i>=pre)
        {
            my_max=max(my_max,max(i*(now-i),func(i,now-i)));
        }
    }
    return max(my_max,pre*my_max);
}

//主函数
int main(){
    int n,num;
    scanf("%d",&n);
    num = func(0,n);
    printf("%d",num);
    return 0;
}
```

+ 总结：这题与**自然数分解之方案数**较为相似，只需要把递归函数 `temp` 的计数改为计算乘积最大值即可。

#### 动态规划

例题：[数塔](https://sunnywhy.com/sfbj/4/3/116)

+ 思路：数塔问题是经典的动态规划问题，通过归纳可以得到一个信息：

  + 如果要求出`dp[i][j]`,那么一定要求出其两个子问题：
  + 从位置`(i+1,j)`到达最底层的最大和`dp[i+1][j]`;
  + 从位置`(i+1,j+1)`到达最底层的最大和`dp[i+1][j+1]`;
  + 即进行了一次决策，走位置`(i,j)`的左下还是右下：
  + 式子如下：

  ```cpp
  dp[i][j]=max(dp(i+1,j),dp(i+1,j+1))+f[i][j];
  ```

  + 把`dp[i][j]`称为问题的**状态**，而把上面的式子称为**状态转移方程**。

+ 例题代码：

```cpp
#include <cstdio>
#include <string.h>
#include <iostream>
#include <algorithm>
#include <cmath>
using namespace std;
const int MAXN = 25;
int f[MAXN][MAXN],dp[MAXN][MAXN];
int n;
//数塔递归函数（动态规划）
int getMax(int i,int j)
{
    if(i==n)
    {
        return f[n][j];
    }
    else
    {
        dp[i][j]=max(getMax(i+1,j),getMax(i+1,j+1))+f[i][j];
        return dp[i][j];
    }
}
//主函数
int main(){
    scanf("%d",&n);
    for(int k=1;k<=n;k++)
    {
        for(int m=1;m<=k;m++)
        {
            scanf("%d",&f[k][m]);
        }
    }
    dp[1][1]= getMax(1,1);
    printf("%d",dp[1][1]);
    return 0;
}
```
### 贪心
#### 简单贪心
+ 贪心法是求解一类最优化问题的方法，它总是考虑在当前状态下**局部最优 (或较优)**。
+ 显然，如果采取较优而非最优的策略 (最优策略可能不存在或不易想到)，得到的全局结果也无法是最优的。而要获得最优结果，则要求**中间的每步策略都是最优的**，因此严谨使用贪心法来求解最优化问题需要对采取的策略进行证明。
+ 证明的一般思路是使用**反证法**以及**数学归纳法**，即假设策略不能导致最优解，然后通过一系列推导来得到矛盾，以此证明策略是最优的，最后用**数学归纳法**保证全局最优。
+ 因为贪心的证明往往比贪心的实现本身更加困难，因此在某个可行的策略以及无法举出反例的情况下可以勇敢地实现。

例题：[PAT B1020 月饼](https://pintia.cn/problem-sets/994805260223102976/exam/problems/994805301562163200?type=7&page=0)
+ **题意：**
+ 现有月饼需求量为 `D`，已知 `n` 种月饼各自的库存量和总售价，问如何销售这些月饼，使得可以获得的收益最大，并求最大收益。
+ **思路：**
1. 这里采用“总是选择单价最高的月饼出售，可以获得最大的收入”的策略。
+ 因此，对每种月饼，都根据其库存量和总售价来计算出该种月饼的**单价**。
+ 之后，将所有月饼按单价从高到低排序。
2. 从单价高的月饼开始枚举：
- 如果该种月饼的库存量不足以填补所有需求量，则将该种月饼全部卖出，此时需求量减少该种月饼的库存量大小，收益值增加该种月饼的总售价大小。
- 如果该种月饼库存量足够供应需求量，则只需要提高需求量大小的月饼，此时收益值增加当前需求量乘以该种月饼的单价，而需求量减为 0。
- 最后即可得到收益值即为所求的最大收益值。
- 策略正确性证明：
- 假设有两种单价不同的月饼，其单价分别为 `a` 和 `b` (`a<b`)。如果当前需求量为 `K`，那么两种月饼的总收入分别为 `aK` 和 `bK` ，而 `aK<bK` 显然成立，因此需要出售单价更高的月饼。
- **注意点：**
1. 月饼**库存量**和**总售价**可以是浮点数（题目中只说是正数，没说是正整数），需要用 `double` 型存储。对于**总需求量** `D` 虽然题目说是正整数，但是为了后面计算方便，也需要定义为**浮点型**。
2. 当月饼库存量高于需求量时，不能先令需求量为 0，然后再计算收益，这会导致该步收益为 0。
3. 当月饼库存量高于需求量时，记得将循环中断，否则会出错。
+ 代码：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <stack>
#include <string>
#include <iostream>
#include <utility>
#include <map>
#include <algorithm>
#include <vector>
using namespace std;

//月饼结构体
struct Mooncake
{
    double num;//月饼库存量
    double total_price;//总售价
    double price;//单价
};

//比较函数
bool cmd(Mooncake a,Mooncake b)
{
    return a.price > b.price;
}

//主函数
int main()
{
    vector<Mooncake> vi;
    int n;
    double need;
    double num,total_price;
    scanf("%d %lf",&n,&need);
    for(int i=0;i<n;i++)
    {
        Mooncake m;
        scanf("%lf",&num);
        m.num=num;
        vi.push_back(m);
    }
    for(int i=0;i<n;i++)
    {
        scanf("%lf",&total_price);
        vi[i].total_price = total_price;
        vi[i].price = total_price/vi[i].num;//计算每个月饼的单价
    }
    //完成单价从高到低排序
    sort(vi.begin(),vi.end(),cmd);
    //定义计算值
    double now_need = need;
    double now_total_price = 0;//总收入
    for(int i=0;i<n;i++)
    {
        if(vi[i].num<=now_need)//如果需求量高于月饼库存量
        {
            now_need -= vi[i].num;//则第i种月饼需要全部卖出
            now_total_price += vi[i].total_price;
        }
        else//如果月饼库存量高于需求量
        {
            //那么只需要卖出剩余需求量的月饼
            now_total_price += now_need*vi[i].price;
            break;
        }
    }
    printf("%.2f\n",now_total_price);
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```

例题：[PAT B1023 组个最小数](https://pintia.cn/problem-sets/994805260223102976/exam/problems/994805298269634560?type=7&page=0)
+ **思路：**
+ **策略**是：先从 `1~9` 中选择个数不为 `0` 的最小数输出，然后从 `0~9` 输出数字，每个数字输出次数为其剩余个数。
+ 以样例为例：最高位为个数不为 `0` 的最小的数 `1`，此后 `1` 的剩余个数减 `1`（由 `2` 变为 `1`）。接着按剩余次数（`0` 剩余两个，`1` 剩余一个，`5` 剩余三个，`8` 剩余一个）依次输出所有数。
+ **策略正确性**证明：
+ 首先，由于所有数字必须参与组合，因此最后结果的位数是确定的。然后，由于最高位不能为 `0`，因此从 `[1,9]` 中选择**最小**的数输出（如果存在两个长度相同的数的最高位不同，那么一定是高位小的数更小）。
+ 最后，针对除最高位外的所有位，也是从高位到低位优先选择 `[0,9]` 还存在的最小数输出。
+ **注意点：**
+ 由于第一位不能是 `0`，因此第一个数字必须从 `1~9` 中选择最小的存在的数字，且找到这样的数字之后要及时中断循环。
+ 代码：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <stack>
#include <string>
#include <iostream>
#include <utility>
#include <map>
#include <algorithm>
#include <vector>
using namespace std;

//变量定义
const int MAX = 10;
int hashtable[MAX];

//主函数
int main()
{
    for(int i=0;i<MAX;i++)
    {
        scanf("%d",&hashtable[i]);
    }
    //输出第一位
    for(int i=1;i<MAX;i++)
    {
        if(hashtable[i]!=0)
        {
            printf("%d",i);
            hashtable[i]--;
            break;
        }
    }
    //输出后面位数
    for(int i=0;i<MAX;i++)
    {
        if(hashtable[i]!=0)
        {
            for(int j=0;j<hashtable[i];j++)
            {
                printf("%d",i);
            }
            hashtable[i]=0;
        }
    }
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
#### 区间贪心
+ 通过上述例子，能够对贪心有一个大致了解。以下是一个稍微复杂一些的问题：
+ **区间不相交问题：**
+ 给出 `N` 个开区间 `(x, y)`，从中选择尽可能多的开区间，使得这些开区间两两没有交集。
+ 例如对开区间 `(1,3)`、`(2,4)`、`(3,5)`、`(6,7)` 来说，可以选出最多三个区间 `(1,3)`、`(3,5)`、`(6,7)`，它们互相没有交集。
+ 首先考虑最简单的情况，如果开区间 $I_1$ 被开区间 $I_2$ 包含，如下图 4-5 a 所示，那么显然选择 $I_1$ 是最好的选择，因为如果选择 $I_1$，那么就有更大的空间去容纳其它开区间。

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20231201135728.png)

+ 接下来把所有开区间按左端点 `x` 从大到小排列，如果去除掉区间包含的情况，那么一定有 $y_1>y_2>...>y_n$ 成立，如上图 4-5b 所示。
+ 现在考虑应当如何选取区间，通过观察会发现，$I_1$ 右边有一段是一定不会和其他区间重叠的，如果把它去掉，那么 $I_1$ 的左边剩余部分就会被 $I_2$ 包含，由图 4-5a 的情况可知，应当选择 $I_1$。
+ 因此对这种情况，**总是先选择左端点最大的区间。**

例题：[区间不相交问题](https://sunnywhy.com/sfbj/4/4/152)
+ 代码：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <stack>
#include <string>
#include <iostream>
#include <utility>
#include <map>
#include <algorithm>
#include <vector>
using namespace std;

//变量定义
struct Inteval
{
    int x,y;//开区间左右端点
};

//比较函数
bool cmp(Inteval a,Inteval b)
{
    if(a.x!=b.x)
        return a.x > b.x;//先按左端点从大到小排序
    else
        return a.y < b.y;//左端点相同时按右端点从小到大排序
}

//主函数
int main()
{
    int n;
    vector<Inteval> vi;
    scanf("%d",&n);
    for(int i=0;i<n;i++)
    {
        Inteval in;
        scanf("%d %d",&in.x,&in.y);
        vi.push_back(in);
    }
    sort(vi.begin(),vi.end(),cmp);//把区间排序
    //ans记录不相交区间个数，lastX记录上一个被选中区间的左端点
    int ans = 1,lastX = vi[0].x;
    for(int i=1;i<n;i++)//关键：从一开始循环,因为第一个自身天然算进去了
    {
        if(vi[i].y <= lastX)//如果该区间右端点lastX左边
        {
            lastX = vi[i].x;
            ans++;
        }
    }
    printf("%d\n",ans);
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ **总结：**
+ 值得注意的是，总是先选择**右端点最小**的区间的策略也是可行的，可以模仿上面思路推导，并写出相应代码：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <stack>
#include <string>
#include <iostream>
#include <utility>
#include <map>
#include <algorithm>
#include <vector>
using namespace std;

//变量定义
struct Inteval
{
    int x,y;//开区间左右端点
};

//比较函数
bool cmp(Inteval a,Inteval b)
{
    if(a.y!=b.y)
        return a.y < b.y;//先按右端点从小到大排序
    else
        return a.x > b.x;//右端点相同时按左端点从大到小排序
}

//主函数
int main()
{
    int n;
    vector<Inteval> vi;
    scanf("%d",&n);
    for(int i=0;i<n;i++)
    {
        Inteval in;
        scanf("%d %d",&in.x,&in.y);
        vi.push_back(in);
    }
    sort(vi.begin(),vi.end(),cmp);//把区间排序
    
    //ans记录不相交区间个数，lastY记录上一个被选中区间的右端点
    int ans = 1,lastY = vi[0].y;
    for(int i=1;i<n;i++)//关键：从一开始循环,因为第一个自身天然算进去了
    {
        if(vi[i].x >= lastY)//如果该区间左端点lastY右边
        {
            lastY = vi[i].y;
            ans++;
        }
    }
    printf("%d\n",ans);
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 与上述问题相类似的是**区间选点问题：**
+ 给出 `N` 个闭区间 `[x, y]`，求至少需要确定多少个点，才能使每个闭区间中都至少存在一个点。
+ 例如对于**闭区间**`[1,4]`、`[2,6]`、`[5,7]` 来说，需要两个点（例如 `3`、`5`）才能保证每个闭区间内都至少有一个点。
+ 事实上，这个问题和区间不相交的策略是一致的。
+ 首先，回到图 4-5 a，如果闭区间 $I_1$ 被闭区间 $I_2$ 包含，那么在 $I_1$ 中取点可以保证这个点一定在 $I_2$ 内。
+ 接着，把所有区间按左端点从大到小排序，去除掉区间包含的情况，就可以到图 4-5 b。
+ 显然，由于每个闭区间中都需要存在一个点，因此对左端点最大的区间 $I_1$ 来说，取哪个点可以让它尽可能多地覆盖其他区间？
+ 很显然，只要取左端点即可，这样这个点就能覆盖到尽可能多的区间。
+ 因此**区间选点问题**代码只需要把**区间不相交问题**代码中的条件 `vi[i].y <= lastX` 改为 `vi[i].y < lastX` 即可！
例题：[区间选点问题](https://sunnywhy.com/sfbj/4/4/153)
+ 代码：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <stack>
#include <string>
#include <iostream>
#include <utility>
#include <map>
#include <algorithm>
#include <vector>
using namespace std;

//变量定义
struct Inteval
{
    int x,y;//开区间左右端点
};

//比较函数
bool cmp(Inteval a,Inteval b)
{
    if(a.x!=b.x)
        return a.x > b.x;//先按左端点从大到小排序
    else
        return a.y < b.y;//左端点相同时按右端点从小到大排序
}

//主函数
int main()
{
    int n;
    vector<Inteval> vi;
    scanf("%d",&n);
    for(int i=0;i<n;i++)
    {
        Inteval in;
        scanf("%d %d",&in.x,&in.y);
        vi.push_back(in);
    }
    sort(vi.begin(),vi.end(),cmp);//把区间排序
    //ans记录不相交区间个数，lastX记录上一个被选中区间的左端点
    int ans = 1,lastX = vi[0].x;
    for(int i=1;i<n;i++)//从一开始循环,因为第一个自身天然算进去了
    {
        if(vi[i].y < lastX)//关键：如果该区间右端点lastX左边
        {
            lastX = vi[i].x;
            ans++;
        }
    }
    printf("%d\n",ans);
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 总的来说，贪心是用来解决一类**最优化问题**，并希望**由局部最优策略来推得全局最优结果**的算法思想。
+ 贪心算法适用的问题一定满足**最优子结构**性质，即一个问题的最优解可以由它的子问题的最优解有效地构造出来。
+ 显然，不是所有问题都适合使用贪心法，但是这并不妨碍**贪心算法**成为一个简洁、实用、高效的算法。

#### 贪心算法例题
例题：[拼接最小数](https://sunnywhy.com/sfbj/4/4/154)
+ **思路：**
+ 给定 `n` 个可能含有前导 `0` 的数字串，将它们按任意顺序拼接，使生成的整数最小。
+ 这道题其实思路很简单，就是对输入的字符串进行排序，然后就是前导 `0`，和全部为 `0` 的情况的特例考虑一下。
+ 但其实事实上会有个小坑点在里面，就是排序不是简单的字符串`a<b`。
+ 因为会有这样的例子：
+ `47`，`470` 和 `470`，如果要输出应该是 `47047047` 这样比 `47470470` 要小！ 
+ 因此排序应该是 `a+b < b+a`；
+ 代码如下：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <stack>
#include <string>
#include <iostream>
#include <utility>
#include <map>
#include <algorithm>
#include <vector>
using namespace std;

//比较函数
bool cmp(string a,string b)
{
    return a+b < b+a;//注意:关键点
}

//主函数
int main()
{
    int n;
    scanf("%d",&n);
    vector<string> vi;
    string str;
    for(int i=0;i<n;i++)
    {
        cin >> str;
        vi.push_back(str);
    }
    //排序
    sort(vi.begin(),vi.end(),cmp);
    //输出答案
    string ans = "";
    for(int i=0;i<n;i++)
    {
        ans+=vi[i];
    }
    //去除前导0
    while(ans[0]=='0')
    {
        ans.erase(ans.begin());
    }
    if(ans.length()==0)
        printf("0");
    else
        cout << ans << endl;
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 总结：在做贪心问题的时候注意看清题意并找出题目中和自己代码中的需要关注的细节，必要时可以举些例子来更清楚地理解。

### 二分
#### 二分查找
+ 先来看**猜数字**的游戏。
+ 在这个游戏中，玩家 `A` 从一个范围中选择一个数（例如从 `[1,1000]` 中选择了 352），然后让玩家 `B` 猜这个数字。
+ 此时，如果玩家 `B` 猜的数字 `x` 小于 `352`，说明 `x< 352 ≤ 1000`，应当在 `[x+1,1000]` 中继续猜；
+ 如果玩家 `B` 猜的数字 `x` 大于 `352`，说明 `1 ≤ 352 < x`,应当在 `[1, x-1]` 中继续猜。
+ 显然，每次**选择当前范围的中间数字**去猜，就能尽可能快地逼近正确的数字，并最终将其猜出来。
+ 该游戏的背后是一个**经典**的问题：
+ 如何在一个严格递增序列 `A` 找出给定的数 `x`。
+ 最**直接**的办法是：线性扫描序列中的所有元素，如果当前元素恰好为 `x`，则表明查找成功；如果扫描完整个序列都没有发现给定的数 `x`，则表明查找失败，说明序列中不存在数 `x`。这种**顺序查找**的时间复杂度为 $O(n)$ (其中 `n` 为序列元素个数)，如果查询的次数不多，则是很好的选择，但是如果有 $10^5$ 个数需要查询，将不太能承受。
+ 更好的办法是使用**二分查找**：二分查找是基于有序序列的查找算法（以下以**严格递增**序列为例），该算法一开始令 `[left, right]` 为整个序列的下标区间，然后每次测试当前 `[left, right]` 的中间位置 `mid=(left+right)/2`，判断 `A[mid]` 与欲查询的元素 `x` 的大小：
1. 如果 `A[mid]==x`，说明查找成功，退出查询。

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20231202141728.png)
2. 如果 `A[mid]>x`，说明元素 x 在 `mid` 位置的左边，因此往左子区间 `[left, mid-1]` 继续查找。

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20231202142141.png)
3. 如果 `A[mid]<x`，说明元素 x 在 `mid` 位置的右边，因此往右子区间 `[mid+1, right]` 继续查找。

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20231202142301.png)
+ 二分查找的高效之处在于，每一步都可以去除当前区间的一半元素，因此时间复杂度是 $O(logn)$，这是十分优秀的。
+ 为了更好地解释二分查找地流程，举一个例子来模拟二分查找地过程：
+ 现在需要从序列 `A={3,7,8,11,15,21,33,52,66,88}` 中查询数字 11 和 34 的位置，其中序列下标从 1 到 10。
+ 首先是 `11` 的查询过程，令 `left=1`、`right=10`，表示当前查询的下标范围：
1. `[left, right]=[1,10]`，因此下标中点 `mid=(left+right)/2=5`。由于 `A[mid]=A[5]=15`，而 `15>11`，说明需要在 `[left, mid-1]` 继续查找，因此令 `right=mid-1=4`。

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20231202143556.png)
2. `[left, right]=[1,4]`，因此下标中点 `mid=(left+right)/2=2`。由于 `A[mid]=A[2]=7`，而 `7<11`，说明需要在 `[mid+1, right]` 继续查找，因此令 `left=mid+1=3`。 

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20231202143824.png)
3. `[left, right]=[3,4]`，因此下标中点 `mid=(left+right)/2=3`。由于 `A[mid]=A[3]=8`，而 `8<11`，说明需要在 `[mid+1, right]` 继续查找，因此令 `left=mid+1=4`。 

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20231202144010.png)
4. `[left, right]=[4,4]`，因此下标中点 `mid=(left+right)/2=4`。由于 `A[mid]=A[4]=11`，而 `11==11`，说明找到了欲查询的数字，因此结束算法，返回下标 4。

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20231202144206.png)
+ 接下来是 `34` 的查询过程，同样令 `left=1`、`right=10`：
1. `[left, right]=[1,10]`，因此下标中点 `mid=(left+right)/2=5`。由于 `A[mid]=A[5]=15`，而 `15<34`，说明需要在 `[mid+1, right]` 继续查找，因此令 `left=mid+1=6`。

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20231202144543.png)
2. `[left, right]=[6,10]`，因此下标中点 `mid=(left+right)/2=8`。由于 `A[mid]=A[8]=52`，而 `52>34`，说明需要在 `[left, mid-1]` 继续查找，因此令 `right=mid-1=7`。

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20231202144805.png)
3. `[left, right]=[6,7]`，因此下标中点 `mid=(left+right)/2=6`。由于 `A[mid]=A[6]=21`，而 `21<34`，说明需要在 `[mid+1, right]` 继续查找，因此令 `left=mid+1=7`。

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20231202145014.png)
4. `[left, right]=[7,7]`，因此下标中点 `mid=(left+right)/2=7`。由于 `A[mid]=A[7]=33`，而 `33<34`，说明需要在 `[mid+1, right]` 继续查找，因此令 `left=mid+1=8`。

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20231202145422.png)
5. `[left, right]=[8,7]`，由于 `left>right`，因此查找失败，说明序列不存在 `34`。

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20231202145614.png)
+ 需要注意的是，二分查找的过程与序列的下标从 `0` 开始还是从 `1` 开始无关，整个过程是相同的。
+ 根据上述基础，给出在**严格递增序列**中查找给定的数 `x` 的代码：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <stack>
#include <string>
#include <iostream>
#include <utility>
#include <map>
#include <algorithm>
#include <vector>
using namespace std;

//二分查找函数
//A[]为严格递增序列，left为二分下界，right为二分上界，x为欲查询数
//二分区间为左闭右闭[left,right]，传入初值为[0,n-1]
int binarySearch(int A[],int left,int right,int x)
{
    int mid; //mid为left和right的中点
    while(left <= right) //如果left>right就没办法形成闭区间
    {
        mid = (left+right)/2; //取中点
        if(A[mid]==x) 
            return mid;//找到x，返回下标
        else if(A[mid]>x)//中间数大于查询数
            right = mid -1;//往左子区间查找
        else
            left = mid + 1;//往右子区间查找
    }
    return -1;//查找失败，返回-1
}

//主函数
int main()
{
    const int n = 10;
    int A[n] = {1,3,4,6,7,8,10,11,12,15};
    printf("%d %d\n",binarySearch(A,0,n-1,6),binarySearch(A,0,n-1,9));
    system("pause");// 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
+ 输出：
```text
3 -1
```
+ 如果是递减序列，只需要把上述代码中 `A[mid]>x` 改为 `A[mid]<x` 即可。
+ 需要注意的是，如果二分上界超过 `int` 型数据类型范围的一半，那么当欲查询元素在序列较靠后位置时，语句 `mid = (left+right)/2;` 中的 `left+right` 超出 `int` 型数据类型范围而导致溢出，此时一般使用 `mid = left+(right-left)/2;` 这条等价语句作为代替以避免溢出。
+ 另外，二分法可以使用递归进行实现，但是在程序设计时**更多采用的是非递归的写法。**
+ **接下来探讨更进一步的问题：**
+ 如果递增序列 `A` 中的元素可能重复，那么如何对给定的欲查询元素 `x`，求出序列中第一个大于等于 `x` 的元素位置 `L` 以及第一个大于 `x` 的元素的位置 `R`,这样元素 `x` 在序列中存在的区间就是左闭右开区间 `[L,R)`。
+ 例如对下标从 `0` 开始，有 `5` 个元素的序列 `{1,3,3,3,6}` 来说，如果要查询 `3`，则应当得到 `L=1、R=4`；
+ 如果要查询 `5`，则应当得到 `L=R=4`；
+ 如果要查询 `6`，则应当得到 `L=4、R=5`；
+ 如果要查询 `8`，则应当得到 `L=R=5`；
+ 显然，如果序列中没有 `x`，那么 `L` 和 `R` 也可以理解为假设序列中存在 `x`，则 `x` 应当在的位置。
+ 首先考虑第一个小问：**求序列中第一个大于等于 x 的元素位置。**
+ 做法与之前的问题类似，假设当前区间为左闭右闭区间 `[left,right]`，那么可以根据 `mid` 位置处的元素与欲查询元素 `x` 的大小来判断应当往哪个子区间继续查找；
1. 如果 `A[mid]≥x`，说明第一个大于等于 `x` 的元素位置一定在 `mid` 处或 `mid` 的左侧，应该往左子区间 `[left,mid]` 继续查询，即令 `right=mid`。

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20231203123942.png)
2. 如果 `A[mid]<x`，说明第一个大于等于 `x` 的元素位置一定在 `mid` 的右侧，应该往右子区间 `[mid+1,right]` 继续查询，即令 `left=mid+1`。

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20231203124543.png)
+ 由此可以写出对应代码：

