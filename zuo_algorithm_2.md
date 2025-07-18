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
	+ 回溯法抽象为树形结构后，其遍历过程就是：`for`循环横向遍历，递归纵向遍历，回溯不断调整结果集。
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

### 组合总和
+ 组合总和：[39.组合总和](https://leetcode.cn/problems/combination-sum/description/)
+ 思路：这道题目和之前的组合总和 III 的关键不同点在于组合总和问题能够重复利用当前元素，该条件要建立在 `candidates` 内的元素是不重复的，这样子遍历出来的解集也不会出现重复的组合。因此代码仅仅需要将迭代递归语句中的 `i+1` 改为 `i` 即可。
+ 代码：

```cpp
class Solution {
public:
    int sum = 0;
    vector<int> path;
    vector<vector<int>> result;
    void backtracking(vector<int>& candidates, int target, int startpoint){
        // 终止条件
        if(sum > target)
            return;
        if(sum == target){
            result.push_back(path);
        }
        // 遍历迭代
        for(int i=startpoint; i<candidates.size(); i++){
            sum += candidates[i];
            path.push_back(candidates[i]);
            backtracking(candidates, target, i); // 关键点
            sum -= candidates[i];
            path.pop_back();
        }
    }

    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        backtracking(candidates, target, 0);
        return result;
    }
};
```

### 组合总和 II
+ 组合总和 II：[40.组合总和 II](https://leetcode.cn/problems/combination-sum-ii/)
+ 思路：这道题和之前的组合总和问题最大区别在于 `candidates` 数组中的元素是包含相同的重复元素，因此按照先前的遍历方式解集必定会出现重复集合，因此处理难度在于去重操作。具体操作如下：
	+ 先对 `candidates` 数组进行排序，`sort(candidates.begin(), candidates.end());`；
	+ 采用 `used` 数组来控制在同树层方向去重，而树枝方向不需要去重；
	+ 在迭代遍历逻辑中 `true` 表示进入下一层时相同元素可以继续用；
	+ `false` 表示进入在层深度一样时相同元素不能继续用了。
+ 代码：

```cpp
class Solution {
public:
    vector<int> path;
    vector<vector<int>> result;
    int sum = 0;
    void backtracking(vector<int>& candidates, int target, int startpoint, vector<bool> used){
        // 终止条件
        if(sum > target)
            return;
        if(sum == target){
            result.push_back(path);
            return;
        }
        // 遍历迭代
        for(int i=startpoint; i<candidates.size(); i++){
            // used 数组控制树层去重
            if(i>0 && candidates[i] == candidates[i-1] && used[i-1] == false)
                continue;
            path.push_back(candidates[i]);
            sum += candidates[i];
            used[i] = true; // true 表示进入下一层时相同元素可以继续用
            backtracking(candidates, target, i+1, used);
            used[i] = false; // 回溯置为 false，指示同层的相同元素不能再用了
            sum -= candidates[i];
            path.pop_back();
        }
    }

    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        // 初始化used数组，第一个参数是数组大小，第二参数是每个位置的值
        vector<bool> used(candidates.size(), false); 
        backtracking(candidates, target, 0, used);
        return result;
    }
};
```

### 分割回文串
+ 分割回文串：[131.分割回文串](https://leetcode.cn/problems/palindrome-partitioning/description/)
+ 思路：
	+ 在递归回溯逻辑中，先判断 `[startpoint, i]` 区间的子串是否为回文串：
		+ 如果是的话就获取（ `s.substr(startpoint, i-startpoint+1）` ）该子串，然后将其添加至 `path` 中。
		+ 如果不是的话，后续不需要在遍历，直接 `continue` 跳过循环。
	+ 根据上述循环逻辑，只要 `startpoint` 能够到达字符串的末尾（ `if(startpoint>=s.size())` ）就说明找到了一种分割方案，可以将 `path` 添加到 `result` 中。
	+ 判断字符串是否为回文串可以采用双指针法。

![分割回文串](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/20250714150713.png)

+ 代码：

```cpp
class Solution {
public:
    vector<vector<string>> result;
    vector<string> path;

    // 判断是否回文串
    bool isPalindrome(const string s, int start, int end){
        int i=start, j=end;
        while(i<=j){
            if(s[i] != s[j])
                return false;
            i++;
            j--;
        }
        return true;
    }

    void backtracking(string s, int startpoint){
        // 终止条件（startpoint能够到达字符串末尾说明一定是满足分割要求）
        if(startpoint>=s.size()){
            result.push_back(path);
            return;
        }
        // 递归回溯逻辑
        for(int i=startpoint; i<s.size(); i++){
            if(isPalindrome(s, startpoint, i)){
                // 获取[startpoint,i]在s中的子串
                string str = s.substr(startpoint, i-startpoint+1);
                path.push_back(str);
            }
            else // 不是回文串，跳过
                continue;
            backtracking(s, i+1);
            path.pop_back();
        }
    }

    vector<vector<string>> partition(string s) {
        backtracking(s, 0);
        return result;
    }
};
```

### 复原IP地址
+ 复原IP地址：[93.复原IP地址](https://leetcode.cn/problems/restore-ip-addresses/description/)
+ 思路：
	+ 这道题同样与前一题类似是分割字符串问题，同样在递归回溯逻辑中，先判断按照 `[startpoint, i]` 区间分割的 IP 子串是否合法：
		+ 合法则给子串后面加上 `'.'` ，递归进入下一个子串的遍历；
		+ 不合法则直接 `continue` 跳过当前循环。
	+ 需要注意判断 IP 地址子串是否合法的一些特性，并构成 `isVaild()` 函数。
+ 代码：

```cpp
class Solution {
public:
    // 判断子串中的数字是否合法，区间[start, end]
    bool isVaild(string s, int start, int end){
        int sum = 0;
        if(end-start>2)
            return false;
        for(int i=start; i<=end; i++){
            if(s[i]-'0'>9 || s[i]-'0'<0)
                return false;
            else{
                sum *= 10;
                sum += s[i]-'0';
            }
        }
        if(end-start == 0 && s[start]-'0' == 0)
            return true;
        else if(end-start > 0 && s[start]-'0' == 0)
            return false;
        else if(sum > 0 && sum <= 255)
            return true;
        else
            return false;
    }

    vector<string> result;

    void backtracking(string s, int startpoint, int pointSum){
        // 终止条件
        if(pointSum == 3){ // 已经插入了 3 个点
            // 判断从 startpoint 到末尾的子串是否合法
            if(isVaild(s, startpoint, s.size()-1)){ 
                result.push_back(s);
            }
            return;
        } 
        // 递归回溯逻辑
        for(int i=startpoint; i<s.size(); i++){
            if(isVaild(s, startpoint, i)){
                s.insert(s.begin()+i+1, '.');
                pointSum++;
                // i+2 跳过刚加上去的 .
                backtracking(s, i+2, pointSum);
                pointSum--;
                s.erase(s.begin()+i+1);
            }
            else
                continue;
        }
    }

    vector<string> restoreIpAddresses(string s) {
        if(s.size() < 4 || s.size() > 12) 
            return result; // 算是剪枝了
        backtracking(s, 0, 0);
        return result;
    }
};
```

### 子集
+ 子集：[78.子集](https://leetcode.cn/problems/subsets/description/)
+ 思路：如果把子集问题、组合问题、分割问题都抽象为一棵树的话，**那么组合问题和分割问题都是收集树的叶子节点，而子集问题是找树的所有节点！**
	+ 因此将收获结果的代码放在了终止条件之前，确保将每个节点的结果都收集。
+ 代码：

```cpp
class Solution {
public:
    vector<int> path;
    vector<vector<int>> result;
    void backtracking(vector<int>& nums, int startpoint){
        // 收获结果
        result.push_back(path);
        // 终止条件
        if(startpoint >= nums.size())
            return;
        // 递归回溯逻辑
        for(int i=startpoint; i<nums.size(); i++){
            path.push_back(nums[i]);
            backtracking(nums, i+1);
            path.pop_back();
        }
    }

    vector<vector<int>> subsets(vector<int>& nums) {
        backtracking(nums, 0);
        return result;
    }
};
```

### 子集 II
+ 子集：[90.子集 II](https://leetcode.cn/problems/subsets-ii/description/)
+ 思路：这道题目与组合总和 II 比较类似，同样是采用 `used` 数组来控制在同树层方向去重，而树枝方向不需要去重；
+ 代码：

```cpp
class Solution {
public:
    vector<int> path;
    vector<vector<int>> result;
    void backtracking(vector<int>& nums, int startpoint, vector<bool>& used){
        // 收集结果
        result.push_back(path);
        // 终止条件
        if(startpoint>=nums.size())
            return;
        // 递归回溯逻辑
        for(int i=startpoint; i<nums.size(); i++){
            if(i>0 && nums[i] == nums[i-1] && used[i-1] == false)
                continue; // 树层重复元素不能使用
            else{
                path.push_back(nums[i]);
                used[i] = true; // 控制树枝重复元素可以使用
                backtracking(nums, i+1, used);
                used[i] = false;
                path.pop_back();
            }
        }
    }

    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        vector<bool> used(nums.size(), false);
        backtracking(nums, 0, used);
        return result;
    }
};
```

### 非递减子序列
+ 非递减子序列：[491.非递减子序列](https://leetcode.cn/problems/non-decreasing-subsequences/description/)
+ 思路：这道题目和之前需要去重的题目主要区别在于不能够对原始的 `nums` 数组进行排序，虽然同样是在同树层方向去重，而树枝方向不需要去重，但是不能像之前的题目一样采用 `used` 数组来去重了。此时需要采用 `unordered_set<int> uset;` 来控制每一层的去重，需要注意的是 `uset` 需要写在 `for` 循环的外面。
+ 代码：

```cpp
class Solution {
public:
    vector<int> path;
    vector<vector<int>> result;
    void backtracking(vector<int>& nums, int startpoint){
        // 终止条件
        if(path.size()>1)
            result.push_back(path);
        // 递归回溯
        unordered_set<int> uset; // 用于判断同树层中有没有重复的数
        for(int i=startpoint; i<nums.size(); i++){
            if(uset.find(nums[i]) != uset.end())
                continue;
            else{
                uset.insert(nums[i]);
                if(path.empty() || (!path.empty() && nums[i] >= path.back())){
                    path.push_back(nums[i]);
                    backtracking(nums, i+1);
                    path.pop_back();
                }
            }
        }
    }

    vector<vector<int>> findSubsequences(vector<int>& nums) {
        backtracking(nums, 0);
        return result;
    }
};
```