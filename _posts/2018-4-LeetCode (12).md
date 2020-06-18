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