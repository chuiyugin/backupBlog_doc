---
title: 算法笔记之提高篇
tags:
  - 数据结构
  - 算法
categories:
  - 数据结构
date: 2024-9-6 10:30:00
excerpt: 算法笔记的学习与分享总结!
---
# 算法笔记之提高篇
## 数据结构专题（1）
### 栈的应用
+ 栈（`stack`）是一种**后进先出**的数据结构。

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202409061059275.png)

+ 栈顶指针是始终指向栈的最上方元素的一个标记。
	+ 当使用数组实现栈时，栈顶指针是一个 `int` 型变量（数组下标从 `0` 开始），通常记为 `TOP`；
	+ 而当使用链表实现栈时，则是一个 `int*` 型的指针。
+ 在图 7-1 中，从左到右每个操作之后，栈顶指针 `TOP` 的值分别为 `0`、`1`、`2`、`1`、`0`、`1`，而当栈中没有元素（即栈空）时令 `TOP` 为 `-1`。
+ 使用数组 `st[]` 来实现栈，`int` 型变量 `TOP` 表示栈顶元素的下标，这样栈空时 `TOP` 就是 `-1`，下面是常见操作的示范：

  1.  清空（`clear`）：栈的清空操作将栈顶指针 `TOP` 置为 `-1`，表示栈中没有元素。
```cpp
void clear()
{
	TOP = -1;
}
```
  2. 获取栈内元素个数（`size`）：由于栈顶指针 `TOP` 始终指向栈顶元素，而数组下标从 `0` 开始，因此栈内元素的数为 `TOP+1`。
```cpp
int size()
{
	return TOP+1;
}
```
  3. 判空（`empty`）：由栈顶指针 `TOP` 的定义可知，仅当 `TOP==-1` 时为栈空，返回 `true`，否则，返回 `false`。
  ```cpp
bool empty()
{
	if(TOP==-1)
		return true;
	else
		return false;
}
```
  4. 进栈（`push`）：`push(x)` 操作将元素 `x` 置于栈顶。由于栈顶指针 `TOP` 指向栈顶元素，因此需要先把 `TOP+1`，然后再把元素 `x` 存入 `TOP` 指向的位置。
```cpp
void push(int x)
{
	st[++TOP]=x;
}
```
  5. 出栈（`pop`）：`pop()` 操作将栈顶元素出栈，事实上可以直接将栈顶元素指针 `TOP-1` 来实现这个效果。
```cpp
void pop()
{
	TOP--;
}
```
  6. 取栈顶元素（`top`）：由于栈顶指针 `TOP` 始终指向栈顶元素，因此可以使得 `st[TOP]` 即为栈顶元素。
```cpp
int top()
{
	return st[TOP];
}
```
+ 需要特别注意的是，出栈操作和取栈顶元素操作必须在栈非空的情形下才能使用，因此在使用 `pop()` 函数和 `top()` 函数之前必须先使用 `empty()` 函数来判断栈是否为空。
+ 事实上，可以使用 C++ 的 STL 中的 stack 容器来非常容易地使用栈。STL 中的 stack 为实现好了栈的常用操作，当需要使用时只需要调用函数即可。
+ 需要指出的是，STL 中没有没有实现栈的清空，所以如果需要实现栈的清空，可以使用一个 `while` 循环反复 `pop` 出元素直至栈空：
```cpp
while(!st.empty())
{
	st.pop();
}
```
* 而事实上，更常用的方法是重新定义一个栈以变相实现栈的清空，因为这并不需要花费很多时间，STL 的 stack 进行定义的时间复杂度是 $O(1)$。
例题：**简单计算器**

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202409071557509.png)

+ **思路：**
+ 题目给出的是中缀表达式，所以要计算它的值主要是两个步骤：
	1. 中缀表达式转后缀表达式；
	2. 计算后缀表达式。
+ 下面分别介绍这两个步骤：
+ **步骤 1：中缀表达式转后缀表达式：**
1. 设立一个操作符，用以临时存放操作符；设立一个数组或者队列，用以存放后缀表达式。
2. 从左到右扫描中缀表达式，如果碰到操作数，就把操作数加入后缀表达式中。
3. 如果碰到操作符 `op`，就将其优先级与操作符栈的栈顶操作符优先级比较。
	1. 若 `op` 的优先级高于栈顶操作符的优先级，则压入操作符栈。
	2. 若 `op` 的优先级低于或等于栈顶操作符的优先级，则将操作符栈的操作符不断弹出到后缀表达式中，直到 `op` 的优先级高于栈顶操作符的优先级。
4. 重复上述操作，直到中缀表达式扫描完毕，之后若操作符栈中仍有元素，则将它们一次弹出至后缀表达式中。
+ 所谓的操作符优先级即它们的计算优先级，其中 `乘法==除法>加法==减法`，在具体实现上可以用 `map` 建立操作符和优先级的映射，优先级可以用数字表示，例如乘法和除法优先级为 1，加法和减法优先级为 0。
+ 本题没有括号，但是如果出现括号，处理方法也很简单，如果遇到是左括号 `'('`，就压入操作符栈；如果是右括号 `')'`，就把操作符栈里的元素不断弹出到后缀表达式中直至碰到左括号 `'('`。
+ **步骤 2：计算后缀表达式：**
1. 从左到右扫描后缀表达式，如果是操作数，就压入栈；如果是操作符，就连续弹出两个操作数（**注意：先入栈的先算**），然后进行操作符的计算，生成的新操作数压入栈中。反复上述操作直至后缀表达式扫描完毕，这时栈中只会存在一个数，就是最终答案。
	+ 注意除法可能导致浮点数，因此操作数类型要设成浮点型；
	+ 题目中说明是合法表达式，因此上述操作一定成功，但是如果题目表明可能出现非法表达式，那就要注意每一步使用的对象是否合法。
+ **注意点：**
	1. 用 `string` 的 `erase` 方法可以直接把表达式中的空格去掉。
+ 代码（使用队列 `queue` 来存放后缀表达式）：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <stack>
#include <cstring>
#include <iostream>
#include <utility>
#include <map>
#include <algorithm>
#include <vector>
#include <climits>
#include <string>
#include <ctime>
#include <cmath>
#include <sstream>
#include <queue>
using namespace std;

struct node
{
    double num; //操作数
    char op;    //操作符
    bool flag;  // true表达操作数，false表示操作符
};

string str;
stack<node> s; //操作符栈
queue<node> q; //后缀表达式序列
map<char, int> op;

//将中缀表达式转换为后缀表达式
void change()
{
    double num;
    node temp;
    for (int i = 0; i < str.length();)
    {
        if (str[i] >= '0' && str[i] <= '9') //如果是数字
        {
            temp.flag = true;                                          //标记为数字
            temp.num = str[i++] - '0';                                 //记录这个操作数的第一个数位
            while (i < str.length() && str[i] >= '0' && str[i] <= '9') //可能大于9的数据
            {
                temp.num = temp.num * 10 + (str[i] - '0'); //更新这个操作数
                i++;
            }
            q.push(temp); //将这个操作数压入后缀表达式队列
        }
        else //如果是操作符
        {
            temp.flag = false;
            //只要操作符栈的栈顶元素比该操作符优先级高
            //就把操作符栈的栈顶元素弹出到后缀表达式的队列中
            while (!s.empty() && op[str[i]] <= op[s.top().op])
            {
                q.push(s.top());
                s.pop();
            }
            temp.op = str[i];
            s.push(temp); //把该操作符压入栈中
            i++;
        }
    }
    //如果操作符栈中还有操作符，就把它弹出到后缀表达式中
    while (!s.empty())
    {
        q.push(s.top());
        s.pop();
    }
}

//计算后缀表达式
double cal()
{
    double temp1, temp2;
    node cur, temp;
    while (!q.empty()) //后缀表达式队列非空
    {
        cur = q.front(); //记录队列头
        q.pop();
        if (cur.flag == true) //如果是操作数
        {
            s.push(cur); //直接入栈
        }
        else //如果是操作符
        {
            temp1 = s.top().num;
            s.pop();
            temp2 = s.top().num;
            s.pop();
            temp.flag = true; //记录运算后的数据
            if (cur.op == '+')
                temp.num = temp2 + temp1;
            else if (cur.op == '-')
                temp.num = temp2 - temp1;
            else if (cur.op == '*')
                temp.num = temp2 * temp1;
            else
                temp.num = temp2 / temp1;
            s.push(temp); //把计算后的数据压栈
        }
    }
    return s.top().num; //栈顶元素就是后缀表达式运算后的值
}

//主函数
int main()
{
    //设定操作符优先级
    op['+'] = op['-'] = 1;
    op['*'] = op['/'] = 2;
    while (getline(cin, str), str != "0")
    {
        for (string::iterator it = str.end(); it != str.begin(); it--)
        {
            if (*it == ' ')
                str.erase(it); //把表达式的空格全部去掉
        }
        while (!s.empty()) //初始化栈
            s.pop();
        change();                //将中缀表达式转换成后缀表达式
        printf("%.2f\n", cal()); //输出计算后缀表达式的结果
    }
    system("pause"); // 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
