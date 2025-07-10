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

### 组合总和 III
+ 组合总和 III：[216.组合总和 III](https://leetcode.cn/problems/combination-sum-iii/description/)
+ 思路：这道题目与上一题较为类似，仅需在遍历过程中加上求和 `sum` 的过程以及 `sum` 的回溯过程即可。同样可以做剪枝操作：
	+ 当总和 `sum` 已经大于 `n` 的时候已经没有意义，不再需要继续遍历；
	+ `for` 循环至多从 `9-(k-path.size())+1` 这个位置开始才能避免无效的递归遍历。
+ 代码：

```cpp
class Solution {
public:
    vector<int> path;
    vector<vector<int>> result;
    int sum = 0;
    void backtracking(int k, int n, int startpoint){
        // 终止条件
        if(sum > n)
            return;
        if(path.size() == k){
            if(sum == n)
                result.push_back(path);
            return;
        }
        // 递归遍历(剪枝操作)
        for(int i = startpoint; i<=9-(k-path.size())+1; i++){
            path.push_back(i);
            sum += i;
            backtracking(k, n, i+1);
            sum -= i;
            path.pop_back();
        }
    }

    vector<vector<int>> combinationSum3(int k, int n) {
        backtracking(k, n, 1);
        return result;
    }
};
```

### 电话号码的字母组合
+ 电话号码的字母组合：[17.电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)
+ 思路：这道题目首先需要建立数字与电话号码字母的对照数组，递归终止条件在于字符串 `path` 的长度和题目输入的 `digits` 字符串的长度相等，在递归逻辑中采用 `startpoint` 来控制选取 `digits` 字符串中的哪个数字，用 `index` 来表示该数字对应的手机中字符串中的字母的数量用于后续 `for` 循环遍历，循环内部为经典回溯算法。
+ 代码：

```cpp
class Solution {
public:
    vector<string> result;
    string path;
    const string Num_letterMap[10] = {
        "", // 0
        "", // 1
        "abc", // 2
        "def", // 3
        "ghi", // 4
        "jkl", // 5
        "mno", // 6
        "pqrs", // 7
        "tuv", // 8
        "wxyz", // 9
    };
    
    void backtracking(string digits, int startpoint){
        // 终止条件
        if(path.size() == digits.size()){
            result.push_back(path);
            return;
        }
        // 递归逻辑
        int digit = digits[startpoint]-'0'; // 取出里面的数字
        int index = Num_letterMap[digit].size();
        for(int i=0; i<index; i++){
            path.push_back(Num_letterMap[digit][i]);
            backtracking(digits, startpoint+1);
            path.pop_back();
        }
    }

    vector<string> letterCombinations(string digits) {
        if(digits.size() == 0)
            return result;
        backtracking(digits, 0);
        return result;
    }
};
```