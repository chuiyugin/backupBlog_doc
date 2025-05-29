---
title: C++库函数积累应用
tags:
  - 库函数
categories:
  - C++
date: 2025-5-13 15:00:00
excerpt: Some useful C++ function!
---
# C++库函数积累应用
## 字符串 (string) 相关
+ 调整字符串数组长度的函数 `string::resize()`：
	+ 将字符串大小调整为 `n` 个字符的长度。
	+ 如果 `n` 小于当前字符串长度，则当前值将缩短为其第一个 `n` 个字符，删除超过 `n` 个字符的字符。
	+ 如果 `n` 大于当前字符串长度，则通过在末尾插入所需数量的字符来扩展当前内容，以达到 `n` 的大小。如果指定了 `char c`，则新元素初始化为 `char c` 的副本，否则，它们是值初始化字符（空字符）。

![string::resize()](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250513151409.png)

## 哈希表相关
### set
+ 在 C++中，`set` 提供以下三种数据结构，其底层实现以及优劣如下表所示：

![set 的三种实现方式](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250517202801.png)

+ `std::unordered_set` 底层实现为哈希表，`std::set` 和 `std::multiset` 的底层实现是红黑树，红黑树是一种平衡二叉搜索树，所以 `key` 值是有序的，但 `key` 不可以修改，改动 `key` 值会导致整棵树的错乱，所以只能删除和增加。
+ 当我们要使用集合来解决哈希问题的时候，优先使用 `unordered_set`，因为它的查询和增删效率是最优的，如果需要集合是有序的，那么就用 `set`，如果要求不仅有序还要有重复数据的话，那么就用 `multiset`。
+ 用法示例：
	+ 两个数组的交集：[349.两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/description/)
+ 代码：

```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> res;
        unordered_set<int> nums_1(nums1.begin(),nums1.end());
        for(int i=0; i<nums2.size(); i++)
        {
            if(nums_1.find(nums2[i])!=nums_1.end())
            {
                res.insert(nums2[i]);
            }
        }
        return vector<int>(res.begin(),res.end());
    }
};
```

+ 从这题可以学到两种用法：
	+ `find()`：找到的是对应数据位置的迭代器（迭代器：`set<int>::iterator`），找不到为 `nums_1.end()` 。（ `nums_1.begin()` 表示第一个元素，`nums_1.end()` 表示最后一个元素的后一位）。
	+ `unordered_set<int> nums_1(nums1.begin(), nums1.end());`：可以将一个容器的内容直接初始化到另外一个容器中，在末尾也有：`return vector<int>(res.begin(),res.end());`！

### map
+ 在 C++中，`map` 提供以下三种数据结构，其底层实现以及优劣如下表所示：

![map 的三种实现方式](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250518195557.png)

+ `std::unordered_map` 底层实现为哈希表，`std::map` 和 `std::multimap` 的底层实现是红黑树。
 + 用法示例：
	+ 两数之和：[1.两数之和](https://leetcode.cn/problems/two-sum/description/)
+ 代码：

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> mp;
        vector<int> ans;
        for(int i=0; i<nums.size(); i++)
        {
            if(mp.find(target-nums[i]) == mp.end())
            {
                mp.insert({nums[i],i});//注意map插入的形式
            }
            else
            {
                ans.push_back(i);
                unordered_map<int,int>::iterator it = mp.find(target-nums[i]);
                ans.push_back(it->second);
                break;
            }
        }
        return ans;
    }
};
```

+ 从这题可以学到两种用法：
	+ 注意 `map` 的插入用法，`mp.insert({nums[i],i});` 和 `mp.insert(pair<int, int>(nums[i], i));`，前者需要 `C++11` 或更高的版本，而后者适用于所有 `C++` 版本。
	+ 注意 `map` 的迭代器用法：其中 `unordered_map<int,int>::iterator` 也可以换成 `auto`，同样后者需要 `C++11` 或更高的版本，而前者适用于所有 `C++` 版本。

```cpp
unordered_map<int,int>::iterator it = mp.find(target-nums[i]);
ans.push_back(it->second);
```

## 栈和队列相关
### 双端队列（deque）
