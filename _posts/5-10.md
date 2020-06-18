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

 