---
title: 左程云算法学习
tags:
  - 算法学习
categories:
  - 算法学习
date: 2025-3-7 15:00:00
excerpt: C++、编程语言、算法学习
---
# 左程云算法学习
## 二分法
+ 二分查找法：[704.二分查找](https://leetcode.cn/problems/binary-search/description/)
+ 代码如下：

```cpp
#include <cstdio>
#include <cstdlib>
#include <vector>
using namespace std;

class Solution {
    public:
        int search(vector<int>& nums, int target) {
            int left = 0;
            int right = nums.size()-1;
            int mid = left + (right-left)/2;
            int ans = -1;

            while(left<right)
            {
                mid = left + (right-left)/2;
                if(nums[mid]==target)
                {
                    ans = mid;
                    break;
                }
                else if(nums[mid]>target)
                {
                    right = mid-1;
                }
                else
                {
                    left = mid+1;
                }
            }
            printf("%d\n",ans);
            return ans;
        }
    };

int main()
{
    Solution s;
    vector<int> v;
    for(int i=0;i<5;i++)
    {
        v.push_back(i);
    }
    s.search(v,0);

    system("pause"); // 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```

## 异或运算
### 题目一
+ 如何不用额外变量交换两个数
+ 关键部分：

```cpp
//采用异或运算不用额外变量交换两个数
void yihuo_swap(int& a,int& b)
{
    a = a^b;
    b = a^b;
    a = a^b;
}
```

+ 代码：

```cpp
#include <cstdio>
#include <cstdlib>
#include <iostream>
using namespace std;

//采用异或运算不用额外变量交换两个数
void yihuo_swap(int& a,int& b)
{
    a = a^b;
    b = a^b;
    a = a^b;
}

int main()
{
    int num1 = 105;
    int num2 = 211;
    swap(num1,num2);
    printf("%d,%d",num1,num2);
    system("pause"); // 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```

+ 证明：
	+ 1、异或可以看成不进位二进制位加法；
	+ 2、具有 `a^a=0` 和 `a^0=a` 以及 `a^b=b^a` 的性质。
	+ 各个步骤详细推导如下：

![异或交换运算](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250307160112.png)
### 题目二

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

### 题目三
+ 怎么把一个 `int` 类型的数，提取出最右侧的 `1` 出来。
+ 关键部分：`ans=a&(-a)；`
+ 代码：

```cpp
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <vector>
using namespace std;

//把int类型最右侧的1找出来
int yihuo_last_one(int& a)
{
    return a&(-a);//把int类型最右侧的1找出来
}

int main()
{
    int num = 28; 
    int ans = yihuo_last_one(num);
    printf("%d",ans);
    system("pause"); // 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```

+ 证明：

![题目三](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250307165903.png)

### 题目四
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
```

+ 代码：

```cpp
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <vector>
using namespace std;

//把int类型最右侧的1找出来
int yihuo_last_one(int& a)
{
    return a&(-a);//把int类型最右侧的1找出来
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

### 题目五
+ 一个数组中有一种数出现 `K` 次，其他数都出现了 `M` 次，`M>1`，`K<M`，请找到出现了 `K` 次的数，要求额外空间复杂度 `O(1)`，时间复杂度 `O(N)`。
+ 关键代码：

```cpp
//一个数组中有一种数出现K次，其他数都出现了M次，M>1，K<M，请找到出现了K次的数
int yihuo_find_KM(vector<int>& a,int& k,int& m)
{
    int arr[32];
    fill_n(arr,32,0);//创建全0数组
    for(int i=0;i<a.size();i++)
    {
        for(int j=0;j<32;j++)//一个int型有32位
        {
            arr[j]+=((a[i]>>j) & 1);//将这个数组中的每个位的1都相加上去
        }
    }
    int ans = 0;
    for(int k=0;k<32;k++)//最后判断每个位上对m取余是否等于0
    {
        if(arr[k]%m != 0)//对m取余不等于0，则在第k位上，有1
        {
            ans |= (1<<k);//将第k位的1或上去
        }
    }
    return ans;
}
```

+ 代码：

```cpp
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

//一个数组中有一种数出现K次，其他数都出现了M次，M>1，K<M，请找到出现了K次的数
int yihuo_find_KM(vector<int>& a,int& k,int& m)
{
    int arr[32];
    fill_n(arr,32,0);//创建全0数组
    for(int i=0;i<a.size();i++)
    {
        for(int j=0;j<32;j++)//一个int型有32位
        {
            arr[j]+=((a[i]>>j) & 1);//将这个数组中的每个位的1都相加上去
        }
    }
    int ans = 0;
    for(int k=0;k<32;k++)//最后判断每个位上对m取余是否等于0
    {
        if(arr[k]%m != 0)//对m取余不等于0，则在第k位上，有1
        {
            ans |= (1<<k);//将第k位的1或上去
        }
    }
    return ans;
}

int main()
{
    int ans=0,k=2,m=3;
    vector<int>  vec1= {4, 4, 3, 3, 3, 6, 6, 6}; 
    ans = yihuo_find_KM(vec1,k,m);
    printf("%d\n",ans);
    system("pause"); // 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```

## 链表
### 题目一
+ 移除链表元素：[203.移除链表元素](https://leetcode.cn/problems/remove-linked-list-elements/description/)
+ 直接解法代码:

```cpp
#include <cstdio>
#include <cstdlib>
#include <iostream>
using namespace std;

/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */

//链表结构
struct ListNode 
{
    int val;
    ListNode *next;
    ListNode() : val(0), next(nullptr) {}
    //构造函数
    ListNode(int x) : val(x), next(nullptr) {}
    ListNode(int x, ListNode *next) : val(x), next(next) {}
};

//解决方案1
class Solution {
    public:
        ListNode* removeElements(ListNode* head, int val) {
            ListNode *now = head;
            ListNode *pre = nullptr;//now的前一个
            ListNode *temp = nullptr;
            while(now != nullptr)
            {
                //头节点就是要删除的数据
                if(head->val == val)
                {
                    head = head->next;
                    delete now;
                    now = head;
                }
                //头节点不是要删除的数据
                else if(now->val == val && head->val != val)
                {
                    pre->next = now->next;
                    temp = now;
                    now = now->next;
                    delete temp;
                    temp = nullptr;
                }
                //没有要删除的数据
                else
                {
                    pre = now;
                    now = now->next;
                }
            }
            return head;
        }
    };

int main()
{
    ListNode *head = new ListNode(1);
    ListNode *now = head;
    for(int i=2;i<=8;i++)
    {
        ListNode *temp = new ListNode(i);
        now->next = temp;
        now = now->next;
    }
    now = head;
    
    Solution s;
    now = s.removeElements(head,1);

    while(now->next!=nullptr)
    {
        printf("%d\n",now->val);
        now = now->next;
    }
    
    system("pause"); // 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```

+ 虚拟头节点解法代码：

```cpp
#include <cstdio>
#include <cstdlib>
#include <iostream>
using namespace std;

/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */

//链表结构
struct ListNode 
{
    int val;
    ListNode *next;
    ListNode() : val(0), next(nullptr) {}
    //构造函数
    ListNode(int x) : val(x), next(nullptr) {}
    ListNode(int x, ListNode *next) : val(x), next(next) {}
};

//解决方案
class Solution {
    public:
        //虚拟头节点解法
        ListNode* removeElements(ListNode* head, int val) {
            ListNode* vitHead = new ListNode(-1);
            vitHead->next = head;
            ListNode* now = vitHead;
            while(now->next != nullptr)
            {
                if(now->next->val == val)
                {
                    ListNode* temp = now->next; 
                    now->next = now->next->next;
                    now = now->next;
                    delete temp;
                }
                else
                {
                    now = now->next;
                }
            }
            return vitHead->next;
        }
    };

int main()
{
    ListNode *head = new ListNode(1);
    ListNode *now = head;
    for(int i=2;i<=8;i++)
    {
        ListNode *temp = new ListNode(i);
        now->next = temp;
        now = now->next;
    }
    now = head;
    
    Solution s;
    now = s.removeElements(head,1);

    while(now->next!=nullptr)
    {
        printf("%d\n",now->val);
        now = now->next;
    }
    
    system("pause"); // 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```