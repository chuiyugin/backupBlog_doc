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
            while(left<=right) //注意点1
            {
                mid = left + (right-left)/2; //注意点2
                if(nums[mid]==target)
                {
                    ans = mid;
                    break;
                }
                else if(nums[mid]>target)
                {
                    right = mid-1; //注意点3
                }
                else
                {
                    left = mid+1; //注意点4
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

+ 变式 1：查找第一个和 `target` 相等的元素
+ 代码如下：

```cpp
//查找第一个和target相等的元素
int binary_search2(vector<int>& nums, int target) 
{
    int left = 0;
    int right = nums.size()-1;
    int mid,cmp;
    while(left<=right)
    {
        mid = left + (right - left)/2;
        cmp = target - nums[mid];
        if(cmp < 0) // target < arr[mid]
        {
            right = mid - 1;
        }
        else if(cmp > 0)
        {
            left = mid + 1;
        }
        else 
        {
            // 注意点：在[left,right]的区间找
            if(mid == left || nums[mid-1]<target)
                return mid;
            else
                right = mid - 1;
        }
    }
    return -1;
}
```

+ 变式 2：查找第一个大于等于 `target` 的元素
+ 代码如下：

```cpp
//查找第一个大于等于target的元素
int binary_search3(vector<int>& nums, int target) 
{
    int left = 0;
    int right = nums.size()-1;
    int mid,cmp;
    while(left<=right)
    {
        mid = left + (right - left)/2;
        cmp = target - nums[mid];
        if(cmp <= 0) // 注意点：target <= nums[mid]
        {
            if( mid == left || nums[mid - 1] < target)
                return mid;
            else
                right = mid - 1;
        }
        else
        {
            left = mid + 1;
        }
    }
    return -1;
}
```

+ 变式 3：查找最后一个小于等于 `target` 的值
+ 代码如下：

```cpp
//查找最后一个小于等于target的元素
int binary_search4(vector<int>& nums, int target) 
{
    int left = 0;
    int right = nums.size()-1;
    int mid,cmp;
    while(left<=right)
    {
        mid = left + (right - left)/2;
        cmp = target - nums[mid];
        if(cmp <= 0) // target <= nums[mid]
        {
            right = mid - 1;
        }
        else // 注意点
        {
            if(mid == right || nums[mid + 1]>target)
                return mid;
            else
                left = mid + 1;
        }
    }
    return -1;
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

## 数组
### 题目一
+ 有序数组的平方：[977.有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/description/)
+ 思路：
	+ 能够找到数组 `nums` 中负数与非负数的分界线，那么就可以用类似「**归并排序**」的方法。
	+ 具体地，设 `index` 为数组 `nums` 中负数与非负数的分界线，也就是说，`nums[0]` 到 `nums[index]` 均为负数，而 `nums[index+1]` 到 `nums[len−1]` 均为非负数。当我们将数组 `nums` 中的数平方后，那么 `nums[0]` 到 `nums[index]` 单调递减，`nums[index+1]` 到 `nums[len−1]` 单调递增。
	+ 由于我们得到了两个已经有序的子数组，因此就可以使用归并的方法进行排序了。具体地，使用两个指针分别指向位置 `index` 和 `index+1`，每次比较两个指针对应的数，选择较小的那个放入答案并移动指针。当某一指针移至边界时，将另一指针还未遍历到的数依次放入答案。
+ 代码：

```cpp
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <vector>
#include <cmath>
using namespace std;

class Solution
{
public:
    vector<int> sortedSquares(vector<int> &nums)
    {
        int len = nums.size();
        if (len == 1)
        {
            nums[0] = nums[0] * nums[0];
            return nums;
        }
        int index = -1;
        for (int i = 0; i < len; i++)
        {
            if (nums[i] < 0)
                index = i;
            else
                break;
        }
        int left = index;
        int right = index + 1;
        vector<int> arr;
        while (left >= 0 && right <= len - 1)
        {
            if (nums[left] * nums[left] <= nums[right] * nums[right])
            {
                arr.push_back(nums[left] * nums[left]);
                left--;
            }
            else
            {
                arr.push_back(nums[right] * nums[right]);
                right++;
            }
        }
        while (left >= 0)
        {
            arr.push_back(nums[left] * nums[left]);
            left--;
        }
        while (right <= len - 1)
        {
            arr.push_back(nums[right] * nums[right]);
            right++;
        }
        return arr;
    }
};

int main()
{
    Solution mysolution;
    vector<int> vec = {0, 2};
    vector<int> ans;
    int num;
    ans = mysolution.sortedSquares(vec);
    num = ans.size();
    for (int i = 0; i < num; i++)
    {
        printf("%d\n", ans[i]);
    }

    system("pause"); // 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```

### 题目二
+ 长度最小的子数组：[209.长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/description/)
+ 思路：
	+ 滑动窗口就是满足总和 `sum` 大于等于 `target` 的长度最小的连续子数组。
	+ 滑动窗口的起始位置如何移动：如果当前窗口的总和 `sum` 大于等于 `target` 了，窗口就要向前移动了（也就是该缩小 `i++` 了）。
	+ 滑动窗口的结束位置如何移动：窗口的结束位置就是遍历数组的指针，也就是 `for` 循环里的索引，`j++` 。
	+ 可以发现滑动窗口的精妙之处在于根据当前子序列和大小的情况，不断调节子序列的起始位置。从而将 $O(n^2)$ 暴力解法降为 $O(n)$ 。
+ 代码：

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int len = nums.size(); //滑动窗口的长度
        int sum = 0; // 滑动窗口数值之和
        int i = 0; //滑动窗口的起始位置
        //j表示移动窗口结束的位置
        for(int j=0;j<nums.size();j++)
        {
            sum+=nums[j];
            while(sum>=target) //直到滑动窗口中的数值之和小于target
            {
                sum-=nums[i];
                len = min(len,j-i+1);//取长度中的最小值
                i++;
            }
        }
        if(i==0 && sum<target)
            return 0;
        else
            return len;
    }
};
```

### 题目三
+ 螺旋矩阵 II：[59.螺旋矩阵 II](https://leetcode.cn/problems/spiral-matrix-ii/description/)
+ 思路：
	+ 这道题就是模拟螺旋矩阵生成的过程，分为四个方向进行循环，同时设定相应的循环边界条件达到模拟的实现。
	+ 注意在 C++ 代码中 `vector` 的二维动态数组可以定义为：`vector<vector<int>> mat(n,vector<int>(n));`
+ 代码：

```cpp
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <vector>
#include <cmath>
using namespace std;

class Solution {
    public:
        vector<vector<int>> generateMatrix(int n) {
            //创建二维数组和下标
            vector<vector<int>> mat(n,vector<int>(n));
            int i=0;
            int j=0;
            //模拟部分
            int count = 1;
            int step = n;
            int num = 1;
            while(step>0)
            {
                //向右横着
                for(int k=0;k<step;k++)
                {
                    //printf("i=%d j=%d num = %d\n",i,j,num);
                    mat[i][j++] = num++;
                }
                count++;
                if(count%2 == 0)
                    step--;
                i++;
                j--;
                //向下竖着
                for(int k=0;k<step;k++)
                {
                    //printf("i=%d j=%d num = %d\n",i,j,num);
                    mat[i++][j] = num++;
                }
                count++;
                if(count%2 == 0)
                    step--;
                j--;
                i--;
                //向左横着
                for(int k=0;k<step;k++)
                {
                    //printf("i=%d j=%d num = %d\n",i,j,num);
                    mat[i][j--] = num++;
                }
                count++;
                if(count%2 == 0)
                    step--;
                i--;
                j++;
                //向上竖着
                for(int k=0;k<step;k++)
                {
                    //printf("i=%d j=%d num = %d\n",i,j,num);
                    mat[i--][j] = num++;
                }
                count++;
                if(count%2 == 0)
                    step--;
                j++;
                i++;
            }
            return mat;
        }
    };

int main()
{
    Solution mysolution;
    vector<vector<int>> ans;
    int num = 4;
    ans = mysolution.generateMatrix(num);
    for (int i = 0; i < num; i++)
    {
        for(int j = 0;j < num; j++)
        {
            printf("%d\n", ans[i][j]);
        }
    }

    system("pause"); // 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```

### 题目四
+ 区间和：[58. 区间和（第九期模拟笔试）](https://kamacoder.com/problempage.php?pid=1070)
	+ 本题主要用于熟悉 ACM 模式的输入输出！
+ 代码如下：

```cpp
#include <cstdio>
#include <algorithm>
#include <vector>

using namespace std;

int main(void)
{
    int n,num,sum;
    int pre,last;
    scanf("%d",&n);
    vector<int> vec;
    for(int i=0;i<n;i++)
    {
        scanf("%d",&num);
        vec.push_back(num);
    }
    //循环输入输出部分
    while(scanf("%d %d",&pre,&last)!=EOF)
    {
        sum = 0;
        for(int i=pre;i<=last;i++)
        {
            sum+=vec[i];
        }
        printf("%d\n",sum);
    }
    return 0;
}
```

### 题目五
+ 开发商购买土地：[44. 开发商购买土地（第五期模拟笔试）](https://kamacoder.com/problempage.php?pid=1044)
+ 思路：
	+ 前缀和计算
		+ **横向前缀和**：计算每一行从左到右的累加和，存储在 `heng` 数组中；
		+ **竖向前缀和**：计算每一列从上到下的累加和，存储在 `shu` 数组中。
	+ 寻找最小差值：
		+ **竖向分割**：尝试所有可能的竖向分割线位置，计算左右两部分和的差值，差值计算公式为 `shu[m-1] - 2*shu[i]`（总列和减去两倍左侧和）；
		+ **横向分割**：尝试所有可能的横向分割线位置，计算上下两部分和的差值，差值计算公式为 `heng[n-1] - 2*heng[i]`（总行和减去两倍上方和）；
		+ 记录所有可能分割中的最小绝对差值。
+ 代码：

```cpp
#include <cstdio>
#include <vector>
#include <limits>
#include <cmath>

using namespace std;

int main(void)
{
    int n,m;
    scanf("%d %d",&n,&m);
    //创建二维数组
    vector<vector<int>> mat(n,vector<int>(m));
    //创建竖向前缀和数组和变量
    vector<int> shu(m);
    int sum_shu = 0;
    //创建横向前缀和数组和变量
    vector<int> heng(n);
    int sum_heng = 0;
    for(int i=0;i<n;i++)
    {
        for(int j=0;j<m;j++)
        {
            scanf("%d",&mat[i][j]);
            sum_heng += mat[i][j];
            heng[i] = sum_heng;
        }
    }
    for(int i=0;i<m;i++)
    {
        for(int j=0;j<n;j++)
        {
            sum_shu += mat[j][i];
            shu[i] = sum_shu;
        }
    }
    //定义最小值
    int min_num = numeric_limits<int>::max();
    //比较竖向分割
    int num = 0;
    for(int i=0;i<m;i++)
    {
        num = shu[m-1]-2*shu[i];
        min_num = min(min_num,abs(num));
    }
    //比较横向分割
    for(int i=0;i<n;i++)
    {
        num = heng[n-1]-2*heng[i];
        min_num = min(min_num,abs(num));
    }

    printf("%d",min_num);

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

## 字符串
### 题目一
+ 替换数字：[54. 替换数字（第八期模拟笔试）](https://kamacoder.com/problempage.php?pid=1064)
+ 思路：首先扩充数组到每个数字字符替换成 `number` 之后的大小，再从后向前进行填充。**其实很多数组填充类的问题，其做法都是先预先给数组扩容带填充后的大小，然后在从后向前进行操作。**
	+ 好处如下：
		1. 不用申请新数组。
		2. 从后向前填充元素，避免了从前向后填充元素时，每次添加元素都要将添加元素之后的所有元素向后移动的问题。
+ 代码：

```cpp
#include <cstdio>
#include <string>
#include <iostream>

using namespace std;

int main(void)
{
    string s;
    cin >> s;
    int count = 0;
    int num = s.size();
    for(int i=0; i<num; i++)
    {
        if(s[i]>='0' && s[i]<='9')
        {
            count++;
        }
    }
    //扩容
    s.resize(s.size() + count * 5);
    //从后往前调整字符串数组
    int j = s.size()-1;
    for(int i=num-1; i>=0; i--)
    {
        if(!(s[i]>='0' && s[i]<='9'))
        {
            s[j] = s[i];
            j--;
        }
        else
        {
            s[j--] = 'r';
            s[j--] = 'e';
            s[j--] = 'b';
            s[j--] = 'm';
            s[j--] = 'u';
            s[j--] = 'n';
        }
    }
    cout << s;
    return 0;
}
```

### 题目二
+ 反转字符串中的单词：[151.反转字符串中的单词](https://leetcode.cn/problems/reverse-words-in-a-string/description/)
+ 思路：
	+ 移除单词中多余的空格->整体反转字符串->逐个单词反转
	+ 这道题目需要注意的是自己写 `string deleteExtraSpaces(string s)` 函数代码来移除单词中多余的空格。
+ 代码：

```cpp
class Solution {
public:
    //删除单词之间多余的空格
    string deleteExtraSpaces(string s)
    {
        int slow = 0;
        for(int fast=0; fast<s.size(); fast++)
        {
            //不等于空格就处理
            if(s[fast]!=' ')
            {
                //补上空格
                if(slow!=0)
                    s[slow++] = ' ';
                //把单词填上去
                while(s[fast]!=' ' && fast<s.size())
                    s[slow++] = s[fast++];
            }
        }
        s.resize(slow);
        return s;
    }

    string reverseWords(string s) {
        s = deleteExtraSpaces(s);
        //将字符串整体翻转
        int i=0;
        int j=s.size()-1;
        while(i<j)
        {
            swap(s[i],s[j]);
            i++;
            j--;
        }
        //逐个将单词翻转
        int m,n,record=0;
        for(int i=0; i<=s.size(); i++)
        {
            if(s[i]==' ' || i==s.size())
            {
                m = record;
                record = i+1;
                n = i-1;
                while(m<n)
                {
                    swap(s[m],s[n]);
                    m++;
                    n--;
                }
            }
        }
        return s;
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

### 题目三
+ 删除字符串中的所有相邻重复项：[1047. 删除字符串中的所有相邻重复项](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/description/)
+ 思路：
	1. **初始化一个空栈**：用于存储字符。
	2. **遍历字符串**：
		+ 如果栈为空，或者当前字符与栈顶字符不同，则将当前字符压入栈。 
		+ 如果当前字符与栈顶字符相同，则弹出栈顶字符（表示删除这对相邻重复字符）。
	3. **构建结果字符串**：
		+ 遍历结束后，栈中剩下的字符就是删除所有相邻重复字符后的结果。
		+ 由于栈是后进先出的，我们需要将栈中的字符**逆序**才能得到正确的顺序。
		+ 字符串逆序采用 `reverse(res.begin(),res.end());` 函数，需要引入 `#include <algorithm>` 头文件，该函数可以用于翻转数组、字符串和向量（`vector`）。
+ 代码：

```cpp
class Solution {
public:
    string removeDuplicates(string s) {
        stack<char> stk;
        for(int i=0;i<s.size();i++)
        {
            if(stk.empty() || s[i] != stk.top())
                stk.push(s[i]);
            else
                stk.pop();
        }
        string res = "";
        while(!stk.empty())
        {
            res += stk.top();
            stk.pop();
        }
        reverse(res.begin(),res.end());//此时需要反转最终的字符串
        return res;
    }
};
```

### 题目四
+ 逆波兰表达式求值：[150. 逆波兰表达式求值](https://leetcode.cn/problems/evaluate-reverse-polish-notation/description/)
+ 注意点：将 `string` 变量转换为 `int` 型、`long` 型、`long long` 型变量可以使用 `std::stoi`、`std::stol` 和 `std::stoll` 函数！
+ 代码：

```cpp
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<long long> stk;
        long long num1,num2;
        for(int i=0;i<tokens.size();i++)
        {
            if(tokens[i]=="+" || tokens[i]=="-" || 
               tokens[i]=="*" || tokens[i]=="/")
            {
                num1 = stk.top();
                stk.pop();
                num2 = stk.top();
                stk.pop();
                switch(tokens[i][0])
                {
                    case '+':
                        stk.push(num2 + num1);
                    break;
                    case '-':
                        stk.push(num2 - num1);
                    break;
                    case '*':
                        stk.push(num2 * num1);
                    break;
                    case '/':
                        stk.push(num2 / num1);
                    break;
                }
            }
            else
                stk.push(stoi(tokens[i]));
        }
        return stk.top();;
    }
};
```

## 排序问题
### 选择排序
#### 普通选择排序
+ 选择排序理论：

![选择排序理论](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250510160136.png)

+ 代码：

```cpp
// 选择排序
void choose_sort(vector<int>& nums)
{
    if(nums.size()==0 || nums.size()==1)
        return;
    int minIndex = 0;
    for(int i=0; i<nums.size()-1; i++)
    {
        minIndex = i;
        for(int j=i+1; j<nums.size(); j++)
        {
            if(nums[j]<nums[minIndex])
                minIndex = j;
        }
        swap(nums[i],nums[minIndex]);
    }
}
```

+ 分析：

![选择排序的分析](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250510210721.png)


### 冒泡排序
#### 普通冒泡排序
+ 冒泡排序理论：

![冒泡排序理论](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250510161815.png)


+ 代码：

```cpp
// 冒泡排序
void bubble_sort(vector<int>& nums)
{
    if(nums.size()==0 || nums.size()==1)
        return;
    int last = nums.size()-1;
    int flag = 0; // 注意点：当不再发生交换的时候就已经有序了
    for(int i=0; i<nums.size(); i++)
    {
        flag = 0;
        for(int j=0; j<last; j++)
        {
            if(nums[j]>nums[j+1])
            {
                swap(nums[j],nums[j+1]);
                flag++;
            }
        }
        if(flag==0)
            break;
    }
}
```

+ 分析：

![冒泡排序的分析](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250510210853.png)


### 插入排序
#### 普通插入排序
+ 插入排序理论：

![插入排序理论](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250510164741.png)

+ 代码：

```cpp
// 插入排序
void insert_sort(vector<int>& nums)
{
    if(nums.size()==0 || nums.size()==1)
        return;
    int value;
    for(int i=1; i<nums.size(); i++) // 要插入元素的索引
    {
        int value = nums[i];
        int j = i - 1;
        while(value < nums[j] && j>=0)
        {
            nums[j+1] = nums[j];
            j--;
        }
        nums[j+1] = value;
    }
}
```

+ 性质和分析：
	+ 在原数组有序的最好情况下，算法时间复杂度是 $O(n)$！
	+ 空间复杂度为 $O(1)$！
	+ 插入排序算法具有**稳定性**（假定在待排序的记录序列中，存在多个具有相同的关键字的记录，若经过排序后，这些记录的相对次序保持不变）

![插入排序分析](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250510165134.png)

+ 使用场景：
	+ 数组长度比较小；
	+ 当数组基本有序（离排好序的最终位置很近），插入排序可以在 $O(n)$ 时间内完成排序！

### 希尔排序
#### 普通希尔排序
+ 希尔排序理论：

![希尔排序理论](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202505101858190.png)

+ 代码：

```cpp
// 希尔排序
void shell_sort(vector<int> &nums)
{
    int gap = nums.size() / 2;
    while (gap)
    {
        for (int i = gap; i < nums.size(); i++)
        {
            int value = nums[i];
            int j = i - gap;
            // 步骤与从插入排序类似
            while (j >= 0 && nums[j] > value)
            {
                nums[j + gap] = nums[j];
                j -= gap;
            }
            nums[j + gap] = value;
        }
        // 缩小gap
        gap /= 2;
    }
}
```

+ 分析：
	+ 时间复杂度和 `gap` 序列相关，一般而言平均情况小于 $O(n^2)$；
	+ 空间复杂度为 $O(1)$；
	+ 插入排序算法不具有**稳定性**，因为发生了长距离交换，通过牺牲不稳定性来换取时间。

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

+ 分析：
	+ 归并排序算法是稳定的，但是空间复杂度比较高；

![归并排序的分析](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250510211156.png)

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
	+ 归并 `2 4 5 | 1 3 7` 两个部分时，`2` 比 `3` 小，此时可以确定后面的数都大于 `2`，此时便可以一次性计算小和 `2 * 2` (两个数大于 `2`)，而不用一个个遍历。
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
  
#### 逆序对问题
+ 交易逆序对总数：[LCR 170 交易逆序对总数](https://leetcode.cn/problems/sort-an-array/description/)
+ 思路：参考归并排序的算法，区别在于在 `merge` 的时候从右到左进行比较，
	+ 左组数比右组数大时，通过 `ans+=j-(M+1)+1;` 计算逆序对的个数，并将左组数拷贝到 `temp` 数组并左移一位；
	+ 右组数比左组数大时，无需计算逆序对个数，只需要将右组数拷贝到 `temp` 数组并左移一位即可。
	+ 注意递归的边界条件！
+ 代码：

```cpp
#include <cstdio>
#include <cstdlib>
#include <iostream>
#include <vector>
using namespace std;

class Solution {
    public:
        
        int merge(vector<int>& record,int L,int M,int R)
        {
            int ans = 0;
            int i = M;
            int j = R;
            vector<int> temp(R-L+1);
            int k=R-L;
            while(i>=L && j >= M+1)
            {
                if(record[i]>record[j])
                {
                    ans+=j-(M+1)+1;
                    temp[k--] = record[i--];
                }
                else
                {
                    temp[k--] = record[j--];
                }
            }
            while(i>=L)
            {
                temp[k--] = record[i--];
            }
            while(j >= M+1)
            {
                temp[k--] = record[j--];
            }
            for(int t=0;t<temp.size();t++)
                record[L+t] = temp[t];
            return ans;
        }
    
        int process(vector<int>& record,int L,int R)
        {
            
            int mid = L + (R-L)/2;
            int ans = 0;
            //递归边界条件
            if(R==L)
                return 0;
            ans = process(record,L,mid) 
                    + process(record,mid+1,R) 
                    + merge(record,L,mid,R);
            return ans;
        }
    
        int reversePairs(vector<int>& record) {
            int ans = 0;
            if(record.size()==0)
                return 0;
            else
            {
                ans = process(record,0,record.size()-1);
                return ans;
            }
        }
    };

int main()
{
    Solution mysolution;
    vector<int> vec = {9,7,5,4,6};
    int ans;
    ans = mysolution.reversePairs(vec);
    printf("%d\n",ans);

    system("pause"); // 防止运行后自动退出，需头文件stdlib.h
    return 0;
}
```

#### 双倍逆序对问题
+ 题目描述：
	+ 给定一个整数数组 `nums`，统计其中满足以下条件的逆序对数量：
	- 条件：存在两个下标 `i` 和 `j`（ `i < j` ），使得 `nums[i] > 2 * nums[j]`。要求设计一个时间复杂度为 $O(NlogN)$ 的算法解决此问题。
	- 示例：
		- 输入：`nums = [8, 3, 5, 1, 2]`  
		- 输出：`6`  
		- 解释：  满足条件的逆序对为：`(8,3)`, `(8,1)`, `(8,2)`, `(3,1)`, `(5,1)`, `(5,2)`，共 `6` 对。
- 思路：
	- 基于**归并排序的分治框架**，在合并两个有序子数组的过程中，利用**双指针滑动窗口**高效统计满足条件的逆序对。具体步骤如下：
	1. **递归划分**：将数组递归分割为子数组，直到子数组长度为 `1`。
	2. **合并与统计**：在合并两个已排序的子数组时，遍历左半部分的每个元素，通过滑动窗口找到右半部分中满足 `nums[i] > 2 * nums[j]` 的元素数量。
	3. **排序保证**：合并完成后，保证当前子数组有序，为后续递归合并提供正确输入。
+ 代码：

```cpp
class Solution {
    public:
        
        int merge(vector<int>& nums,int L,int M,int R)
        {
            vector<int> temp(R-L+1);
            int i = L,j = M+1,k = 0;
            int ans = 0;
            
            //计算左边数比右边数大2倍的数量
            int winR = M+1;
            for(int i=L;i<=M;i++)
            {
                while(winR<=R && nums[i]>(nums[winR]*2))
                    winR++;
                ans += winR - (M+1);
            }

            //排序合并
            i = L,j = M+1,k = 0;
            while(i<=M && j<=R)
            {
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
    
        int Than2(vector<int>& nums) {
            int ans;
            ans = process(nums,0,nums.size()-1);
            return ans;
        }
    };
```

#### 区间和的个数问题
+ 区间和的个数问题：[327.区间和的个数](https://leetcode.cn/problems/count-of-range-sum/description/)
+ 思路：
	+ 转化为归并排序进行求解，复杂度为 $O(NlogN)$。将原问题转化为**前缀和数组**的有序对统计问题，区间和 `[i, j]` 对应前缀和 `sum[j] - sum[i]`，要求满足 `lower ≤ sum[j] - sum[i] ≤ upper`，即等价于统计满足 `sum[j] - upper ≤ sum[i] ≤ sum[j] - lower` 的有序对 `(i, j)`（`i < j`）。
+ 代码：

```cpp
class Solution {
    public:
        
        int merge(vector<long long>& sum,int L,int M,int R, int lower, int upper)
        {
            int ans = 0;
            int winL = L;
            int winR = L;
            for(int i=M+1;i<=R;i++)
            {
                long long min_ = sum[i] - upper;
                long long max_ = sum[i] - lower;
                while(winR<=M && sum[winR]<=max_)
                    winR++;
                while(winL<=M && sum[winL]<min_)
                    winL++;
                ans += winR - winL;
            }
            //做merge操作
            vector<long long> temp(R-L+1);
            int k=0;
            int i=L;
            int j=M+1;
            while(i<=M && j<=R)
                temp[k++] = sum[i]<sum[j] ? sum[i++] : sum[j++];
            while(i<=M)
                temp[k++] = sum[i++];
            while(j<=R)
                temp[k++] = sum[j++];
            for(int k=0;k<temp.size();k++)
                sum[L+k] = temp[k];
    
            return ans;
        }
    
        int process(vector<long long>& sum,int L,int R ,int lower, int upper)
        {
            int ans = 0;
            int mid = L + (R-L)/2;
            //递归边界条件
            if(L==R)
            {
                if(sum[L]>=lower && sum[L]<=upper)
                    return 1;
                else
                    return 0;
            }
            ans = process(sum,L,mid,lower,upper) 
            + process(sum,mid+1,R,lower,upper) 
            + merge(sum,L,mid,R,lower,upper);
            return ans;
        }
    
        int countRangeSum(vector<int>& nums, int lower, int upper) 
        {
            if(nums.size() == 0)
                return 0;
    
            //求前缀和数组
            vector<long long> sum;
            sum.push_back(nums[0]);
            for(int i=1;i<nums.size();i++)
            {
                sum.push_back(sum[i-1]+nums[i]);
            }
    
            int ans = 0;
            ans = process(sum,0,sum.size()-1,lower,upper);
            return ans;
        }
    };
```

### 快速排序
#### 颜色分类
+ 颜色分类问题：[75.颜色分类](https://leetcode.cn/problems/sort-colors/)
+ 思路：

![颜色分类问题](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250420144955.png)

+ 代码：

```cpp
class Solution {
public:
    void swap(vector<int>& nums,int a,int b)
    {
        int temp = nums[a];
        nums[a] = nums[b];
        nums[b] = temp;
    }

    void sortColors(vector<int>& nums) {
        if(nums.size()==1)
            return;
        int pre = -1;//数组的前一个区域
        int now = 0;
        int last = nums.size();//数组后的一个数

        while(now<last)
        {
            if(nums[now]<1)
            {
                swap(nums,now,pre+1);
                pre++;
                now++;
            }
            else if(nums[now]>1)
            {
                swap(nums,now,last-1);
                last--;
            }
            else
                now++;
        }
    }
};
```

#### 随机快排
+ 思路：
	+ 在前面所述的颜色分类问题的基础上，随机快排的 `partition()` 函数将数组分为 `<基准值区域`、`=基准值区域`、`>基准值区域` 三个区域，通过 `pre` 和 `last` 标记边界；
	+ 快速排序随机选择从 `[L, R-1]` 范围上选择基准值与 `nums[R]` 进行交换，在代码中通过 `swap(nums,R,rand()%(R-L+1))` 体现；
	+ 随机快排采用递归的方式完成对数组的排序，需要注意随机快排依赖 `qickSort()` 函数中的**递归终止条件**（`L >= R`）。
+ 代码：

```cpp
class Solution {
    public:
	    //交换函数
        void swap(vector<int>& nums,int a,int b)
        {
            int temp = nums[a];
            nums[a] = nums[b];
            nums[b] = temp;
        }
        
	    //荷兰国旗问题（颜色分类问题）的变形
        vector<int> partition(vector<int>& nums,int L,int R)
        {
            vector<int> area(2);
            int pre = L-1; // <区
            int last = R; // >区
            int now = L;
            if(L==R)
            {
                area[0] = L;
                area[1] = R;
                return area;
            }
            else
            {
                while(now<last)
                {
                    if(nums[now]==nums[R])
                        now++;
                    else if(nums[now]<nums[R])
                    {
                        swap(nums,now,pre+1);
                        pre++;
                        now++;
                    }
                    else
                    {
                        swap(nums,now,last-1);
                        last--;
                    }
                }
                swap(nums,last,R);//最后把nums[R]放到=区的末尾
                area[0] = pre+1;
                area[1] = last;
                return area;
            }
        }

		//快排递归函数（注意递归边界）
        void qickSort(vector<int>& nums,int L,int R)
        {
            //递归边界（注意）
            if(L>=R)
                return;

            //随机选择一个数和nums[R]进行交换
            srand(time(NULL));
            swap(nums,R,L+rand()%(R-L+1));

            vector<int> area(2);
            area = partition(nums,L,R);
            qickSort(nums,L,area[0]-1);
            qickSort(nums,area[1]+1,R);
        }
    
        vector<int> sortArray(vector<int>& nums) {
            qickSort(nums,0,nums.size()-1);
            return nums;
        }
    };
```

+ 分析：
	+ 快排的空间复杂度是 $O(logn)$，并且快排算法是不稳定的。