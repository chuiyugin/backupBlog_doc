---
title: 左程云、代码随想录算法学习（二）
tags:
  - 算法学习
categories:
  - 算法学习
date: 2025-3-7 15:00:00
excerpt: C++、编程语言、算法学习
---
# 左程云、代码随想录算法学习（二）
## 回溯算法
### 回溯算法代码模板
+ 回溯算法的核心思想是：
	+ 递归构造解空间树，在树的每个节点上做出一个选择，并继续探索；若当前选择不符合条件则回退（撤销选择）；
+ 代码模板：

```cpp
void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }
    // 递归遍历
    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}
```
### 组合问题
+ 组合问题：[77.组合](https://leetcode.cn/problems/combinations/)
+ 思路：
	+ 当当前路径 `path` 中的元素数量达到 `k`，即完成一个组合，加入结果集中，作为**终止条件**；
	+ 从 `startpoint` 开始，避免重复元素；
	+ 每次递归往路径中添加一个数字 `i`；
	+ 然后递归继续选下一个数（从 `i+1` 开始）；
	+ 最后撤销上一步的选择，进行回溯。
+ 代码：

```cpp
class Solution {
public:
    vector<int> path;
    vector<vector<int>> result;
    void backtracking(int n, int k,int startpoint){
        // 终止条件
        if(path.size() == k){
            result.push_back(path);
            return;
        }
        // 递归遍历
        for(int i=startpoint; i<=n; i++){
            path.push_back(i);
            backtracking(n, k, i+1);
            path.pop_back();
        }
    }

    vector<vector<int>> combine(int n, int k) {
        backtracking(n, k, 1);
        return result;
    }
};
```

+ 剪枝优化：
	+ 在递归遍历部分可以进行剪枝优化，`for` 循环至多从 `n-(k-path.size())+1` 这个位置开始才能避免无效的递归遍历。
+ 剪枝优化后代码：

```cpp
class Solution {
public:
    vector<int> path;
    vector<vector<int>> result;
    void backtracking(int n, int k,int startpoint){
        // 终止条件
        if(path.size() == k){
            result.push_back(path);
            return;
        }
        // 剪枝优化 n-(k-path.size())+1
        for(int i=startpoint; i<=n-(k-path.size())+1; i++){ 
            path.push_back(i);
            backtracking(n, k, i+1);
            path.pop_back();
        }
    }

    vector<vector<int>> combine(int n, int k) {
        backtracking(n, k, 1);
        return result;
    }
};
```