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

