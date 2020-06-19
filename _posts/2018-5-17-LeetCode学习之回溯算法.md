---
layout: post
title:  "LeetCode学习之回溯算法"
date:   2018-05-17
categories: 算法学习  
tags: 算法学习


---

* content
{:toc}


本文为LeetCode中关于回溯算法的题目学习

# 关于DFS算法

> DFS算法其实就是回溯算法。
>
> 解决一个回溯问题，实际上就是一个决策树的遍历过程。其实值需要思考三个问题：
>
> 1->路径：也就是已经做出的选择。
>
> 2->选择列表：也就是你当前可以做的选择。
>
> 3->结束条件：也就是到达决策树底层，无法再做选择的条件。
>
> 代码方面，回溯算法的框架：

~~~~
result = []
def  backtrack(路径 ， 选择列表):
	if 满足结束条件：
		result.add(路径)
		return
	for 选择 in 选择列表：
		做选择
		backtrace(路径， 选择列表)
		撤销选择
~~~~

> 其核心就是for循环里边的递归，再递归调用之后撤销 选择，特别简单。

# LeetCode 5-4

#### [401. 二进制手表](https://leetcode-cn.com/problems/binary-watch/)

二进制手表顶部有 4 个 LED 代表**小时（0-11）**，底部的 6 个 LED 代表**分钟（0-59）**。

每个 LED 代表一个 0 或 1，最低位在右侧。

![img](https://upload.wikimedia.org/wikipedia/commons/8/8b/Binary_clock_samui_moon.jpg)

例如，上面的二进制手表读取 “3:25”。

给定一个非负整数 *n* 代表当前 LED 亮着的数量，返回所有可能的时间。

**案例:**

```
输入: n = 1
返回: ["1:00", "2:00", "4:00", "8:00", "0:01", "0:02", "0:04", "0:08", "0:16", "0:32"]
```

 

**注意事项:**

- 输出的顺序没有要求。
- 小时不会以零开头，比如 “01:00” 是不允许的，应为 “1:00”。
- 分钟必须由两位数组成，可能会以零开头，比如 “10:2” 是无效的，应为 “10:02”。

**代码:**

```c++
vector<string> readBinaryWatch(int num) {
    vector<string> res;
    //直接遍历  0:00 -> 12:00   每个时间有多少1
    for (int i = 0; i < 12; i++) {
        int n = count1(i);
        if(n > num)
            continue;
        for (int j = 0; j < 60; j++) {
            if (n + count1(j) == num) 
                res.push_back(to_string(i)+":" + (j < 10 ? "0"+to_string(j) : to_string(j)));
        }
    }
    return res;
}
//计算二进制中1的个数
int count1(int n) {
    int res = 0;
    while (n != 0) {
        n = n & (n - 1);
        res++;
    }
    return res;
}
```

 [784. 字母大小写全排列](https://leetcode-cn.com/problems/letter-case-permutation/)

给定一个字符串`S`，通过将字符串`S`中的每个字母转变大小写，我们可以获得一个新的字符串。返回所有可能得到的字符串集合。

```
示例:
输入: S = "a1b2"
输出: ["a1b2", "a1B2", "A1b2", "A1B2"]

输入: S = "3z4"
输出: ["3z4", "3Z4"]

输入: S = "12345"
输出: ["12345"]
```

**注意：**

- `S` 的长度不超过`12`。
- `S` 仅由数字和字母组成。

**代码:**

```c++
/*
	1:回溯 
	遍历字符串，如果到最后，将字符串加入结果中。
			  如果当前字符为大写， 转为小写，继续回溯
			  如果当前字符为小写， 转为大写，继续回溯
*/
vector<string> letterCasePermutation(string S) {
    vector<string> res;
    string tmp = S;
    backTrace(tmp , 0 , res);
    return res;
}
void backTrace(string& s , int pos ,vector<string>& res){
    if(pos == s.size()){
        res.push_back(s);
        return;
    }
    backTrace(s , pos+1 , res);
    if(s[pos] >= 'A' && s[pos] <= 'Z')
        s[pos] =s[pos] + 32;
    else if(s[pos] >= 'a' && s[pos] <= 'z')
        s[pos] =s[pos] - 32;
    else   
        return;
    backTrace(s , pos+1 , res);
}
```

# LeetCode：5-5

#### [17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

给定一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/original_images/17_telephone_keypad.png)

**示例:**

```
输入："23"
输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

**代码:**

```c++
vector<string> letterCombinations(string digits) {
    vector<string> res;
    if(digits.size() == 0)
        return res;
    backTrace(res , digits , 0);
    return res;
}
void backTrace(vector<string>& res , string dig , int pos){
    if(pos >= dig.size()){
        res.push_back(dig);
        return;
    }
    char tmp = dig[pos];
    int start = tmp + 47 + (tmp -'2')*2 ;
    if(tmp == '8' || tmp == '9')
        start = tmp + 48 + (tmp -'2')*2;        
    dig[pos] = start;
    backTrace(res , dig , pos+1);
    dig[pos] = start + 1 ;
    backTrace(res , dig , pos+1);
    dig[pos] = start + 2;
    backTrace(res , dig , pos+1);
    if(tmp == '7' || tmp == '9'){
        dig[pos] = start + 3;
        backTrace(res , dig , pos+1);
    }
}
```

#### [22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)

数字 *n* 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 **有效的** 括号组合。 

**示例：**

```
输入：n = 3
输出：[
       "((()))",
       "(()())",
       "(())()",
       "()(())",
       "()()()"
     ]
```

**代码：**

```c++
vector<string> generateParenthesis(int n) {
    vector<string> res;
    int lc = 0, rc = 0;
    dfs(res, "", n, lc, rc);
    return res;
}
void dfs(vector<string>& res, string path, int n, int lc, int rc) {
    if (rc > lc || lc > n || rc > n) return;
    if (lc == n && rc == n) {
        res.push_back(path);
        return;
    }
    dfs(res, path + '(', n, lc + 1, rc);
    dfs(res, path + ')', n, lc, rc + 1);
}
```

#### [39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)

给定一个**无重复元素**的数组 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。

`candidates` 中的数字可以无限制重复被选取。

**说明：**

- 所有数字（包括 `target`）都是正整数。
- 解集不能包含重复的组合。 

**示例 1:**

```
输入: candidates = [2,3,6,7], target = 7,
所求解集为:
[
  [7],
  [2,2,3]
]
```

**示例 2:**

```
输入: candidates = [2,3,5], target = 8,
所求解集为:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```

**代码：**

```c++
class Solution {
private:
    vector<vector<int>> res;
    vector<int> path;//存储回溯过程中的路径
public:
    void backTrace(int start, int target , vector<int> & candidates) {
        if (target == 0) {
            res.push_back(path);
            return;
        }
        for (int i = start;i < candidates.size() && target - candidates[i] >= 0; i++) {
            path.push_back(candidates[i]);
            backTrace(i , target - candidates[i] , candidates);
            path.pop_back();
        }
    }
    vector<vector<int>> combinationSum(vector<int> &candidates, int target) {
        std::sort(candidates.begin(), candidates.end());//必须排序
        backTrace(0 , target , candidates);
        return res;
    }
};
```

#### [40. 组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii/)

给定一个数组 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。

`candidates` 中的每个数字在每个组合中只能使用一次。

**说明：**

- 所有数字（包括目标数）都是正整数。
- 解集不能包含重复的组合。 

**示例 1:**

```
输入: candidates = [10,1,2,7,6,1,5], target = 8,
所求解集为:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```

**示例 2:**

```
输入: candidates = [2,5,2,1,2], target = 5,
所求解集为:
[
  [1,2,2],
  [5]
]
```

**代码:**

```c++
class Solution {
private:
    vector<vector<int>> res;
    vector<int> path;
public:
    void backTrace(int start, int target , vector<int> & candidates) {
        if (target == 0) {
            res.push_back(path);
            return;
        }
        for (int i = start;i < candidates.size(); i++) {
            // 因为数组有重复，需要去重
            if(i > start && candidates[i] == candidates[i-1])
                continue;
            if(target - candidates[i] >= 0){
                path.push_back(candidates[i]);
                backTrace(i+1 , target - candidates[i] , candidates);
                path.pop_back();
            }
        }
    }
    vector<vector<int>> combinationSum2(vector<int> &candidates, int target) {
        std::sort(candidates.begin(), candidates.end());
        backTrace(0 , target , candidates);
        return res;
    }
};
```

#### [46. 全排列](https://leetcode-cn.com/problems/permutations/)

给定一个 **没有重复** 数字的序列，返回其所有可能的全排列。

**示例:**

```
输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

**代码：**

```c++
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;
    vector<int> flag;
    vector<vector<int>> permute(vector<int>& nums) {
        flag.resize(nums.size() , 0);
        backTrace(nums , flag);
        return res;
    }
    void backTrace(vector<int>& nums,vector<int>& flag){
        if(path.size() == nums.size()){
            res.push_back(path);
            return;
        }
        for(int i = 0 ; i < nums.size() ; i++){
            if(flag[i] == 1)
                continue;
            flag[i] = 1;
            path.push_back(nums[i]);
            backTrace(nums , flag);
            flag[i] = 0 ;
            path.pop_back();
        }
    }

};
```

#### [47. 全排列 II](https://leetcode-cn.com/problems/permutations-ii/)

给定一个可包含重复数字的序列，返回所有不重复的全排列。

**示例:**

```
输入: [1,1,2]
输出:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```

**代码:**

```c++
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;
    vector<int> flag;
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin() , nums.end());
        flag.resize(nums.size() , 0);
        backTrace(nums , flag);
        return res;
    }
    void backTrace(vector<int>& nums,vector<int>& flag){
        if(path.size() == nums.size()){
            res.push_back(path);
            return;
        }
        for(int i = 0 ; i < nums.size() ; i++){
            if(flag[i] == 1)
                continue;
            if(i > 0 && nums[i] == nums[i-1] && flag[i-1]==1)
                continue;
            flag[i] = 1;
            path.push_back(nums[i]);
            backTrace(nums , flag);
            flag[i] = 0 ;
            path.pop_back();
        }
    }
};
```

# LeetCode 5-6

#### [60. 第k个排列](https://leetcode-cn.com/problems/permutation-sequence/)

给出集合 `[1,2,3,…,*n*]`，其所有元素共有 *n*! 种排列。

按大小顺序列出所有排列情况，并一一标记，当 *n* = 3 时, 所有排列如下：

1. `"123"`
2. `"132"`
3. `"213"`
4. `"231"`
5. `"312"`
6. `"321"`

给定 *n* 和 *k*，返回第 *k* 个排列。

**说明：**

- 给定 *n* 的范围是 [1, 9]。
- 给定 *k* 的范围是[1,  *n*!]。

**示例 1:**

```
输入: n = 3, k = 3
输出: "213"
```

**示例 2:**

```
输入: n = 4, k = 9
输出: "2314"
```

**代码:**

```c++
class Solution {
private:
    // 以n = 4 k = 17 为例  
    // 第一个值为1的序列总共有3*2*1个
    // k = 17 那么序列第一个值为3。更新K = 17-12 = 5
    // 第二个值为1的序列有2*1个，
    // k = 5 ，则第二个值为 2  更新k = 5-4 =1
    // 第三个值为1的序列有1个 
    // k = 1 , 则第三个值为 1  更新k = 1-1 = 0 
    vector<int> hash; // 记录数字的使用情况
    vector<int> factorial; // 记录各个阶乘
    bool flag = true; // 是否结束标识
    void dfs(int cur, int n, int &k, string &ret){
        if(cur == n){
            flag = false;
            return;
        }
        int temp = factorial[n-cur-1];
        for(int i=0; i<n; i++){
            if(hash[i] && flag){
                if(temp < k ){
                    k = k - temp;
                    continue;
                }
                ret.push_back(i+1+'0');// 将数字转为字符
                hash[i] = 0;// 设置标志位
                dfs(cur+1,n,k,ret);
            }
        }
    }

public:
    string getPermutation(int n, int k) {
        if(n == 1) return "1";
        factorial.resize(n);
        factorial[0] = 1;
        factorial[1] = 1;
        // 获取n的阶乘
        for(int i=2; i<n; i++)
            factorial[i] = i * factorial[i-1];
        string ret;
        string path;
        hash.resize(n,1);
        dfs(0,n,k,ret);
        return ret;
    }
};
```

#### [77. 组合](https://leetcode-cn.com/problems/combinations/)

给定两个整数 *n* 和 *k*，返回 1 ... *n* 中所有可能的 *k* 个数的组合。

**示例:**

```
输入: n = 4, k = 2
输出:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

**代码:**

```c++
class Solution {
public:
    vector<vector<int>> res;
    void backTrace(int pos , int n , int k , vector<int>& tmp){
        if(tmp.size() == k){//这里不能通过pos去判断  想清楚
            res.push_back(tmp);
            return;
        }
        for(int i = pos ; i <= n ; i++){
            tmp.push_back(i);
            backTrace(i+1 , n , k , tmp);// 为啥是i+1呢  需要想清楚
            tmp.pop_back();
        }
    }
    vector<vector<int>> combine(int n, int k) {
        vector<int> tmp;
        backTrace(1 , n , k , tmp);
        return res;

    }
};
```

#### [78. 子集](https://leetcode-cn.com/problems/subsets/)



给定一组**不含重复元素**的整数数组 *nums*，返回该数组所有可能的子集（幂集）。

**说明：**解集不能包含重复的子集。

**示例:**

```
输入: nums = [1,2,3]
输出:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```

**代码:**

```c++
class Solution {
public:
    vector<vector<int>> res;
    void backTrace(vector<int>& nums , int pos , vector<int>& tmp){
        if(tmp.size() <= nums.size()){
            res.push_back(tmp);
        }else{
            return;
        }
        for(int i = pos ; i < nums.size() ; i++){
            tmp.push_back(nums[i]);
            backTrace(nums , i+1 , tmp);
            tmp.pop_back();
        }
    }
    vector<vector<int>> subsets(vector<int>& nums) {
       vector<int> tmp;
       backTrace(nums , 0 , tmp);
       return res;
    }
};
```

#### 

#### [79. 单词搜索](https://leetcode-cn.com/problems/word-search/)

给定一个二维网格和一个单词，找出该单词是否存在于网格中。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

 

**示例:**

```
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

给定 word = "ABCCED", 返回 true
给定 word = "SEE", 返回 true
给定 word = "ABCB", 返回 false
```

 

**提示：**

- `board` 和 `word` 中只包含大写和小写英文字母。
- `1 <= board.length <= 200`
- `1 <= board[i].length <= 200`
- `1 <= word.length <= 10^3`

**代码:**

```c++
class Solution {
public:
    bool flag = false;
    bool exist(vector<vector<char>>& board, string word) {
        for(int i = 0; i < board.size(); i++){
            for(int j = 0; j < board[0].size(); j++){
                backtrack(board, word, 0, i , j); // 从二维表格的每一个格子出发
                if(flag)return true;
            }
        }
        return flag;
    }
private:
    void backtrack(vector<vector<char>>& board, string& word, int wordIndex, int x, int y){
        if( board[x][y] != word[wordIndex] || flag) // 当前位的字母不相等，此路不通
            return ;
        if(word.size() - 1  == wordIndex){ // 最后一个字母也相等, 返回成功
            flag = true;
            return;
        }
        char tmp = board[x][y]; 
        board[x][y] = 0; // 避免该位重复使用 它很重要
        wordIndex++;
        if(x > 0)
            backtrack(board, word, wordIndex, x - 1, y); // 往上走
        if(y > 0)
            backtrack(board, word, wordIndex, x, y - 1); // 往左走
        if(x < board.size() - 1)
            backtrack(board, word, wordIndex, x + 1, y);// 往下走
        if(y < board[0].size() - 1)
            backtrack(board, word, wordIndex, x, y + 1); // 往右走
        board[x][y] = tmp; // 如果都不通，则回溯上一状态
    }
};
```

 [90. 子集 II](https://leetcode-cn.com/problems/subsets-ii/)

给定一个可能包含重复元素的整数数组 ***nums***，返回该数组所有可能的子集（幂集）。

**说明：**解集不能包含重复的子集。

**示例:**

```
输入: [1,2,2]
输出:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```

**代码:**

```c++
class Solution {
public:
    vector<int> flag;
    vector<vector<int>> res;
    void backTrace(vector<int>& nums , int pos , vector<int>& tmp){
        if(tmp.size() <= nums.size()){
            res.push_back(tmp);
        }else{
            return;
        }
        for(int i = pos ; i < nums.size() ; i++){
            if(i > 0 && nums[i] == nums[i-1] && flag[i] && flag[i-1])// 去重关键
                continue;
            tmp.push_back(nums[i]);
            flag[i] = 0;
            backTrace(nums , i+1 , tmp);
            flag[i] = 1;
            tmp.pop_back();
        }
    }
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
       vector<int> tmp;
       sort(nums.begin() , nums.end());// 必须排序
       flag.resize(nums.size() , 0);
       backTrace(nums , 0 , tmp);
       return res;
    }
};
```

# LeetCode 5-7

#### [93. 复原IP地址](https://leetcode-cn.com/problems/restore-ip-addresses/)

给定一个只包含数字的字符串，复原它并返回所有可能的 IP 地址格式。

有效的 IP 地址正好由四个整数（每个整数位于 0 到 255 之间组成），整数之间用 `'.' `分隔。

 

**示例:**

```
输入: "25525511135"
输出: ["255.255.11.135", "255.255.111.35"]
```

**代码:**

```c++
class Solution {
public:
    vector<string> res;
    void backTrace(string& s , string& tmp , int pos , int nSize){
        // 一个网址包含四个部分  完整的网址必须比字符串多三个. , 
        // 所以可以通过生成的字符串的长度来判断是否为合法的网址
        if(tmp.size() == s.size() + 3 && nSize == 4 ){
            res.push_back(tmp);
            return;
        }
        if(pos > s.size()-1 || nSize >= 4)  // 如果pos超过s的范围，或者超出4的范畴 ，则为异常
            return;
        if(!tmp.empty())
            tmp += ".";
        string tt = tmp;
        for(int i = 1; i <=3 ; i++){
            if(pos <= s.size()-i){
                string t1 = s.substr(pos , i);
                if(t1.size() > 1 && t1[0] == '0')// 如果截取的字符串为012 02，非法
                    continue;
                int n1 = atoi(t1.c_str());
                if(n1 <= 255){
                    tmp += t1;
                    backTrace(s , tmp , pos+i , nSize+1);
                    tmp = tt;// 字符串复原
                }
            }
        }
    }
    vector<string> restoreIpAddresses(string s) {
        string tmp;
        if(s.empty())
            return res;
        backTrace(s , tmp , 0 , 0);// 回溯法
        return res;
    }
};
```

# LeetCode 5-8

#### [131. 分割回文串](https://leetcode-cn.com/problems/palindrome-partitioning/)

给定一个字符串 *s*，将 *s* 分割成一些子串，使每个子串都是回文串。

返回 *s* 所有可能的分割方案。

**示例:**

```
输入: "aab"
输出:
[
  ["aa","b"],
  ["a","a","b"]
]
```

**代码:**

```c++
class Solution {
public:
    int sLen = 0 ;
    vector< vector<string> > res;
    bool isBackStr(int i , int j , string& s){
        if (j < i) return true;
        if (s[i++] == s[j--]) 
            return isBackStr(i, j , s);
        else 
            return false;
    }
    void backTrace(vector<string>& tmp , string& s , int pos  ){
        if(pos >= sLen){//如果遍历到了最后，说明符合要求
            res.push_back(tmp);
            return;
        }
        for(int i = pos ; i < sLen ; i++){// 这里的for循环是指下一个分隔符可以落在哪里
            if(isBackStr(pos , i , s)){// 如果当前子串为回文子串，则继续
                tmp.push_back(s.substr(pos , i-pos+1));
                backTrace(tmp , s , i+1);
                tmp.pop_back();
            }
        }
    }
    vector<vector<string>> partition(string s) {
        sLen = s.size();
        if(sLen == 0) return res;
        vector<string> tmp;
        backTrace(tmp , s , 0);
        return res;
    }
};
```

#### [216. 组合总和 III](https://leetcode-cn.com/problems/combination-sum-iii/)

找出所有相加之和为 ***n*** 的 ***k\*** 个数的组合***。\***组合中只允许含有 1 - 9 的正整数，并且每种组合中不存在重复的数字。

**说明：**

- 所有数字都是正整数。
- 解集不能包含重复的组合。 

**示例 1:**

```
输入: k = 3, n = 7
输出: [[1,2,4]]
```

**示例 2:**

```
输入: k = 3, n = 9
输出: [[1,2,6], [1,3,5], [2,3,4]]
```

**代码:**

```c++
vector<vector<int>> res;
int nSum = 0 , nK = 0 ;
void backTrace(vector<int>& tmp , int sum , int pos){
    if(tmp.size() == nK  && sum == nSum){
        res.push_back(tmp);
        return;
    }
    if(tmp.size() > nK  || sum > nSum)
    	return;
    for(int i = pos ; i <= 9 ; i++){
        if(tmp.size() <= nK && sum + i <= nSum){
            tmp.push_back(i);
            backTrace(tmp , sum + i , i+1);
            tmp.pop_back();
        }
    }
}
vector<vector<int>> combinationSum3(int k, int n) {
    nSum = n ;
    nK = k;
    vector<int> tmp;
    backTrace(tmp , 0 , 1);
    return res;
}
```

#### [526. 优美的排列](https://leetcode-cn.com/problems/beautiful-arrangement/)

假设有从 1 到 N 的 **N** 个整数，如果从这 **N** 个数字中成功构造出一个数组（**这个数组的长度为N**），使得数组的第 **i** 位 (1 <= i <= N) 满足如下两个条件中的一个，我们就称这个数组为一个优美的排列。条件：

1. 第 **i** 位的数字能被 **i** 整除
2. **i** 能被第 **i** 位上的数字整除

现在给定一个整数 N，请问可以构造多少个优美的排列？

**示例1:**

```
输入: 2
输出: 2
解释: 

第 1 个优美的排列是 [1, 2]:
  第 1 个位置（i=1）上的数字是1，1能被 i（i=1）整除
  第 2 个位置（i=2）上的数字是2，2能被 i（i=2）整除

第 2 个优美的排列是 [2, 1]:
  第 1 个位置（i=1）上的数字是2，2能被 i（i=1）整除
  第 2 个位置（i=2）上的数字是1，i（i=2）能被 1 整除
```

**代码:**

```c++
void backTrace(int pos , int N , vector<int>& vecFlag , int &res ){
    if(pos > N){//pos是从1开始的
        res++;
        return ;
    }
    for(int i = 1 ; i <= N ; i++){
        if(vecFlag[i-1] == 1 && (pos%i == 0 || i%pos == 0)){
            vecFlag[i-1] = 0 ;// i是从1开始的
            backTrace(pos+1 , N , vecFlag , res);
            vecFlag[i-1] = 1;
        }
    }
}
int countArrangement(int N) {
    vector<int> vecFlag(N , 1);//用于记录是否被使用过
    int res = 0;
    backTrace(1 , N , vecFlag , res);
    return res;

}
```

#### [842. 将数组拆分成斐波那契序列](https://leetcode-cn.com/problems/split-array-into-fibonacci-sequence/)

给定一个数字字符串 `S`，比如 `S = "123456579"`，我们可以将它分成斐波那契式的序列 `[123, 456, 579]`。

形式上，斐波那契式序列是一个非负整数列表 `F`，且满足：

- `0 <= F[i] <= 2^31 - 1`，（也就是说，每个整数都符合 32 位有符号整数类型）；
- `F.length >= 3`；
- 对于所有的`0 <= i < F.length - 2`，都有 `F[i] + F[i+1] = F[i+2]` 成立。

另外，请注意，将字符串拆分成小块时，每个块的数字一定不要以零开头，除非这个块是数字 0 本身。

返回从 `S` 拆分出来的所有斐波那契式的序列块，如果不能拆分则返回 `[]`。

**示例 1：**

```
输入："123456579"
输出：[123,456,579]
```

**示例 2：**

```
输入: "11235813"
输出: [1,1,2,3,5,8,13]
```

**示例 3：**

```
输入: "112358130"
输出: []
解释: 这项任务无法完成。
```

**示例 4：**

```
输入："0123"
输出：[]
解释：每个块的数字不能以零开头，因此 "01"，"2"，"3" 不是有效答案。
```

**示例 5：**

```
输入: "1101111"
输出: [110, 1, 111]
解释: 输出 [11,0,11,11] 也同样被接受。
```

**提示：**

1. `1 <= S.length <= 200`
2. 字符串 `S` 中只含有数字。

**代码：**

```c++
bool backTrace(string& s , int pos , vector<int> &res){
    if(res.size() >= 3 && pos == s.size()){
        return true ;
    }
    for(int i = pos ; i < s.size() ; i++ ){
        if(s[pos]== '0' && i-pos+1 > 1)//过滤 01  0123  这样的字符串 同时考虑0的情况
            continue;
        long long  nT = atoll(s.substr(pos , i-pos+1).c_str());
        if(nT <= INT_MAX){
            // res[n-1] + res[n-2]  可能超出int的范围 所以用减法
            if(res.size() < 2 || nT - res[res.size()-1] == res[res.size() - 2]){
                res.push_back(nT);
                if(backTrace(s , i+1 , res)) // 需要明白为啥第二个参数为i+1
                    return true;
                res.pop_back();
            }
        }
    }
    return false;
}
vector<int> splitIntoFibonacci(string S) {
    vector<int> res;
    backTrace(S , 0 , res);
    return res;
}
```

#### [306. 累加数](https://leetcode-cn.com/problems/additive-number/)

累加数是一个字符串，组成它的数字可以形成累加序列。

一个有效的累加序列必须**至少**包含 3 个数。除了最开始的两个数以外，字符串中的其他数都等于它之前两个数相加的和。

给定一个只包含数字 `'0'-'9'` 的字符串，编写一个算法来判断给定输入是否是累加数。

**说明:** 累加序列里的数不会以 0 开头，所以不会出现 `1, 2, 03` 或者 `1, 02, 3` 的情况。

**示例 1:**

```
输入: "112358"
输出: true 
解释: 累加序列为: 1, 1, 2, 3, 5, 8 。1 + 1 = 2, 1 + 2 = 3, 2 + 3 = 5, 3 + 5 = 8
```

**示例 2:**

```
输入: "199100199"
输出: true 
解释: 累加序列为: 1, 99, 100, 199。1 + 99 = 100, 99 + 100 = 199
```

**进阶:**
你如何处理一个溢出的过大的整数输入?

**代码:**

```c++
bool backTrace(string& num , int pos , vector<long>& res){
    if(pos == num.size() && res.size() >= 3){
        return true;
    }
    for(int i = pos ; i < num.size() ; i++){
        if(num[pos] == '0' && i-pos+1 >1)
            continue;
        long long nTmp = atoll(num.substr(pos , i-pos+1).c_str());
        if(res.size() < 2 || nTmp - res[res.size()-1] == res[res.size()-2]){
            res.push_back(nTmp);
            if(backTrace(num , i+1 , res))
                return true;
            res.pop_back();
        }
    }
    return false;
}
bool isAdditiveNumber(string num) {
    vector<long> res;
    return backTrace(num , 0 , res);
}
```

# LeetCode 5-9

#### [1079. 活字印刷](https://leetcode-cn.com/problems/letter-tile-possibilities/)

你有一套活字字模 `tiles`，其中每个字模上都刻有一个字母 `tiles[i]`。返回你可以印出的非空字母序列的数目。

**注意：**本题中，每个活字字模只能使用一次。

 

**示例 1：**

```
输入："AAB"
输出：8
解释：可能的序列为 "A", "B", "AA", "AB", "BA", "AAB", "ABA", "BAA"。
```

**示例 2：**

```
输入："AAABBC"
输出：188
```

**提示：**

1. `1 <= tiles.length <= 7`
2. `tiles` 由大写英文字母组成

**代码：**

```c++
unordered_map<string , int> unMap;
vector<int> vecFlag;
void backTrace(int pos , string& tiles , string& tt){
    if(pos <= tiles.size() && tt.size() >0 && unMap.find(tt) == unMap.end())
        unMap[tt] = 1;
    for(int i = 0 ; i < vecFlag.size() ; i++){
        if(vecFlag[i] == 1){
            string tmp =tt ;
            tt += tiles[i];
            vecFlag[i] = 0 ;
            backTrace(pos+1 , tiles , tt);
            vecFlag[i] = 1 ;
            tt = tmp;
        }
    }
}
int numTilePossibilities(string tiles) {
    vecFlag.resize(tiles.size() , 1);
    string tmp;
    backTrace(0 , tiles , tmp);
    return unMap.size();
}
```

 # LeetCode 5-11

#### [1239. 串联字符串的最大长度](https://leetcode-cn.com/problems/maximum-length-of-a-concatenated-string-with-unique-characters/)

给定一个字符串数组 `arr`，字符串 `s` 是将 `arr` 某一子序列字符串连接所得的字符串，如果 `s` 中的每一个字符都只出现过一次，那么它就是一个可行解。

请返回所有可行解 `s` 中最长长度。

 

**示例 1：**

```
输入：arr = ["un","iq","ue"]
输出：4
解释：所有可能的串联组合是 "","un","iq","ue","uniq" 和 "ique"，最大长度为 4。
```

**示例 2：**

```
输入：arr = ["cha","r","act","ers"]
输出：6
解释：可能的解答有 "chaers" 和 "acters"。
```

**示例 3：**

```
输入：arr = ["abcdefghijklmnopqrstuvwxyz"]
输出：26
```

 

**提示：**

- `1 <= arr.length <= 16`
- `1 <= arr[i].length <= 26`
- `arr[i]` 中只含有小写英文字母

**代码：**

```c++
class Solution {
public:
    int maxLength(vector<string>& arr) {
        vector<int> m(26, 0);  
        return dfs(arr, 0, m);  
    }
    int dfs(vector<string>& arr, int childIndex, vector<int> m) {
        if (childIndex == arr.size()) {
            return 0;
        }
        // 再定义一个状态表来保存加入当前字符串之后的状态
        vector<int> t(m); 
        if (isUnique(arr[childIndex], t)) {
            int curLen = arr[childIndex].length();
            int len1 = curLen + dfs(arr, childIndex+1, t);
            int len2 = dfs(arr, childIndex+1, m);
            return max(len1, len2);
        }
        return dfs(arr, childIndex+1, m);
    }
    /*
    判断加入字符串s后，是否满足不含相同字符
    注意对于哈希表传入的是引用
    */
	bool isUnique(string& s, vector<int>& t) {
        bool flag = true;
        int pos = -1 ;
	    for (int i = 0; i < s.length(); i++) {
	        t[s[i]-'a']++;
            if(t[s[i]-'a'] > 1){
                pos = i;
                flag = false;
            }
	    }
	    for(int i = 0 ; i <= pos ; i++)
            t[s[i]-'a']--;
	    return flag;
	}

};
```

 #### [1367. 二叉树中的列表](https://leetcode-cn.com/problems/linked-list-in-binary-tree/)

难度中等33

给你一棵以 `root` 为根的二叉树和一个 `head` 为第一个节点的链表。

如果在二叉树中，存在一条一直向下的路径，且每个点的数值恰好一一对应以 `head` 为首的链表中每个节点的值，那么请你返回 `True` ，否则返回 `False` 。

一直向下的路径的意思是：从树中某个节点开始，一直连续向下的路径。

 

**示例 1：**

**![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/29/sample_1_1720.png)**

```
输入：head = [4,2,8], root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]
输出：true
解释：树中蓝色的节点构成了与链表对应的子路径。
```

**示例 2：**

**![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/29/sample_2_1720.png)**

```
输入：head = [1,4,2,6], root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]
输出：true
```

**示例 3：**

```
输入：head = [1,4,2,6,8], root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]
输出：false
解释：二叉树中不存在一一对应链表的路径。
```

 

**提示：**

- 二叉树和链表中的每个节点的值都满足 `1 <= node.val <= 100` 。
- 链表包含的节点数目在 `1` 到 `100` 之间。
- 二叉树包含的节点数目在 `1` 到 `2500` 之间。

**代码：**

```c++
// 方法：枚举 ，枚举二叉树每一个节点向下的路径中是否右和链表相匹配
// 一共四种情况：
// 1：链表匹配完 ， 匹配成功 ， 返回true
// 2: 二叉树访问到空节点，匹配失败 ， 返回false
// 3:当前匹配的值与链表中值不相等，匹配失败，返回false
// 4:匹配  继续往下遍历
bool backTrace(ListNode *head , TreeNode *root){
    // 从一个节点开始往下遍历
    if(head == NULL)
        return true;
    if(root == NULL)
        return false;
    if(head->val != root->val)
        return false;
    return backTrace(head->next , root->left) || backTrace(head->next , root->right);
}
// 递归的方式
bool isSubPath(ListNode* head, TreeNode* root) {
    if(head == NULL || root == NULL)
        return false;
    // 从根节点往下遍历  从左节点和右节点往下遍历
    return backTrace(head , root) || isSubPath(head , root->left) || isSubPath(head , root->right);
}

// 使用栈来模拟递归的方式
bool isSubPath(ListNode* head, TreeNode* root) {
    bool res = false;
    if(root == NULL)return res;
    stack<TreeNode*> st;
    st.push(root);
    while(!st.empty() && !res){
        TreeNode *tmp = st.top();
        st.pop();
        if(tmp->val == head->val)
            res = backTrace(head , tmp);
        if(tmp->left)
            st.push(tmp->left);
        if(tmp->right)
            st.push(tmp->right);
    }
    return res;
}
```

#### [102. 二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

难度中等516

给你一个二叉树，请你返回其按 **层序遍历** 得到的节点值。 （即逐层地，从左到右访问所有节点）。

 

**示例：**
二叉树：`[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

返回其层次遍历结果：

```
[
  [3],
  [9,20],
  [15,7]
]
```

**代码：**

```c++
//此题依然采用前序遍历的方式，唯一的改变是同一层的数据需要放到同一个vector中，因此针对每一层增加一个层号即可
void backTrace(TreeNode *root , int level , vector<vector<int>>& res){
    if(root == NULL) return ;
    if(res.size() < level)
        res.resize(level);
    res.at(level-1).push_back(root->val);
    backTrace(root->left , level+1 , res);
    backTrace(root->right , level+1 , res);
}
// 递归的方式
vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> res;
    backTrace(root , 1 , res);
    return res;
}
// 栈的方式
vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> res;
    if(root == NULL) return res;
    stack<pair<TreeNode* , int>> st;
    st.push(make_pair(root , 1));
    while(!st.empty()){
        TreeNode* tmp = st.top().first;
        int lev = st.top().second;
        st.pop();
        if(res.size() < lev)
            res.resize(lev);
        res.at(lev-1).push_back(tmp->val);
        if(tmp->right)
            st.push(make_pair(tmp->right , lev+1));
        if(tmp->left)
            st.push(make_pair(tmp->left , lev+1));
    }
    return res;
}
```

 [103. 二叉树的锯齿形层次遍历](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)

难度中等191

给定一个二叉树，返回其节点值的锯齿形层次遍历。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。

例如：
给定二叉树 `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

返回锯齿形层次遍历如下：

```
[
  [3],
  [20,9],
  [15,7]
]
```

**代码：**

```c++
vector<vector<int>> res;
vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
    DFS(root , 1);
    return res;
}

void DFS(TreeNode* root , int level ){
    if(root == NULL)
        return ;
    if(level > res.size())
        res.resize(level);
    //根据level 改变数据插入vector的顺序
    if(level % 2 == 0){
        res[level - 1].insert(res[level -1].begin() , root->val);
    }else{
        res[level - 1].push_back(root->val);
    }
    // 不能根据level ，改变调用DFS的顺序
    DFS(root->left , level+1);
    DFS(root->right , level+1);

}
```

#### [105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

难度中等515

根据一棵树的前序遍历与中序遍历构造二叉树。

**注意:**
你可以假设树中没有重复的元素。

例如，给出

```
前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
```

返回如下的二叉树：

```
    3
   / \
  9  20
    /  \
   15   7
```

**代码：**

```c++
/**

**/
TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
    int pos = 0;
    return DFS(preorder , pos , inorder , 0 , inorder.size()-1);
}

TreeNode* DFS(vector<int>& preorder , int& pos , vector<int>& inorder , int left , int right){
    if(pos >= preorder.size())
        return NULL;
    int i = left;
    for(; i <= right ; i++){
        if(preorder[pos] == inorder[i])
            break;
    }
    TreeNode* tmp = new TreeNode(preorder[pos]);
    if(i >= left +1){
        tmp->left = DFS(preorder , ++pos , inorder , left , i-1);
    }
    if(i <= right -1){
        tmp->right = DFS(preorder , ++pos , inorder , i+1 , right);
    }
    return tmp;
}
```