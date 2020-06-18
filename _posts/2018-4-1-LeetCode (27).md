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

 