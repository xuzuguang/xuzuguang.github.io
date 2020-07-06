---

layout: post
title:  "LeetCode学习之哈希表"
date:   2018-05-1
categories: 算法学习  
tags: 算法学习
---

* content
{:toc}
本文为LeetCode中关于哈希表的题目学习

# 基础总结

　　哈希表由一定大小的连续桶(bucket)构成, 借助散列函数映射到具体某个桶上. 当多个key/value对聚集到同一桶时, 会演化构成一个链表.
　　![img](https://xuzuguang.men//images/posts/hash1.png)
　　哈希表结构有两个重要的参数, **容量大小**(Capacity)和**负载因子**(LoadFactor). 两者的乘积 Capacity * LoadFactor决定了哈希表**rehash**的触发条件.
　　以空间换时间为核心思想, 确保其数据结构的访问时间控制在O(1).
　　哈希表隐藏了内部细节, 而对外的使用则非常的简单. 只需定义key的**hash函数**和**compare函数**即可.

　　hash函数的选择需保证一定散列度, 这样才能充分利用空间. 事实上哈希表的使用者, 往往关注hash函数的**快速计算和高散列度**, 却忽视了其潜在的风险和危机.
　　**1). hash碰撞攻击**
　　 php爆出hash碰撞的攻击漏洞. 其攻击原理, 简单可概括为: **特定的大量key组合, 让哈希表退化为链表访问, 进而拖慢处理速度, 请求堆积, 最终演变为拒绝服务状态**.
　　![img](https://xuzuguang.men//images/posts/hash2.png)
　　具体可参考博文: [PHP哈希表碰撞攻击原理](http://blog.jobbole.com/11516/). 
　　大致的思路是利用php哈希表大小为**2的幂次**, 索引位置计算由 hash(key) % size(bucket) 转变为 hash(key) & (1^n - 1).
　　![img](https://images0.cnblogs.com/blog2015/305284/201504/211531151879040.png)
　　黑客知晓time33算法和hash函数, 可以构造/收集特定的key系列, 使得其hash(key)为**同一桶索引值**. 通过post请求附带, 导致php构造超长链的哈希表.
　　其实如果能理解hash碰撞攻击的原理, 说明其对hash的冲突处理和哈希表本身的数据结构模型有了较深的理解了.

　　**2). 分段锁机制**
　　如果加锁是不可避免的选择, 那能否减少锁冲突的概率呢?
　　答案是肯定的, 不同桶之间的key/value操作彼此互不影响. 在此前提下, 对哈希桶进行分段加锁. 这样**全局锁**就退化为多个**分段锁**, 而锁冲突的概率由于分区的原因, 降低至1/N (N为分段锁个数).
　　![img](https://xuzuguang.men//images/posts/hash3.png)
　　哈希表单key的操作复杂度为O(1), 性能异常优异. 但需要对哈希表进行迭代遍历其所有元素时, 其性能就非常的差. 究其原因是各个key/value对分散在各个桶中, 彼此并无关联. 元素遍历转化为对哈希桶的全扫描.
　　那如果存在这样的需求, 既要保证O(1)的单key操作时间复杂度, 又要让迭代遍历的复杂度为O(n) (n为哈希表的key/value对个数, 不是桶个数), 那如何去实现呢?
　　**1). LinkedHashMap&LRU缓存**
　　是否存在一个**复合数据结构**, 既有Hashmap的特性, 又具备DoubleLinkedList线性遍历的特征?
　　答案是肯定的, 该复合结构就是LinkedHashmap.
　　![img](https://xuzuguang.men//images/posts/hash4.png)
　　注: 依次添加key1, key2, ..., key6, 其按插入顺序构成一个双向列表.
　　一图胜千言, 该图很形象的描述了LinkedHashMap的构成. 可以这么认为: 每个hash entry的结构的基础上, 添加prev和next成员指针用于维护双向列表. 实现就这么简单.
　　在工程实践中, 往往采用**LinkedHashMap的变体来实现带LRU机制的Cache**.
　　简单描述其操作流程:
　　(1). 查询/添加key, 则把该key/value对搁置于LRU队列的末尾
　　(2). 若key/value对个数超过阈值时, 则选择把LRU队列的首元素淘汰掉.
　　模拟key5元素被查询访问, 成为最近的热点, 则内部的链接模型状态转变如下:
![img](https://xuzuguang.men//images/posts/hash5.png)
　　注: key5被访问后, 内部双向队列发生变动, 可以理解为删除key5, 然后再添加key5至末尾.

　　当哈希表中的元素数量超过预定的阈值时, 就会触发rehash过程. 但是若此时的hash表已然很大, rehash的完整过程会阻塞服务很长时间. 这对**高可用高响应的服务是不可想象的灾难**.
　　面对这种情况, 要么避免大数据量的rehash出现, 预先对数据规模进行有效评估. 要么就继续优化哈希的rehash过程.
　　**2). 0/1切换和渐进式rehash**
　　redis的设计者给出了一个很好的解决方案, 就是0/1切换hash表+渐进式rehash.
　　其渐进的rehash把整个迁移过程拆分为多个细粒度的子过程, 同时**0/1切换的hash表共存**.
　　redis的rehash过程分两种方式:
　　• **lazy rehashing**: 在对dict操作的时候附带执行一个slot的rehash
　　• **active rehashing**：定时做个小时间片的rehash

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

 

<img src="D:\JGYData\Typora\images\keyboard.png" alt="American keyboard" style="zoom:50%;" />

 

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

# LeetCode：4-23

#### [575. 分糖果](https://leetcode-cn.com/problems/distribute-candies/)

给定一个**偶数**长度的数组，其中不同的数字代表着不同种类的糖果，每一个数字代表一个糖果。你需要把这些糖果**平均**分给一个弟弟和一个妹妹。返回妹妹可以获得的最大糖果的种类数。

**示例 1:**

```
输入: candies = [1,1,2,2,3,3]
输出: 3
解析: 一共有三种种类的糖果，每一种都有两个。
     最优分配方案：妹妹获得[1,2,3],弟弟也获得[1,2,3]。这样使妹妹获得糖果的种类数最多。
```

**示例 2 :**

```
输入: candies = [1,1,2,3]
输出: 2
解析: 妹妹获得糖果[2,3],弟弟获得糖果[1,1]，妹妹有两种不同的糖果，弟弟只有一种。这样使得妹妹可以获得的糖果种类数最多。
```

**代码 :**

```c++
int distributeCandies(vector<int>& candies) {
    /**
    	妹妹分得的糖为candies.size()/2颗
    	如果总的糖果种类n > candies.size()/2 ，则妹妹获得糖果种类最多为 candies.size()/2
	   	如果总的糖果种类n < candies.size()/2 ，则妹妹获得糖果种类最多为 n
    	所以本题本质为统计糖果的种类。
    **/
    int nSize = candies.size();
    int canCount = 0 ;
    unordered_map<int , int> unMap;
    for(int i = 0 ; i < nSize ; i++){
        if(unMap.find(candies[i]) != unMap.end())
            continue;
		unMap[candies[i]] = 1;
        canCount++;
        if(canCount >= nSize/2)
        	break;
    }
    return canCount > nSize/2 ? nSize/2 : canCount;
}
```

#### [594. 最长和谐子序列](https://leetcode-cn.com/problems/longest-harmonious-subsequence/)

和谐数组是指一个数组里元素的最大值和最小值之间的差别正好是1。

现在，给定一个整数数组，你需要在所有可能的子序列中找到最长的和谐子序列的长度。

**示例 1:**

```
输入: [1,3,2,2,5,2,3,7]
输出: 5
原因: 最长的和谐数组是：[3,2,2,2,3].
```

**说明:** 输入的数组长度最大不超过20,000.

**代码:**

```c++
int findLHS(vector<int>& nums) {
    /**
    1：记录所有的数字在数组冲出现的次数，存储在hash表中
    2：遍历hash表，res = max(i出现的次数 + （i+1）出现的次数 , res)
    **/
    unordered_map<int , int> unMap;
    int res = 0 ;
    for(int i = 0 ; i < nums.size() ; i++){
        unMap[nums[i]] ++;
    }
    for(auto i:unMap){
        if(unMap.find(i.first+1) != unMap.end())
            res = max(i.second + unMap[i.first + 1] , res);
    }
    return res;
}
```

#### [599. 两个列表的最小索引总和](https://leetcode-cn.com/problems/minimum-index-sum-of-two-lists/)

假设Andy和Doris想在晚餐时选择一家餐厅，并且他们都有一个表示最喜爱餐厅的列表，每个餐厅的名字用字符串表示。

你需要帮助他们用**最少的索引和**找出他们**共同喜爱的餐厅**。 如果答案不止一个，则输出所有答案并且不考虑顺序。 你可以假设总是存在一个答案。

**示例 1:**

```
输入:
["Shogun", "Tapioca Express", "Burger King", "KFC"]
["Piatti", "The Grill at Torrey Pines", "Hungry Hunter Steakhouse", "Shogun"]
输出: ["Shogun"]
解释: 他们唯一共同喜爱的餐厅是“Shogun”。
```

**示例 2:**

```
输入:
["Shogun", "Tapioca Express", "Burger King", "KFC"]
["KFC", "Shogun", "Burger King"]
输出: ["Shogun"]
解释: 他们共同喜爱且具有最小索引和的餐厅是“Shogun”，它有最小的索引和1(0+1)。
```

**代码:**

```c++
vector<string> findRestaurant(vector<string>& list1, vector<string>& list2) {
    vector< string > res;
    /**
    1:遍历第一个字符串数组，将字符串放入hash表中。
    2：遍历第二个字符串数组，在hash表中查找对应字符串
    	如果找到，则判断当前索引和是否更小，更小的话，清空vector 插入当前字符串 
    	                              和当前索引一样大，则直接插入当前字符串
    **/
    unordered_map<string , int> unMap;
    for(int i = 0 ; i < list1.size() ; i++){
        if(unMap.find(list1[i]) == unMap.end())
            unMap[list1[i]] = i;
    }
	int sum = INT_MAX ;// 记录 最小索引之和
    for(int i = 0 ; i < list2.size() ; i++){
        if(unMap.find(list2[i]) != unMap.end()){
            if(sum > i + unMap[list2[i]]){
                sum = i + unMap[list2[i]];
                res.clear();
                res.push_back(list2[i]);
            }else if(sum == i + unMap[list2[i]]){
                res.push_back(list2[i]);
            }
        }
    }
    return res;
}
```

#### [645. 错误的集合](https://leetcode-cn.com/problems/set-mismatch/)

集合 `S` 包含从1到 `n` 的整数。不幸的是，因为数据错误，导致集合里面某一个元素复制了成了集合里面的另外一个元素的值，导致集合丢失了一个整数并且有一个元素重复。

给定一个数组 `nums` 代表了集合 `S` 发生错误后的结果。你的任务是首先寻找到重复出现的整数，再找到丢失的整数，将它们以数组的形式返回。

**示例 1:**

```
输入: nums = [1,2,2,4]
输出: [2,3]
```

**注意:**

1. 给定数组的长度范围是 [2, 10000]。
2. 给定的数组是无序的。

**代码:**

```c++
vector<int> findErrorNums(vector<int>& nums) {
    /**
    	遍历数组，将数字出现次数记录在hashmap中，超过1个的数字插入res中。
    	从1遍历到n，在hashmap中找对应值，找不到则将数字插入到res中
    **/
    vector<int> res;
    unordered_map<int , int> unMap;
    for(int i = 0 ; i < nums.size() ; i++){
        unMap[nums[i]]++;
        if(unMap[nums[i]] == 2)
            res.push_back(nums[i]);
    }
    for(int i = 1; i <= nums.size() ; i++){
        if(unMap.find(i) == unMap.end()){
            res.push_back(i);
            break;
        }
    }
    return res;
}
```

#### [690. 员工的重要性](https://leetcode-cn.com/problems/employee-importance/)

给定一个保存员工信息的数据结构，它包含了员工**唯一的id**，**重要度** 和 **直系下属的id**。

比如，员工1是员工2的领导，员工2是员工3的领导。他们相应的重要度为15, 10, 5。那么员工1的数据结构是[1, 15, [2]]，员工2的数据结构是[2, 10, [3]]，员工3的数据结构是[3, 5, []]。注意虽然员工3也是员工1的一个下属，但是由于**并不是直系**下属，因此没有体现在员工1的数据结构中。

现在输入一个公司的所有员工信息，以及单个员工id，返回这个员工和他所有下属的重要度之和。

**示例 1:**

```
输入: [[1, 5, [2, 3]], [2, 3, []], [3, 3, []]], 1
输出: 11
解释:
员工1自身的重要度是5，他有两个直系下属2和3，而且2和3的重要度均为3。因此员工1的总重要度是 5 + 3 + 3 = 11。
```

**注意:**

1. 一个员工最多有一个**直系**领导，但是可以有多个**直系**下属
2. 员工数量不超过2000。

**代码:**

```c++
int getImport(Employee* em , unordered_map<int , Employee*>& uMap){
    //递归方式放下级累加所有的重要度
    int res = em->importance;
    for(int i = 0 ; i < em->subordinates.size() ; i++)
        res += getImport(uMap[em->subordinates[i]] , uMap);
    return res;
}
int getImportance(vector<Employee*> employees, int id) {
    //遍历employee ，根据员工ID存储hashmap 
    unordered_map<int , Employee*> unMap;
    Employee* tmp = NULL;
    for(int i = 0 ; i < employees.size() ; i++){
        unMap[employees[i]->id] = employees[i];
        if(employees[i]->id == id)
            tmp = employees[i];
    }
    if(tmp == NULL)
        return 0;
    return getImport(tmp , unMap);
}
```

# LeetCode： 4-24

#### [748. 最短完整词](https://leetcode-cn.com/problems/shortest-completing-word/)

如果单词列表（`words`）中的一个单词包含牌照（`licensePlate`）中所有的字母，那么我们称之为完整词。在所有完整词中，最短的单词我们称之为最短完整词。

单词在匹配牌照中的字母时不区分大小写，比如牌照中的 `"P"` 依然可以匹配单词中的 `"p"` 字母。

我们保证一定存在一个最短完整词。当有多个单词都符合最短完整词的匹配条件时取单词列表中最靠前的一个。

牌照中可能包含多个相同的字符，比如说：对于牌照 `"PP"`，单词 `"pair"` 无法匹配，但是 `"supper"` 可以匹配。

 

**示例 1：**

```
输入：licensePlate = "1s3 PSt", words = ["step", "steps", "stripe", "stepple"]
输出："steps"
说明：最短完整词应该包括 "s"、"p"、"s" 以及 "t"。对于 "step" 它只包含一个 "s" 所以它不符合条件。同时在匹配过程中我们忽略牌照中的大小写。
```

 

**示例 2：**

```
输入：licensePlate = "1s3 456", words = ["looks", "pest", "stew", "show"]
输出："pest"
说明：存在 3 个包含字母 "s" 且有着最短长度的完整词，但我们返回最先出现的完整词。
```



**代码：**

```c++
bool bComplete(string word , unordered_map<char , int> unMap){
    // 判断单个字符串是否包含hashmap中所有数据
    // 遍历数组 ，如果字符在hashmap中， --
    for(auto i : word){
        if(i >= 'A' &&  i <= 'Z' && unMap.find(tolower(i)) != unMap.end())
            unMap[tolower(i)] -= 1;
        else if(i >= 'a' &&  i <= 'z' && unMap.find(i) != unMap.end())
            unMap[i] -= 1;
    }
    // 遍历hashmap ， map中值 如果 >0 则false
    for(auto i:unMap){
        if(i.second > 0 )
            return false;
    }
    return true;
}
string shortestCompletingWord(string licensePlate, vector<string>& words) {
    unordered_map<char , int> unMap;
    string res ;
    for(char i :licensePlate){
        if(i >= 'A' &&  i <= 'Z')
            unMap[tolower(i)] += 1;
        else if(i >= 'a' && i <= 'z')
            unMap[i] += 1;
    }
    for(auto i :words){
        if(!bComplete(i , unMap)){
            continue;
        }
        if(res.empty() || res.size() > i.size())
            res = i;
    }
    return res;
}
```

#### [771. 宝石与石头](https://leetcode-cn.com/problems/jewels-and-stones/)

 给定字符串`J` 代表石头中宝石的类型，和字符串 `S`代表你拥有的石头。 `S` 中每个字符代表了一种你拥有的石头的类型，你想知道你拥有的石头中有多少是宝石。

`J` 中的字母不重复，`J` 和 `S`中的所有字符都是字母。字母区分大小写，因此`"a"`和`"A"`是不同类型的石头。

**示例 1:**

```
输入: J = "aA", S = "aAAbbbb"
输出: 3
```

**示例 2:**

```
输入: J = "z", S = "ZZ"
输出: 0
```

**代码:**

```c++
int numJewelsInStones(string J, string S) {
    int res = 0 ; 
    unordered_map<char , int> unMap;
    for(char i : J){
        unMap[i] = 1;
    }
    for(char i : S){
        if(unMap.find(i) != unMap.end())
            res++;
    }
    return res;
}
```

#### [705. 设计哈希集合](https://leetcode-cn.com/problems/design-hashset/)

不使用任何内建的哈希表库设计一个哈希集合

具体地说，你的设计应该包含以下的功能

- `add(value)`：向哈希集合中插入一个值。
- `contains(value)` ：返回哈希集合中是否存在这个值。
- `remove(value)`：将给定值从哈希集合中删除。如果哈希集合中没有这个值，什么也不做。


**示例:**

```
MyHashSet hashSet = new MyHashSet();
hashSet.add(1);         
hashSet.add(2);         
hashSet.contains(1);    // 返回 true
hashSet.contains(3);    // 返回 false (未找到)
hashSet.add(2);          
hashSet.contains(2);    // 返回 true
hashSet.remove(2);          
hashSet.contains(2);    // 返回  false (已经被删除)
```


**注意：**

- 所有的值都在 `[0, 1000000]`的范围内。
- 操作的总数目在`[1, 10000]`范围内。
- 不要使用内建的哈希集合库。

**代码:**

```c++
class MyHashSet {
    const int HASH_LEN = 100;
    struct ListNode{ // 链表
        int val;
        ListNode *next;
        ListNode(int x) : val(x), next(NULL) {}
    };
public:
    vector<ListNode*> m_List;
    /** Initialize your data structure here. */
    MyHashSet() {
        m_List.resize(HASH_LEN);
        for(int i = 0 ; i < HASH_LEN ; i++ )
            m_List[i] = new ListNode(-1);
    }
    
    void add(int key) {
        int haval = key % HASH_LEN;
        ListNode *tmp = m_List[haval];
        if(tmp->val == -1){// 空的
            tmp->val = key;
            return;
        }
        while(tmp){
            if(tmp->val == key)
                return;
            if(!(tmp->next)){
                ListNode * node = new ListNode(key);
                tmp->next = node;
                return;
            }
            tmp = tmp->next;
        }
    }
    
    void remove(int key) {
        int haval = key % HASH_LEN;
        ListNode *tmp = m_List[haval];
        if(tmp->val == -1)
            return;
        ListNode* preTmp = NULL;
        while(tmp){
            if(tmp->val == key){
                if(preTmp != NULL)
                    preTmp->next = tmp->next;
                else{
                    if(tmp->next == NULL){
                        ListNode * node = new ListNode(-1);
                        m_List[haval] = node;
                    }else
                        m_List[haval] = tmp->next;
                }
                delete tmp;
                tmp = NULL;
                return;
            }
            preTmp = tmp;
            tmp = tmp->next;
        }
    }
    
    /** Returns true if this set contains the specified element */
    bool contains(int key) {
        int haval = key % HASH_LEN;
        ListNode *tmp = m_List[haval];
        if(tmp->val == -1){
            return false;
        }
        while(tmp){
            if(tmp->val == key) 
                return true;
            tmp = tmp->next;
        }
        return false;
    }
};
```

#### [706. 设计哈希映射](https://leetcode-cn.com/problems/design-hashmap/)

不使用任何内建的哈希表库设计一个哈希映射

具体地说，你的设计应该包含以下的功能

- `put(key, value)`：向哈希映射中插入(键,值)的数值对。如果键对应的值已经存在，更新这个值。
- `get(key)`：返回给定的键所对应的值，如果映射中不包含这个键，返回-1。
- `remove(key)`：如果映射中存在这个键，删除这个数值对。


**示例：**

```
MyHashMap hashMap = new MyHashMap();
hashMap.put(1, 1);          
hashMap.put(2, 2);         
hashMap.get(1);            // 返回 1
hashMap.get(3);            // 返回 -1 (未找到)
hashMap.put(2, 1);         // 更新已有的值
hashMap.get(2);            // 返回 1 
hashMap.remove(2);         // 删除键为2的数据
hashMap.get(2);            // 返回 -1 (未找到) 
```

**代码：**

```c++
class MyHashMap {
    struct ListNode{
        int key;
        int value ;
        ListNode* next;
        ListNode(int n , int m):key(n),value(m),next(NULL){}
    };
    const int HASH_LEN = 100;
public:
    vector<ListNode*> m_vec;
    /** Initialize your data structure here. */
    MyHashMap() {
        m_vec.resize(HASH_LEN);
        for(int i = 0 ; i < HASH_LEN ; i++)
            m_vec[i] = new ListNode(-1 , -1);    
    }
    
    /** value will always be non-negative. */
    void put(int key, int value) {
        int n = key%HASH_LEN;
        ListNode* tmp = m_vec[n];
        if(tmp->key == -1){
            tmp->key = key ;
            tmp->value = value;
            return;
        }
        while(tmp){
            if(tmp->key == key){
                tmp->value = value;
                return;
            }
            if(tmp->next == NULL){
                ListNode* node = new ListNode(key ,value);
                tmp->next = node;
                return;
            }
            tmp = tmp->next;
        }
    }
    
    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    int get(int key) {
        int n = key%HASH_LEN;
        ListNode* tmp = m_vec[n];
        if(tmp->key == -1)
            return -1;
        while(tmp){
            if(tmp->key == key)
                return tmp->value;
            tmp = tmp->next;
        }
        return -1;
    }
    
    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    void remove(int key) {
        int n = key%HASH_LEN;
        ListNode* tmp = m_vec[n];
        if(tmp->key == -1)
            return;
        ListNode* preNode = NULL;
        while(tmp){
            if(tmp->key == key){
                if(preNode == NULL){
                    if(tmp->next == NULL)
                        m_vec[n] = new ListNode(-1 , -1);
                    else
                        m_vec[n] = tmp->next;
                }else{
                    preNode->next = tmp->next;
                }
                delete tmp;
                tmp = NULL;
                return;
            }
            preNode = tmp;
            tmp = tmp->next;
        }
    }
};
```

# LeetCode：4-25

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

# LeetCode： 4-26

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