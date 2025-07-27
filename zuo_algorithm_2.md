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

### 全排列
+ 全排列：[46.全排列](https://leetcode.cn/problems/permutations/)
+ 思路：普通全排列的题目是 `nums` 数组中没有重复的元素，因此不需要进行去重处理。全排列问题与之前的组合问题最大的区别在于 `[1,2]` 和 `[2,1]` 可以看作是两个不一样的结果，因此全排列的代码不需要 `startpoint` 来控制顺序，但需要 `used` 数组来记录哪些元素在上一个树层中已经用过了。
+ 代码：

```cpp
class Solution {
public:
    vector<int> path;
    vector<vector<int>> result;
    void backtracking(vector<int>& nums, vector<bool>& used) {
        // 终止条件
        if(path.size() == nums.size()){
            result.push_back(path);
            return;
        }
        // 递归回溯
        for(int i=0; i<nums.size(); i++){
            if(used[i] == true){
                path.push_back(nums[i]);
                used[i] = false;
                backtracking(nums, used);
                used[i] = true;
                path.pop_back();
            }
            else
                continue;
        }
    }

    vector<vector<int>> permute(vector<int>& nums){
        vector<bool> used(nums.size(), true);
        backtracking(nums, used);
        return result;
    }
};
```

### 全排列 II
+ 全排列 II：[47.全排列 II](https://leetcode.cn/problems/permutations-ii/description/)
+ 思路：
	+ 这道题目相对于上一道普通全排列问题的最大区别在于 `nums` 数组中存在重复的元素，因此需要进行去重。去重的逻辑与组合问题的去重逻辑基本一致，就是需要先对 `nums` 数组进行排序，然后根据 `used` 数组判断是在树层中重复还是在树枝上重复，最终只需要去掉同树层中重复的情况即可。
	+ 同样也有不排序去重的方法，和前面的非递减子序列题目类似，采用 `unordered_set<int> uset;` 来控制每一层的去重，需要注意的是 `uset` 需要写在 `for` 循环的外面，但是该方法更加耗时和占用了额外的内存空间。
+ 代码 1（排序去重）：

```cpp
class Solution {
public:
    vector<int> path;
    vector<vector<int>> result;
    void backtracking(vector<int>& nums, vector<bool>& used){
        // 终止条件
        if(path.size() == nums.size()){
            result.push_back(path);
            return;
        }
        // 递归回溯
        for(int i=0; i<nums.size(); i++){
            if(i>0 && nums[i] == nums[i-1] && used[i-1] == true)
                continue;
            if(used[i] == false) // 该元素已经被使用过了
                continue;
            else{
                path.push_back(nums[i]);
                used[i] = false;
                backtracking(nums, used);
                used[i] = true;
                path.pop_back();
            }
        }
    }

    vector<vector<int>> permuteUnique(vector<int>& nums) {
        vector<bool> used(nums.size(), true);
        sort(nums.begin(), nums.end());
        backtracking(nums, used);
        return result;
    }
};
```

+ 代码 2（非排序去重）：

```cpp
class Solution {
public:
    vector<int> path;
    vector<vector<int>> result;
    void backtracking(vector<int>& nums, vector<bool>& used){
        // 终止条件
        if(path.size() == nums.size()){
            result.push_back(path);
            return;
        }
        // 递归回溯
        unordered_set<int> uset;  // 控制同树层间去重
        for(int i=0; i<nums.size(); i++){
            if(used[i] == true && uset.find(nums[i]) == uset.end()){
                uset.insert(nums[i]);
                path.push_back(nums[i]);
                used[i] = false;
                backtracking(nums, used);
                used[i] = true;
                path.pop_back();
            }
            else
                continue;
        }
    }

    vector<vector<int>> permuteUnique(vector<int>& nums) {
        vector<bool> used(nums.size(), true);
        backtracking(nums, used);
        return result;
    }
};
```

### N 皇后
+ N 皇后：[51.N 皇后](https://leetcode.cn/problems/n-queens/description/)
+ 思路：N 皇后问题的递归回溯函数 `backtracking()` 函数并不复杂，遍历行 `row` 中的每一列 `col`，并采用 `isVaild()` 函数判断皇后放置在坐标 `[row, col]` 上是否合法，合法才能继续递归。而 `isVaild()` 函数检查同一列或同一斜线上是否有棋子（递归算法保证了每一行是不重复的），并且需要注意的是只需要往行数**减少方向**检查即可，行数**增大方向**并没有放置皇后，因此不需要检查！
+ 代码：

```cpp
class Solution {
public:
    vector<vector<string>> result;
    // 检查同一列或同一斜线上是否有棋子(递归算法保证了每一行是不重复的)
    bool isVaild(vector<string>& chessboard, int x, int y, int n){
        // 检查列是否重复
        for(int i=0; i<n; i++){
            if(chessboard[i][y] == 'Q')
                return false;
        }
        // 检查45°是否重复(只需要往行数减少方向检查即可)
        for(int i=x,j=y; i>=0 && j>=0; i--,j--){
            if(chessboard[i][j] == 'Q')
                return false;
        }
        // 检查135°是否重复(只需要往行数减少方向检查即可)
        for(int i=x,j=y; i>=0 && j<n; i--,j++){
            if(chessboard[i][j] == 'Q')
                return false;
        }
        return true;
    }
    // 递归回溯函数
    void backtracking(vector<string>& chessboard, int row, int n){
        // 终止条件
        if(row == n){
            result.push_back(chessboard);
            return;
        }
        // 递归回溯(遍历行row中的每一列col)
        for(int col=0; col<n; col++){
            if(isVaild(chessboard, row, col, n)){ // 合法
                chessboard[row][col] = 'Q'; // 放置皇后
                backtracking(chessboard, row+1, n);
                chessboard[row][col] = '.'; // 回溯
            }
        }
    }

    vector<vector<string>> solveNQueens(int n) {
        vector<string> chessboard(n, string(n, '.'));
        backtracking(chessboard, 0, n);
        return result;
    }
};
```

### 解数独
+ 解数独：[37.解数独](https://leetcode.cn/problems/sudoku-solver/description/)
+ 思路：这道题目与 N 皇后问题的最大区别在于 N 皇后问题只需要遍历行 `row` 中的每一列 `col` 来寻找一个放置皇后的位置即可，而解数独这道题目需要逐个遍历各个格子，如果发现该格子为字符 `'.'` 则需要遍历填入字符 `'1' - '9'`，填入一个数字后判断是否合法：
	+ 如果所选的数字合法则继续递归，此时如果后续递归是 `true` 的话就可以直接返回 `true`。
	+ 如果所选的数字不合法则在 `for` 循环填入下一个数字尝试。
	+ 或者后续递归返回 `false`，就需要回溯，并在 `for` 循环填入下一个数字尝试。
	+ 如果该空格填入了 `9` 个数字都不行则返回 `false`。
	+ 最终棋盘已经填满，则遍历完棋盘都没有返回 `false`，说明找到了合适棋盘位置了，直接返回 `true`。
+ 代码：

```cpp
class Solution {
public:
    bool isValid(int x, int y, char k, vector<vector<char>>& board){
        int n = board.size();
        // 判断行是否有字符k
        for(int i=0; i<n; i++){
            if(board[x][i] == k)
                return false;
        }
        // 判断行是否有字符k
        for(int i=0; i<n; i++){
            if(board[i][y] == k)
                return false;
        }
        // 判断九宫格内是否有字符k
        int m = x-x%3;
        int l = y-y%3;
        for(int i=m; i<m+3; i++){
            for(int j=l; j<l+3; j++){
                if(board[i][j] == k)
                    return false;
            }
        }
        return true;
    }

    bool backtracking(vector<vector<char>>& board){
        // 不需要终止条件
        int n = board.size();
        for(int i=0; i<n; i++){
            for(int j=0; j<n; j++){
                if(board[i][j] == '.'){
                    // 依次填入'1' - '9'
                    for(char k='1'; k<='9'; k++){
                        if(isValid(i, j, k, board)){
                            board[i][j] = k;
                            if(backtracking(board))
                                return true;
                            board[i][j] = '.'; // 回溯
                        }
                    }
                    // 9个数试完了都不行返回false
                    return false;
                }
            }
        }
        return true; // 遍历完没有返回false，说明找到了合适棋盘位置了
    }

    void solveSudoku(vector<vector<char>>& board) {
        backtracking(board);
    }
};
```

## 贪心算法
### 贪心算法理论基础
+ 贪心的本质是选择每一阶段的局部最优，从而达到全局最优。
+ 贪心算法一般分为如下四步：
	- 将问题分解为若干个子问题
	- 找出适合的贪心策略
	- 求解每一个子问题的最优解
	- 将局部最优解堆叠成全局最优解
+ 这个四步其实过于理论化了，我们平时在做贪心类的题目时，如果按照这四步去思考，真是有点“鸡肋”。做题的时候，只要想清楚**局部最优**是什么，能否推导出**全局最优**，其实就够了。
+ 总结：**贪心没有套路，说白了就是常识性推导加上举反例**。

### 分发饼干
+ 分发饼干：[455.分发饼干](https://leetcode.cn/problems/assign-cookies/description/)
+ 思路：这里的局部最优就是用尽可能接近该小孩胃口的饼干喂饱该小孩，充分利用饼干尺寸喂饱一个小孩，全局最优就是喂饱尽可能多的小孩。
	+ 代码开始是需要对两个数组进行排序，排序后判断胃口最小的小孩都比最大的饼干大则直接返回 `0`。
	+ 采用 `index` 控制遍历饼干大小数组的起始位置，当饼干 `[j]` 已经投喂了小孩 `[i]` 之后，小孩 `[i+1]` 就需要用 `[j+1]` 及之后的饼干投喂了。否则容易造成超时问题。
+ 代码：

```cpp
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        int ans = 0;
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());
        // 排序后判断胃口最小的小孩都比最大的饼干大则直接返回0
        if(s.size()>0 && g[0]>s[s.size()-1])
            return 0;
        int index = 0;
        for(int i=0; i<g.size(); i++){
            for(int j=index; j<s.size(); j++){
                if(s[j] >= g[i]){
                    ans++;
                    index = j+1;
                    break;
                }
            }
        }
        return ans;
    }
};
```

### 摆动序列
+ 摆动序列：[376.摆动序列](https://leetcode.cn/problems/wiggle-subsequence/description/)
+ 思路一：默认序列至少一个元素是峰值，令 `result = 1`，例如只有一个元素或者所有元素全部相等的的序列 `[2]` 和 `[2,2,2,2]`，元素本身就是峰值。对于序列 `[1,2,2,2,3,4]` ，只有出现峰值的时候才更新 `pre` 的值，这样就能保证当 `nums[i]` 指向最后一个 `2` 的时候出现多加 `1` 的情况。
+ 代码：

```cpp
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        // 默认至少一个元素是峰值
        int result = 1;
        if(nums.size() == 1)
            return 1;
        if(nums.size() > 2){
            int pre = 0;
            int nex = 0;
            for(int i=0; i<nums.size()-1; i++){
                nex = nums[i+1] - nums[i];
                // 出现峰值
                if((pre>=0 && nex<0) || (pre<=0 && nex>0)){
                    result++;
                    pre = nex;
                }
            }
        }
        return result;
    }
};
```

+ 思路二：分别使用 `up` 和 `down` 两个变量交替记录上升趋势和下降趋势，当 `nums[i] > nums[i-1]` 时，`up = down + 1`，当 `nums[i] < nums[i-1]` 时，`down = up + 1` ，这样当趋势持续上升或者下降的时候只会 `+1` 一次而不会重复记录，最终取 `up` 和 `down` 两个变量的最大值即作为结果。
+ 代码：

```cpp
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        int up = 1;
        int down = 1;
        int result = 1;
        if(nums.size() == 1)
            return result;
        if(nums.size() >= 2){
            for(int i=1; i<nums.size(); i++){
                if(nums[i] > nums[i-1])
                    up = down + 1;
                if(nums[i] < nums[i-1])
                    down = up + 1;
            }
        }
        result = max(up, down);
        return result;
    }
};
```

### 最大子数组和
+ 最大子数组和：[53.最大子数组和](https://leetcode.cn/problems/maximum-subarray/description/)
+ 思路一（贪心解法）：贪心解法的核心在于如果此前记录的子数组累加和为负数时，再和数组 `nums` 中后续的数相加会使得总的子数组和变小，因此子数组累加和为负数时应当置为 `0`，用后一个数为起点再次累加，每次需要将较大值通过 `result = max(result, sum)` 来记录。
+ 代码：

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        // 贪心解法
        int sum = 0; // 子数组累加和
        int result = INT_MIN;
        for(int i=0; i<nums.size(); i++){
            sum += nums[i];
            result = max(result, sum);
            if(sum < 0) // 子数组累加和为负数
                sum = 0;
        }
        return result;
    }
};
```

+ 思路二（前缀和解法）：应当找到一个尽可能小的前缀和 `MinPreSum`，然后用当前的总和 `sum` 减去此时最小的前缀和跟目前的最大子数组和比较再取最大值：`max(MaxSubSum, sum - MinPreSum)`。
+ 代码：

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        // 前缀和解法
        int sum = 0;
        // 最大子数组和
        int MaxSubSum = INT_MIN;
        // 最小前缀和(初始为0)
        int MinPreSum = 0;
        for(int i=0; i<nums.size(); i++){
            sum += nums[i];
            MaxSubSum = max(MaxSubSum, sum - MinPreSum);
            MinPreSum = min(sum, MinPreSum);
        }
        return MaxSubSum;
    }
};
```

### 买卖股票的最佳时机 II
+ 买卖股票的最佳时机 II：[122.买卖股票的最佳时机 II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/description/)
+ 思路：这道题目采用贪心算法的思路非常直观和简单，只要每一天的收益为正便进行股票的买卖从而使得总收益的最大化。局部最优是每一天的收益为正数，全局最优即为总的收益最大。
+ 代码：

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size() == 0 || prices.size() == 1)
            return 0;
        int sum = 0;
        for(int i=0; i<prices.size()-1; i++){
            if(prices[i] < prices[i+1])
                sum += prices[i+1] - prices[i];
        }
        return sum;
    }
};
```

### 跳跃游戏
+ 跳跃游戏：[55.跳跃游戏](https://leetcode.cn/problems/jump-game/description/)
+ 思路：本题的巧妙之处在于不去纠结每一跳具体是跳几步，只需要计算在 `i` 这个节点能够跳的范围 `cover`，并在 `cover` 这个范围内去寻找更大的范围，最终判断能否找到到达最后的范围。
+ 代码：

```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int cover = 0;
        for(int i=0; i<=cover; i++){
            cover = max(cover, i+nums[i]);
            if(cover >= nums.size()-1)
                return true;
        }
        return false;
    }
};
```

### 跳跃游戏 II
+ 跳跃游戏 II：[45.跳跃游戏 II](https://leetcode.cn/problems/jump-game-ii/description/)
+ 思路：这道题目需要求从起点跳到终点的最短跳跃次数，处理方法和上一题有较大区别。我们需要记录当前访问过的所有节点能够跳到的最远距离 `max_cover`，以及现阶段的覆盖距离 `now_cover`，要从现阶段的覆盖范围出发，不管怎么跳，覆盖范围内一定是可以跳到的，以最小的步数增加覆盖范围，覆盖范围一旦覆盖了终点，得到的就是最少步数！

![跳跃游戏 II](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202507251344404.png)

+ 代码：

```cpp
class Solution {
public:
    int jump(vector<int>& nums) {
        if(nums.size() == 1)
            return 0;
        int max_cover = 0;
        int now_cover = 0;
        int ans = 0;
        for(int i=0; i<nums.size(); i++){
            max_cover = max(max_cover, i+nums[i]);
            if(i == now_cover){
                now_cover = max_cover;
                ans++;
                if(now_cover >= nums.size()-1)
                    break;
            }
        }
        return ans;
    }
};
```

### K 次取反后最大化的数组和
+ K次取反后最大化的数组和：[1005.K次取反后最大化的数组和](https://leetcode.cn/problems/jump-game-ii/description/)
+ 思路：
	+ 贪心的思路：
		+ 当数组有正数和负数时，局部最优：让绝对值大的负数变为正数，当前数值达到最大。整体最优：整个数组和达到最大。
		+ 当数组只有正数和 `0` 后，局部最优：只让最小的数连续翻转，耗尽 `K` 值。整体最优：整个数组和达到最大。
+ 代码：

```cpp
class Solution {
public:
    int sum(vector<int>& nums){
        int ans = 0;
        for(int i=0; i<nums.size(); i++){
            ans += nums[i];
        }
        return ans;
    }

    int largestSumAfterKNegations(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        int i=0;
        while(i<nums.size() && nums[i]<0 && k>0){
            nums[i] = -nums[i];
            k--;
            i++;
        }
        sort(nums.begin(), nums.end());
        if(k%2 == 1) // k是奇数
            nums[0] = -nums[0];
        return sum(nums);
    }
};
```

### 加油站
+ 加油站：[134.加油站](https://leetcode.cn/problems/gas-station/description/)
+ 思路：如果总油量减去总消耗大于等于零那么一定可以跑完一圈，说明各个站点的加油站剩油量相加一定是要大于等于零的。那么每个加油站的剩余量 `rest[i]` 为 `gas[i] - cost[i]` 。`i` 从 `0` 开始累加 `rest[i]`，和记为 `curSum` ，一旦 `curSum` 小于零，说明 `[0, i]` 区间都不能作为起始位置，因为这个区间选择任何一个位置作为起点，到 `i` 这里都会断油，那么起始位置从 `i+1` 算起，再从 `0` 计算 `curSum`。
+ 代码：

```cpp
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int curSum = 0;
        int totalSum = 0;
        int start = 0;
        for(int i=0; i<gas.size(); i++){
            curSum  = curSum + (gas[i] - cost[i]);
            totalSum  = totalSum + (gas[i] - cost[i]);
            if(curSum < 0){
                start = i+1;
                curSum = 0;
            }
        }
        if(totalSum<0)
            return -1;
        return start;
    }
};
```