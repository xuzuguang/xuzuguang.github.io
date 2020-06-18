# LeetCode：4-22

#### [447. 回旋镖的数量](https://leetcode-cn.com/problems/number-of-boomerangs/)

给定平面上 *n* 对不同的点，“回旋镖” 是由点表示的元组 `(i, j, k)` ，其中 `i` 和 `j` 之间的距离和 `i` 和 `k` 之间的距离相等（**需要考虑元组的顺序**）。

找到所有回旋镖的数量。你可以假设 *n* 最大为 **500**，所有点的坐标在闭区间 **[-10000, 10000]** 中。

**示例:**

```
输入:
[[0,0],[1,0],[2,0]]

输出:
2

解释:
两个回旋镖为 [[1,0],[0,0],[2,0]] 和 [[1,0],[2,0],[0,0]]
```

#### [447. 回旋镖的数量](https://leetcode-cn.com/problems/number-of-boomerangs/)

难度简单85

给定平面上 *n* 对不同的点，“回旋镖” 是由点表示的元组 `(i, j, k)` ，其中 `i` 和 `j` 之间的距离和 `i` 和 `k` 之间的距离相等（**需要考虑元组的顺序**）。

找到所有回旋镖的数量。你可以假设 *n* 最大为 **500**，所有点的坐标在闭区间 **[-10000, 10000]** 中。

**代码:**

```c++
int numberOfBoomerangs(vector<vector<int>>& points) {
    int num=0 , dis=0;
    unordered_map<int,int> n;
    /**	
    	5个点 以a1为原点 存在a1a2 = a1a3 = a1a4 那么存在回旋镖个数为2*3 = 2*(2-1) + 2(3-1)
    **/
    for(int i=0;i<points.size();++i){
        n.clear();
        for(int j=0;j<points.size();++j){
            if(i==j)continue;
            dis=pow(points[i][0]-points[j][0],2)+pow(points[i][1]-points[j][1],2);
            n[dis]++;//同长度线段计数
            if(n[dis]>1)
            	num += 2*(n[dis]-1);//一次加入等长线段,增加2*(n-1)个回旋镖
        }
    }
    return num;
}
```

#### [500. 键盘行](https://leetcode-cn.com/problems/keyboard-row/)

给定一个单词列表，只返回可以使用在键盘同一行的字母打印出来的单词。键盘如下图所示。

 

![American keyboard](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/keyboard.png)

 

**示例：**

```
输入: ["Hello", "Alaska", "Dad", "Peace"]
输出: ["Alaska", "Dad"]
```

**代码：**

```c++
vector<string> findWords(vector<string>& words) {
    string s[3] = {
        "qwertyuiopQWERTYUIOP",
        "ASDFGHJKLasdfghjkl",
        "ZXCVBNMzxcvbnm"
    };
    unordered_map<char,int> map1;
    for(int i = 0;i < 3;i ++) {
        for(int j = 0;j < s[i].length(); j ++)
            map1[s[i][j]] = i;
    }
    vector<string> vec;//存放最终结果
    for(int i=0;i<words.size();i++){
        int k=map1[words[i][0]];//获取首字母所在的行
        for(int j = 1 ; j < words[i].size() ; j++){
            if(map1[words[i][j]] != k){
                k = -1 ;
                break;
            }
        }
        if(k >= 0)
            vec.push_back(words[i]);
    }
    return vec;
}
```

#### [463. 岛屿的周长](https://leetcode-cn.com/problems/island-perimeter/)

难度简单176

给定一个包含 0 和 1 的二维网格地图，其中 1 表示陆地 0 表示水域。

网格中的格子水平和垂直方向相连（对角线方向不相连）。整个网格被水完全包围，但其中恰好有一个岛屿（或者说，一个或多个表示陆地的格子相连组成的岛屿）。

岛屿中没有“湖”（“湖” 指水域在岛屿内部且不和岛屿周围的水相连）。格子是边长为 1 的正方形。网格为长方形，且宽度和高度均不超过 100 。计算这个岛屿的周长。

 

**示例 :**

```
输入:
[[0,1,0,0],
 [1,1,1,0],
 [0,1,0,0],
 [1,1,0,0]]

输出: 16

解释: 它的周长是下面图片中的 16 个黄色的边：
```

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/island.png)



**代码 :**

```c++
int islandPerimeter(vector<vector<int>>& grid) {
    int nLen = grid.size();
    int mLen = grid[0].size();
    int res = 0 ;
    for(int i = 0 ; i < nLen ; i++){
        for(int j = 0 ; j < mLen ; j++){
            if(grid[i][j] == 0)
                continue;
            res += 4;
            if(i > 0 && grid[i-1][j] == 1)
                res -= 2;
            if(j > 0 && grid[i][j-1] == 1)
                res -= 2;
        }
    }
    return res;
}
```

