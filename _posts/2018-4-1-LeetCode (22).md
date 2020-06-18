# LeetCode 4-25

#### [811. 子域名访问计数](https://leetcode-cn.com/problems/subdomain-visit-count/)

一个网站域名，如"discuss.leetcode.com"，包含了多个子域名。作为顶级域名，常用的有"com"，下一级则有"leetcode.com"，最低的一级为"discuss.leetcode.com"。当我们访问域名"discuss.leetcode.com"时，也同时访问了其父域名"leetcode.com"以及顶级域名 "com"。

给定一个带访问次数和域名的组合，要求分别计算每个域名被访问的次数。其格式为访问次数+空格+地址，例如："9001 discuss.leetcode.com"。

接下来会给出一组访问次数和域名组合的列表`cpdomains` 。要求解析出所有域名的访问次数，输出格式和输入格式相同，不限定先后顺序。

```
示例 1:
输入: 
["9001 discuss.leetcode.com"]
输出: 
["9001 discuss.leetcode.com", "9001 leetcode.com", "9001 com"]
说明: 
例子中仅包含一个网站域名："discuss.leetcode.com"。按照前文假设，子域名"leetcode.com"和"com"都会被访问，所以它们都被访问了9001次。
示例 2
输入: 
["900 google.mail.com", "50 yahoo.com", "1 intel.mail.com", "5 wiki.org"]
输出: 
["901 mail.com","50 yahoo.com","900 google.mail.com","5 wiki.org","5 org","1 intel.mail.com","951 com"]
说明: 
按照假设，会访问"google.mail.com" 900次，"yahoo.com" 50次，"intel.mail.com" 1次，"wiki.org" 5次。
而对于父域名，会访问"mail.com" 900+1 = 901次，"com" 900 + 50 + 1 = 951次，和 "org" 5 次。
```

**代码：**

```c++
class Solution {
public:
    vector<string> subdomainVisits(vector<string>& cpdomains) {
        unordered_map<string , int> unMap;
        for(int i = 0 ; i < cpdomains.size() ; i++ ){
            string str = cpdomains[i];
            int pos = str.find(' ');
            int num =atoi(str.substr(0 , pos).c_str());
            str = str.substr(pos , str.size()-pos);
            unMap[str] +=num ;
            pos = str.find('.');
            while(pos != string::npos){
                int n = pos;
                string tmp = str.substr(n+1 , str.size()-n);
                std::cout<< tmp<<std::endl;
                unMap[tmp] +=num;
                pos = str.find('.',pos+1);
            }
        }
        vector<string> res;
        for(auto i:unMap){
            string num = to_string(i.second);
            cout<<num <<std::endl;
            res.push_back(num + ' ' +);
            res.push_back(i.first);            
        }
        return res;
    }
};
```

#### [3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

给定一个字符串，请你找出其中不含有重复字符的 **最长子串** 的长度。

**示例 1:**

```
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

**示例 2:**

```
输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

**示例 3:**

```
输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

**代码:**

```c++
int lengthOfLongestSubstring(string s) {
    // i记录开始位置   j记录结束位置
    int res = 0, i = 0, j = 0;
    vector<int> idx(128, -1);
    while(j < s.size())
    {
        // 正常情况下， 不重复 则IDX[s[j]] = -1
        if(idx[s[j]] >= i)//说明 此处为重复元素
        {
            res = max(res, j - i);//更新最大值
            i = idx[s[j]] + 1;//更新开始位置         
        }
        idx[s[j++]] = j;//记录字符s[j]对应下标
    }
    res = max(res, j - i);
    return res;
}

int lengthOfLongestSubstring(string s) {
    int res = 0;
    if(s.empty()) return res;
    unordered_map<char , int> unMap;
    int left = 0 , right = 0 , pos = 0;
    // left 记录开始位置  right记录结束位置
    while(pos < s.size()){
        // 使用hashmap判断数据是否重复
        if(unMap.find(s[pos]) != unMap.end() && unMap[s[pos]] >= left){
            res = max(right - left , res);
            left = unMap[s[pos]] +1;
        }
        unMap[s[pos]] = pos;
        right++;
        pos++;
    }
    res = max(right-left , res);
    return res;
}
```

