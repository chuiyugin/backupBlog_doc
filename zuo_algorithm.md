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

### 题目二
+ 移除链表元素：[707.设计链表](https://leetcode.cn/problems/design-linked-list/description/)
+ 代码：

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

 //解决方案
 class MyLinkedList {
    public:

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

        ListNode* _VitHead;
        int _size = -1;
    
        MyLinkedList() {
            _VitHead = new ListNode(-1);
            _size = -1;
        }
        
        int get(int index) {
            if((_size < 0) || (index > _size))
            {
                return -1;
            }
            else
            {
                ListNode* now = _VitHead;
                for(int i=0;i<=index;i++)
                {
                    now = now->next;
                }
                return now->val;
            }
        }
        
        void addAtHead(int val) {
            ListNode* temp = new ListNode(val);
            temp->next = _VitHead->next;
            _VitHead->next = temp;
            _size++;
        }
        
        void addAtTail(int val) {
            ListNode* temp = new ListNode(val);
            ListNode* now = _VitHead;
            while(now->next!=nullptr)
            {
                now = now->next;
            }
            now->next = temp;
            _size++;
        }
        
        void addAtIndex(int index, int val) {
            ListNode* temp = new ListNode(val);
            ListNode* now = _VitHead;
            ListNode* pre = _VitHead;
            if(index > _size+1)
                return;
            for(int i=0;i<=index;i++)
            {
                
                pre = now;
                now = now->next;
            }
            temp->next = pre->next;
            pre->next = temp;
            _size++;
        }
        
        void deleteAtIndex(int index) {
            ListNode* now = _VitHead;
            ListNode* pre = _VitHead;
            if(index > _size)
                return;
            for(int i=0;i<=index;i++)
            {
                pre = now;
                now = now->next;
            }
            pre->next = now->next;
            delete now;
            _size--;
        }
    };
    
/**
    * Your MyLinkedList object will be instantiated and called as such:
    * MyLinkedList* obj = new MyLinkedList();
    * int param_1 = obj->get(index);
    * obj->addAtHead(val);
    * obj->addAtTail(val);
    * obj->addAtIndex(index,val);
    * obj->deleteAtIndex(index);
*/

int main()
{
    MyLinkedList* myLinkedList = new MyLinkedList();
    myLinkedList->addAtHead(7);
    myLinkedList->addAtHead(2);
    myLinkedList->addAtHead(1);
    myLinkedList->addAtIndex(3, 0);
    myLinkedList->deleteAtIndex(2);
    myLinkedList->addAtHead(6);
    myLinkedList->addAtTail(4);
    cout << myLinkedList->get(4) << endl; 
    
    system("pause"); // 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```

### 题目三
+ 反转链表：[206.反转链表](https://leetcode.cn/problems/reverse-linked-list/description/)
+ 代码：

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
        ListNode* reverseList(ListNode* head) {
            ListNode* now;
            ListNode* pre;
            ListNode* temp;
            pre = head;
            now = head;
            if((now == nullptr) || (now->next == nullptr))
                return head;
            now = pre->next;
            pre->next = nullptr;
            while(now != nullptr)
            {
                temp = now->next;
                now->next = pre;
                pre = now;
                now = temp;
            }
            return pre;
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
    now = s.reverseList(head);

    while(now->next!=nullptr)
    {
        printf("%d\n",now->val);
        now = now->next;
    }
    
    system("pause"); // 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```

### 题目四
+ 两两交换链表中的节点：[24.两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/description/)
+ 代码：普通解法

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
        ListNode* swapPairs(ListNode* head) {
            ListNode* temp = nullptr;
            ListNode* temp1 = nullptr;
            ListNode* now = nullptr;
            int count = 0;
            if((head==nullptr) || (head->next==nullptr))
                return head;
            else if(head->next->next == nullptr)
            {
                temp = head->next;
                head->next = nullptr;
                temp->next = head;
                return temp;
            }
            else
            {
                now = head;
                int count = 0;
                while(now->next!=nullptr && now->next->next!=nullptr)
                {
                    if(count==0)
                    {
                        head = now->next;
                        now->next = now->next->next;
                        head->next = now;
                        count++;
                    }
                    else if(count%2==1)
                    {
                        temp = now->next;
                        temp1 = now->next->next->next;
                        now->next = now->next->next;
                        now->next->next = temp;
                        temp->next = temp1;
                        count++;
                        now = now->next;
                    }
                    else
                    {
                        count++;
                        now = now->next;
                    }
                }
                return head;
            }
        }
    };

int main()
{
    ListNode *head = new ListNode(1);
    ListNode *now = head;
    for(int i=2;i<=7;i++)
    {
        ListNode *temp = new ListNode(i);
        now->next = temp;
        now = now->next;
    }
    now = head;
    
    Solution s;
    now = s.swapPairs(head);

    while(now!=nullptr)
    {
        printf("%d\n",now->val);
        now = now->next;
    }
    
    system("pause"); // 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```

+ 虚拟头节点解法：

```cpp
//虚拟头节点解法
class Solution {
    public:
        ListNode* swapPairs(ListNode* head) {
            ListNode* temp = nullptr;
            ListNode* temp1 = nullptr;
            ListNode* VitHead = new ListNode(-1);
            VitHead->next = head;
            ListNode* now = VitHead;
            while((now->next != nullptr) && (now->next->next != nullptr))
            {
                temp = now->next;
                temp1 = now->next->next->next;
                
                now->next = now->next->next;
                now->next->next = temp;
                temp->next = temp1;

                now = now->next->next;
            }
            return VitHead->next;
        }
    };
```

### 题目五
+ 删除链表的倒数第 `N` 个节点：[19.删除链表的倒数第 N 个节点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/description/)
+ 思路：使用双指针的方法，在 `now` 指针往后移动的同时使得 `pre` 指针与 `now` 指针保持 `N` 的距离，当 `now` 指针遍历到链表末尾后就可以采用 `pre` 指针来删除链表的倒数第 `N` 个节点。
+ 代码：

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
//虚拟头节点解法
class Solution {
    public:
        ListNode* removeNthFromEnd(ListNode* head, int n) {
            
            //创建虚拟头节点
            ListNode* VitHead = new ListNode(-1);
            VitHead->next = head;
            
            ListNode* now = VitHead;
            ListNode* pre = VitHead;
            int count = 0;

            while(now->next != nullptr)
            {
                now = now->next;
                count++;
                if(count > n)
                    pre = pre->next;
            }

            ListNode* temp = pre->next;
            pre->next = temp->next;
            delete temp;
            
            return VitHead->next;
        }
    };

int main()
{
    ListNode *head = new ListNode(1);
    ListNode *now = head;
    for(int i=2;i<=3;i++)
    {
        ListNode *temp = new ListNode(i);
        now->next = temp;
        now = now->next;
    }
    now = head;
    
    Solution s;
    now = s.removeNthFromEnd(head, 1);

    while(now!=nullptr)
    {
        printf("%d\n",now->val);
        now = now->next;
    }
    
    system("pause"); // 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```

### 题目六
+  相交链表：[160.相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/description/)
+ 思路：

![相交链表思路](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250407170603.png)

+ 代码：

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode *p = headA;
        ListNode *q = headB;
        while(q!=p)
        {
            if(p!=nullptr)
                p = p->next;
            else
                p = headB;
            if(q!=nullptr)
                q = q->next;
            else
                q = headA;
        }
        return p;
    }
};
```

### 题目七
+ 环形链表 II：[142.环形链表II](https://leetcode.cn/problems/linked-list-cycle-ii/description/)
+ 思路：[代码随想录-142.环形链表](https://programmercarl.com/0142.%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8II.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
+ 代码：

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* fast_ptr = head;
        ListNode* slow_ptr = head;
        while(fast_ptr != nullptr && fast_ptr->next != nullptr)
        {
            fast_ptr = fast_ptr->next->next;
            slow_ptr = slow_ptr->next;
            if(slow_ptr == fast_ptr)
            {
                ListNode* index1 = head;
                ListNode* index2 = fast_ptr;
                while(index1 != index2)
                {
                    index1 = index1->next;
                    index2 = index2->next;
                }
                return index1;
            }
        }
        return nullptr;
    }
};
```

## 队列和栈
### 题目一
+ 用栈实现队列：[232.用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/description/)
+ 思路：[代码随想录-232.用栈实现队列](https://programmercarl.com/0232.%E7%94%A8%E6%A0%88%E5%AE%9E%E7%8E%B0%E9%98%9F%E5%88%97.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
+ 代码：

```cpp
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <stack>
using namespace std;

class MyQueue {
    public:
        stack<int> In;
        stack<int> Out;
        MyQueue() {}
    
        void push(int x) { In.push(x); }
    
        int pop() {
            if (!Out.empty()) {
                int temp = Out.top();
                Out.pop();
                return temp;
            } else {
                while (!In.empty()) {
                    Out.push(In.top());
                    In.pop();
                }
                return Out.top();
            }
        }
    
        int peek() {
            if (!Out.empty())
                return Out.top();
            else {
                while (!In.empty()) {
                    Out.push(In.top());
                    In.pop();
                }
                return Out.top();
            }
        }
    
        bool empty() {
            if (In.empty() && Out.empty())
                return true;
            else
                return false;
        }
    };
    
    /**
     * Your MyQueue object will be instantiated and called as such:
     * MyQueue* obj = new MyQueue();
     * obj->push(x);
     * int param_2 = obj->pop();
     * int param_3 = obj->peek();
     * bool param_4 = obj->empty();
     */
    
int main()
{
    MyQueue* myQueue = new MyQueue();
    //myQueue->push(1);
    myQueue->push(2);
    printf("%d\n",myQueue->peek());
    myQueue->pop();
    //myQueue->pop();
    printf("%d\n",myQueue->empty());
    //myQueue->peek();

    system("pause"); // 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```

### 题目二
+ 用队列实现栈：[225.用队列实现栈](https://leetcode.cn/problems/implement-stack-using-queues/description/)
+ 思路：采用两个队列，始终保持一个队列为空，当要 `pop` 数据的时候，把非空队列的元素依次输出到另外一个空队列，剩下最后一个元素进行返回和 `pop`。`push` 数据时要向非空队列传入数据，若两个队列都是空的就向任意一个队列传数据即可。
+ 代码：

```cpp
class MyStack {
public:
    queue<int> qu_1;
    queue<int> qu_2;
    MyStack() {}

    void push(int x) 
    {
        if(!qu_1.empty())
            qu_1.push(x);
        else if(!qu_2.empty())
            qu_2.push(x);
        else
            qu_1.push(x);
    }

    int pop() 
    {
        int temp;
        if(!qu_1.empty())
        {
            while(qu_1.size()>1)
            {
                qu_2.push(qu_1.front());
                qu_1.pop();
            }
            temp = qu_1.front();
            qu_1.pop();
            return temp;
        }
        else
        {
            while(qu_2.size()>1)
            {
                qu_1.push(qu_2.front());
                qu_2.pop();
            }
            temp = qu_2.front();
            qu_2.pop();
            return temp;
        }
    }

    int top() 
    {
        if(!qu_1.empty())
            return qu_1.back();
        else
            return qu_2.back();
    }

    bool empty() 
    {
        if(qu_1.empty() && qu_2.empty())
            return true;
        else
            return false;
    }
};

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack* obj = new MyStack();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->top();
 * bool param_4 = obj->empty();
 */
```

## 排序问题
### 归并排序
#### 普通归并排序
+ 递归方法：[912.排序数组](https://leetcode.cn/problems/sort-an-array/description/)
+ 思路：采用二分法和递归的方式实现 $O(NlogN)$ 的归并排序；
+ 代码：

```cpp
class Solution {
public:
    
    void merge(vector<int>& nums,int L,int M,int R)
    {
        vector<int> temp(R-L+1);
        int i = L,j = M+1,k = 0;
        while(i<=M && j<=R)
        {
            temp[k++] = nums[i]<=nums[j]? nums[i++] : nums[j++];
        }
        //要么i越界了,把j剩下的拷贝进去
        while(j<=R)
            temp[k++] = nums[j++];
        //要么j越界了,把i剩下的拷贝进去
        while(i<=M)
            temp[k++] = nums[i++];
        for(int t=0;t<temp.size();t++)
            nums[L+t] = temp[t];//需要注意nums的区间
    }
    
    void process(vector<int>& nums,int L,int R)
    {
        int mid = L + (R - L)/2;
        //递归边界条件
        if(L == R)
            return;
        process(nums,L,mid);
        process(nums,mid+1,R);
        merge(nums,L,mid,R);
    }

    vector<int> sortArray(vector<int>& nums) {
        process(nums,0,nums.size()-1);
        return nums;
    }
};
```

#### 小和问题
+ 问题描述：
+ 在一个数组中，每一个数的左边比当前数小的数累加起来，叫做这个数组的小和。求一个数组的小和。  
	+ 例子：对于数组 `[1,3,4,2,5]` ，
		+ `1` 左边比 `1` 小的数，没有；
		+ `3` 左边比 `3` 小的数，`1`；
		+ `4` 左边比 `4` 小的数，`1,3`；
		+ `2` 左边比 `2` 小的数，`1`；
		+ `5` 左边比 `5` 小的数，`1,2,3,4`；
		+ 所以小和为 `1+1+3+1+1+2+3+4 = 16` 。
+ 思路：
	+ 此处使用归并排序，在 `merge` 时，由于左右两部分都已经有序，可以确定一侧的数都大于正在比较的数，例如，
	+ 归并 `2 4 5 | 1 3 7` 两个部分时，`2` 比 `3` 小，此时可以确定后面的数都大于 `2`，此时便可以一次性计算小和 `2 * 2`(两个数大于 `2`)，而不用一个个遍历。
+ 代码：

```cpp
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <vector>
using namespace std;

class Solution {
    public:
        
        int merge(vector<int>& nums,int L,int M,int R)
        {
            vector<int> temp(R-L+1);
            int i = L,j = M+1,k = 0;
            int ans = 0;
            while(i<=M && j<=R)
            {
                //左组小于右组数，把小和累加进去
                ans += nums[i]<nums[j] ? (R-j+1)*nums[i] : 0;
                temp[k++] = nums[i]<nums[j] ? nums[i++] : nums[j++];
            }
            //要么i越界了,把j剩下的拷贝进去
            while(j<=R)
                temp[k++] = nums[j++];
            //要么j越界了,把i剩下的拷贝进去
            while(i<=M)
                temp[k++] = nums[i++];
            for(int t=0;t<temp.size();t++)
                nums[L+t] = temp[t];//需要注意nums的区间
            return ans;
        }
        
        int process(vector<int>& nums,int L,int R)
        {
            int mid = L + (R - L)/2;
            int ans = 0;
            //递归边界条件
            if(L == R)
                return 0;
            ans = process(nums,L,mid) + process(nums,mid+1,R) + merge(nums,L,mid,R);
            return ans;
        }
    
        int smallSum(vector<int>& nums) {
            int ans;
            ans = process(nums,0,nums.size()-1);
            return ans;
        }
    };

int main()
{
    Solution mysolution;
    vector<int> vec = {2,4,5,1,7,3};
    int ans;
    ans = mysolution.smallSum(vec);
    printf("%d\n",ans);

    system("pause"); // 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```
  



