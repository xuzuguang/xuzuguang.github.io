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

 