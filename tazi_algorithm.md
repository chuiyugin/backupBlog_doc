---
title: 暑期算法学习经验分享
tags:
  - 算法学习
categories:
  - 算法学习
date: 2026-3-22 23:00:00
excerpt: C++、编程语言、算法学习
---

# 暑期算法学习经验分享
## 二分
### 二分找下标问题
+ 二分找下标问题：[二分找下标问题](https://codefun2000.com/p/P13044)
+ 思路：
	+ 需要注意在一段连续的相同的数中找到最左边和最右边的数的方法。
+ 代码：

```cpp
#include <bits/stdc++.h>
using namespace std;

int left(vector<int>& arr, int target){
    int left = 0;
    int right = arr.size()-1;
    int mid;
    while(left<=right){
        mid = left+(right-left)/2;
        if(arr[mid] < target)
            left = mid+1;
        else if(arr[mid] > target)
            right = mid-1;
        else{
            if(left == mid || arr[mid-1] < target)
                return mid;
            else
                right = mid-1;
        }
    }
    return -1;
}

int right(vector<int>& arr, int target){
    int left = 0;
    int right = arr.size()-1;
    int mid;
    while(left<=right){
        mid = left+(right-left)/2;
        if(arr[mid] < target)
            left = mid+1;
        else if(arr[mid] > target)
            right = mid-1;
        else{
            if(right == mid || arr[mid+1] > target)
                return mid;
            else
                left = mid+1;
        }
    }
    return -1;
}

int main(){
    int n, m;
    scanf("%d %d",&n, &m);
    vector<int> arr;
    int num;
    for(int i=0; i<n; i++){
        scanf("%d", &num);
        arr.push_back(num);
    }
    for(int i=0; i<m; i++){
        scanf("%d", &num);
        int a = left(arr, num);
        int b = right(arr, num);
        if(a != -1 && b!= -1)
            printf("%d %d\n", a+1, b+1);
        else
            printf("%d %d\n", a, b);
    }
    
    return 0;
}
```

## 字符串
###  字符串插入问题
+ 字符串插入问题：[字符串插入问题](https://codefun2000.com/p/P13037)
+ 思路：
	+ 需要注意 `substr()` 的用法。
+ 代码：

```cpp
#include <bits/stdc++.h>
using namespace std;

bool isVaild(const string& str){
    int i=0;
    int j=str.size()-1;
    while(i<j){
        if(str[i] != str[j])
            return false;
        i++;
        j--;
    }
    return true;
}

int main(){
    string a,b;
    getline(cin, a);
    getline(cin, b);
    bool ans = false;
    for(int i=0; i<=a.length(); i++){
        string left = a.substr(0, i);
        string right = a.substr(i, a.length()-i);
        string str = left+b+right;
        ans = isVaild(str);
        if(ans){
            break;
        }
    }
    if(ans)
        printf("YES");
    else
        printf("NO");
    return 0;
}
```
## 动态规划
### 0 - 1 背包问题
#### 理论基础
+ 理论基础详见：[0 - 1背包问题理论基础](https://chuiyugin.github.io/2025/03/07/zuo_algorithm_2/#%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92)
+ ⚠️注意区分以下两类问题：
#### 背包价值最大化（max）类→ 求可达性/最值
+  分割等和子集：[416.分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/description/)
+ 思路：
	+ 这道题目可以看成是 `0 - 1` 背包问题的变式，题目只要求将一个数组分割成两个子集，使得两个子集的元素和相等。那么可以将背包容量设定为数组总和 `sum` 的一半，即 `sum/2`，各个物品的重量和价值都是 `nums` 数组的元素，若能刚好装下 `sum/2` 容量的背包则返回 `true`，否则返回 `false`。
	+ 按照动态规划五部曲进行分析：
		+ 确定 `dp` 数组及下标的含义：`dp[j]` 的定义为：背包在容量为 `j` 的情况下从 `0` 到 `i` 的物品中选择装入得到的最大重量。 
		+ 确定递推公式（与 `0 - 1` 背包问题一致）：
			+ 状态转移方程 `dp[j] = max(dp[j], dp[j-nums[i]]+nums[i]);` ;
		+ `dp` 数组如何初始化：全部初始化成 `0`；
		+ 确定遍历顺序：在 `i` 上正序遍历，在 `j` 上倒序遍历；
		+ 举例推导 `dp` 数组符合要求。
+ 代码：

```cpp
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int sum = 0;
        for(int i=0; i<nums.size(); i++){
            sum+=nums[i];
        }
        if(sum%2 == 1) // 求和之后是奇数
            return false;
        // 背包容量是 sum/2
        int target = sum/2;
        vector<int> dp(target+1, 0);
        for(int i=0; i<nums.size(); i++){
            for(int j=target; j>=nums[i]; j--){
                dp[j] = max(dp[j], dp[j-nums[i]]+nums[i]);
                if(dp[target] == target)
                    return true;
            }
        }
        return false; 
    }
};
```

#### 装满背包有多少种方案类 → 求方案数
* 整数划分(01背包基础题)：[整数划分(01背包基础题)](https://codefun2000.com/p/P3627)
+ 思路：
	+ 这题本质是在问：从 `1~n` 里选若干个互不相同的数，使它们的和等于 `n`，一共有多少种选法。由于每个数只能用一次，所以它就是一个 **0/1 背包计数问题**。
	+ 按照动态规划五部曲进行分析：
		+ 确定 `dp` 数组及下标的含义：从数字 `1 ~ i` 中选取若干个互不相同的正整数，使它们的和恰好为 `j` 的方案数。
		+ 确定递推公式（与 `0 - 1` 背包问题一致）：
			+ 状态转移方程 `dp[j] = (dp[j] + dp[j - i]) % MOD;` ;
		+ `dp` 数组如何初始化：`dp[0] = 1` 表示凑出和为 `0` 有 1 种方法，即“什么都不选”；其余位置初始化为 `0`，表示一开始还没有方案能凑出对应的和。
		+ 确定遍历顺序：在 `i` 上正序遍历，在 `j` 上倒序遍历；
		+ 举例推导 `dp` 数组符合要求。
+ 代码：

```cpp
#include <bits/stdc++.h>

using namespace std;

const int MOD = 1e9 + 7;

int main(){
    int num;
    scanf("%d", &num);
    // dp[j] 表示装满容量为j的背包有多少种方案
    vector<int> dp(num+1, 0);
    dp[0] = 1;
    for(int i=1; i<=num; i++){
        for(int j=num; j>=i; j--){
            dp[j] = (dp[j] + dp[j - i]) % MOD;
        }
    }
    printf("%d", dp[num]%MOD);

    return 0;
}
```

## 二叉树
### 求二叉树的最大深度
+ 求二叉树的最大深度：[求二叉树的最大深度](https://codefun2000.com/p/P4018)
+ 思路：这道题难度不大，核心在于注意二叉树的 ACM 输入格式，详见 `buildTree` 函数（类似于层序遍历来建树）。
+ 代码：

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <sstream>

using namespace std;

struct TreeNode{
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x):val(x), left(nullptr), right(nullptr) {}
};

int travelsal(TreeNode* root){
    if(root == nullptr)
        return 0;
    int left = travelsal(root->left);
    int right = travelsal(root->right);
    return max(left, right)+1;
}

TreeNode* buildTree(vector<string>& treeList){
    if(treeList.empty() || treeList[0] == "null")
        return nullptr;
    
    TreeNode* root = new TreeNode(stoi(treeList[0]));
    int i=1;
    queue<TreeNode*> q;
    q.push(root);
    while(i<treeList.size()){
        TreeNode* cur = q.front();
        q.pop();
        // 左
        if(i<treeList.size() && treeList[i] != "null"){
            cur->left = new TreeNode(stoi(treeList[i]));
            q.push(cur->left);
        }
        i++;
        // 右
        if(i<treeList.size() && treeList[i] != "null"){
            cur->right = new TreeNode(stoi(treeList[i]));
            q.push(cur->right);
        }
        i++;
    }
    return root;
}

int main(){
    vector<string> treeList;
    string line;
    getline(cin, line);
    string token;
    stringstream ss(line);
    while(ss >> token){
        treeList.push_back(token);
    }
    TreeNode* root = buildTree(treeList);
    int ans;
    ans = travelsal(root);
    printf("%d", ans);

    return 0;
}
```

## 贪心
### 选商品
+ 选商品：[选商品](https://codefun2000.com/p/P13070)
+ 思路：
	+ 这题贪心的本质在于：优先选择“单位价值（`v/w`）最高”的物品
	+ 核心在于要熟悉对 `vector` 自定义排序用法的使用。
	+ 并且需要注意虽然 `map` 这个数据结构是有序的，但是默认是从小到大排序，并不符合本题的要求。
+ 代码：

```cpp
#include <bits/stdc++.h>
using namespace std;

struct Item{
    int w;
    int v;
    double ratio;
};

// 大于号从大到小排序
static bool cmp(const Item& a, const Item& b){
    return a.ratio > b.ratio;
}

int main(){
    int n, C;
    scanf("%d %d",&n, &C);
    vector<Item> items;
    for(int i=0; i<n; i++){
        Item item;
        scanf("%d %d", &item.w, &item.v);
        item.ratio = double(item.v) / item.w;
        items.push_back(item);
    }
    sort(items.begin(), items.end(), cmp);
    double ans = 0;
    for(int i=0; i<n; i++){
        if(C - items[i].w >= 0){
            ans += items[i].v;
            C -= items[i].w;
        }else{
            ans += items[i].ratio * C;
            break;
        }    
    }
    printf("%.2f", ans);
    return 0;
}
```