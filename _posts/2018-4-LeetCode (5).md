# LeetCode：4-21

#### [两数之和](https://leetcode-cn.com/problems/two-sum/)

给定一个整数数组 `nums` 和一个目标值 `target`，请你在该数组中找出和为目标值的那 **两个** 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。

 

**示例:**

```
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

**代码:**

```c++
vector<int> twoSum(vector<int>& nums, int target) {
	vector<int> result;
    result.resize(2);
    unordered_map<int,int> ss;
    for(int i = 0 ; i<nums.size() ; i++){
		int tmp = target - nums[i];
        if(ss.find(tmp) != ss.end()){
        	result[0] =ss[tmp];// 注意两个下标的顺序 是反过来的
            result[1] =i;
            break;
		}
		// 遍历的过程中同时更新hashMap
        ss[nums[i]] = i;
	}
    return result;
}
```



####  [只出现一次的数字](https://leetcode-cn.com/problems/single-number/)

给定一个**非空**整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

**说明：**

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

**示例 1:**

```
输入: [2,2,1]
输出: 1
```

**示例 2:**

```
输入: [4,1,2,1,2]
输出: 4
```

**代码:**

~~~~c++
int singleNumber(vector<int>& nums) {
    int ans=0;
    /**
    a^b = b^a  a^a = 0  0^a = a
    nums[0]^nums[1]^nums[2]^......nums[n-1]  (其中 一样的元素相乘为0  只会剩余剩下的不重复的数字)
    **/
    for(int i=0;i<nums.size();i++)
        ans^=nums[i];
    return ans;
}
~~~~



#### [290. 单词规律](https://leetcode-cn.com/problems/word-pattern/)

难度简单146

给定一种规律 `pattern` 和一个字符串 `str` ，判断 `str` 是否遵循相同的规律。

这里的 **遵循** 指完全匹配，例如， `pattern` 里的每个字母和字符串 `str` 中的每个非空单词之间存在着双向连接的对应规律。

**示例1:**

```
输入: pattern = "abba", str = "dog cat cat dog"
输出: true
```

**示例 2:**

```
输入:pattern = "abba", str = "dog cat cat fish"
输出: false
```

**示例 3:**

```
输入: pattern = "aaaa", str = "dog cat cat dog"
输出: false
```

**代码:**

```c++
bool wordPattern(string pattern, string str) {
    vector<int> res1 , res2;
    unordered_map<char , int> unMap1;// 主要还是利用哈希表的find函数（O（1））
    unordered_map<string , int > unMap2;
    /**
    	1：将aaaa转换为0000  abba转换为0110  
    	2：将dog cat cat dog 转换为 0110 
    	3：比较两个数组
    **/
    for(int i = 0 ; i < pattern.size() ; i++){
        if(unMap1.find(pattern[i]) == unMap1.end())
            unMap1[pattern[i]] = i;
        res1.push_back( unMap1[pattern[i]]);
    }
    int start = 0 , len = 0  , nSize = 0;
    for(int i = 0 ; i < str.size() ; i++){
        if(str[i] == ' '){
            string t = str.substr(start , len);
            if(unMap2.find(t) == unMap2.end())
                unMap2[t] = nSize;
            res2.push_back( unMap2[t]);
            start = i+1;
            len = 0;
            nSize++;
        }else{
            len++;
        }
    }
    string t = str.substr(start , len+1);
    if(unMap2.find(t) == unMap2.end())
        unMap2[t] = nSize;
    res2.push_back(unMap2[t]);
    return res1 == res2;
}
```

#### [最长回文串](https://leetcode-cn.com/problems/longest-palindrome/)

给定一个包含大写字母和小写字母的字符串，找到通过这些字母构造成的最长的回文串。

在构造过程中，请注意区分大小写。比如 `"Aa"` 不能当做一个回文字符串。

**注意:**
假设字符串的长度不会超过 1010。

**示例 1:**

```
输入:
"abccccdd"

输出:
7

解释:
我们可以构造的最长的回文串是"dccaccd", 它的长度是 7。
```

**代码:**

```c++
int longestPalindrome(string s) {
    /**
    统计所有的字母的个数  如果为偶数 最长回文子串长度 +2   为奇数 +n-1  
    **/
    int res = 0 ;
    unordered_map <int , int> unMap;
    for(auto i :s){
        int tmp = (int)i;
        if(unMap.find(tmp) != unMap.end())
            unMap[tmp] += 1;
        else
            unMap[tmp] = 1;
    }
    int nj =0; 
    for(auto i :unMap){
        if(i.second %2 == 0)
            res += i.second;
        else{
            res += i.second -1;
            nj++;
        }
    }
    if(nj>0)// 这个需要知道为啥  比如输入为aaa / aabbb /aa 明确三种输入的不同
        res++;
    return res;
}
```

#### [字符串中的第一个唯一字符](https://leetcode-cn.com/problems/first-unique-character-in-a-string/)



给定一个字符串，找到它的第一个不重复的字符，并返回它的索引。如果不存在，则返回 -1。

**案例:**

```
s = "leetcode"
返回 0.

s = "loveleetcode",
返回 2.
```

**代码**

```c++
int firstUniqChar(string s) {
    /**
    思路：统计所有字符出现的次数
    **/
    if(s.size() == 0)
        return -1;
    int res = INT_MAX ;
    unordered_map<char ,int> unMap;
    vector<int> vecFlag(s.size() , 0);
    for(int i = 0 ; i < s.size() ; i++){
        if(unMap.find(s[i]) != unMap.end()){
            unMap[s[i]] = -1;
        }else{
            unMap[s[i]] = i; 
        }
    }
    for(auto i :unMap){
        if(i.second != -1){
            res = min(i.second,res);
        }
    }
    return res == INT_MAX ? -1 :res;
}
```

#### [找不同](https://leetcode-cn.com/problems/find-the-difference/)

给定两个字符串 ***s*** 和 ***t***，它们只包含小写字母。

字符串 ***t\*** 由字符串 ***s\*** 随机重排，然后在随机位置添加一个字母。

请找出在 ***t*** 中被添加的字母。

 

**示例:**

```
输入：
s = "abcd"
t = "abcde"

输出：
e

解释：
'e' 是那个被添加的字母。
```

**代码:**

```c++
// 思路和 只出现一次的字符 一样  巧用 a^b = b^a  a^a =0  0^a = a
char findTheDifference(string s, string t) {
    char res =t[0] ;
    for(int i = 1 ; i < t.size() ; i++)
        res = res^t[i];
    for(int i = 0 ; i < s.size() ; i++)
        res = res^s[i];
    return res;
}
// 遍历字符串t 记录hashmap
// 遍历字符串s hashmap value-1
// 剩余 值为1 便是结果
char findTheDifference(string s, string t) {
    char res ;
    unordered_map<char , int> unMap;
    for(int i = 0 ; i < t.size() ; i++){
       if(unMap.find(t[i]) == unMap.end())
            unMap[t[i]] = 1;
        else
            unMap[t[i]] = unMap[t[i]] + 1;
    }
    for(int i = 0 ; i < s.size() ; i++){
        if(unMap.find(s[i]) == unMap.end())
            unMap[s[i]] = 1;
        else
            unMap[s[i]] = unMap[s[i]] - 1;
    }
    for(auto i:unMap){
        if(i.second == 1)
            res = i.first;
    }
    return res;
}
```

