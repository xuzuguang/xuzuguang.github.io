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