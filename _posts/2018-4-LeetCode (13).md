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

