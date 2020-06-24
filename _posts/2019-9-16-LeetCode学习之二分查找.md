---
layout: post
title:  "LeetCode学习之二分查找"
date:   2018-04-15
categories: 算法学习  
tags: 算法学习


---

* content
{:toc}




本文为LeetCode中关于二分查找的题目学习

## 关于二分查找

二分查找也称折半查找（Binary Search），它是一种效率较高的查找方法，前提是数据结构必须先排好序，可以在数据规模的对数时间复杂度内完成查找。但是，二分查找要求线性表具有有随机访问的特点（例如数组），也要求线性表能够根据中间元素的特点推测它两侧元素的性质，以达到缩减问题规模的效果。

### 二分查找框架

~~~~c++
int binarySearch(int[] nums , int target){
    int left = 0 , right =...;
    while(...){
        int mid = (right + left)/2;

        if(nums[mid] == target){
            ...
		}else if(nums[mid] > target){
            right = ...
        }
    }
    return ...;
}
~~~~

分析二分查找的一个技巧是：不要出现else，而是把所有情况用else if 写清楚，这样可以清楚的展现所有细节。

### 一些问题

1. 为什么while循环中条件是 <=，而不是< ?

   两者是都可以的。前者相当于是闭区间[left , right],而后者相当于左闭右开区间[left ,right),因为如果有nums[length],会出现索引越界。

#### [441. 排列硬币](https://leetcode-cn.com/problems/arranging-coins/)

难度简单58

你总共有 *n* 枚硬币，你需要将它们摆成一个阶梯形状，第 *k* 行就必须正好有 *k* 枚硬币。

给定一个数字 *n*，找出可形成完整阶梯行的总行数。

*n* 是一个非负整数，并且在32位有符号整型的范围内。

**示例 1:**

```
n = 5

硬币可排列成以下几行:
¤
¤ ¤
¤ ¤

因为第三行不完整，所以返回2.
```

**示例 2:**

```
n = 8

硬币可排列成以下几行:
¤
¤ ¤
¤ ¤ ¤
¤ ¤

因为第四行不完整，所以返回3.
```

**代码:**

```c++
int arrangeCoins(int n) {
    int left = 1 , right = n;
    long sum = 0 , mid = 0 ;
    while(left <= right){
        mid = left + (right - left) / 2;
        sum = mid *(mid + 1)/2;
        if(sum == n)
            return mid;
        else if(sum > n)
            right = mid - 1;
        else if(sum < n)
            left = mid + 1;
    }
    mid = sum > n ? mid-1 :mid;   
    return mid;
}
```

#### [剑指 Offer 53 - II. 0～n-1中缺失的数字](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)

难度简单29

一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0～n-1之内。在范围0～n-1内的n个数字中有且只有一个数字不在该数组中，请找出这个数字。

 

**示例 1:**

```
输入: [0,1,3]
输出: 2
```

**示例 2:**

```
输入: [0,1,2,3,4,5,6,7,9]
输出: 8
```

 

**限制：**

```
1 <= 数组长度 <= 10000
```

**代码：**

```c++
int missingNumber(vector<int>& nums) {
    int left = 0, right = nums.size()-1 , mid = 0;
    while(left <= right){
        // 左右 往中间找
        mid = left + (right - left)/2;
        if(mid == nums[mid]) 
            left = mid + 1;
        else 
            right = mid - 1;
    }
    return left;
}
```

#### [704. 二分查找](https://leetcode-cn.com/problems/binary-search/)

难度简单133

给定一个 `n` 个元素有序的（升序）整型数组 `nums` 和一个目标值 `target` ，写一个函数搜索 `nums` 中的 `target`，如果目标值存在返回下标，否则返回 `-1`。


**示例 1:**

```
输入: nums = [-1,0,3,5,9,12], target = 9
输出: 4
解释: 9 出现在 nums 中并且下标为 4
```

**示例 2:**

```
输入: nums = [-1,0,3,5,9,12], target = 2
输出: -1
解释: 2 不存在 nums 中因此返回 -1
```

 

**提示：**

1. 你可以假设 `nums` 中的所有元素是不重复的。
2. `n` 将在 `[1, 10000]`之间。
3. `nums` 的每个元素都将在 `[-9999, 9999]`之间。

**代码:**

```c++
int search(vector<int>& nums, int target) {
    int left = 0 , right = nums.size()-1;
    while(left <= right){
        int mid = left + (right - left)/2;
        if(nums[mid] == target)
            return mid;
        else if(nums[mid] > target)
            right = mid - 1;
        else if(nums[mid] < target)
            left = mid + 1;
    }
    return -1;
}
```

 [1337. 方阵中战斗力最弱的 K 行](https://leetcode-cn.com/problems/the-k-weakest-rows-in-a-matrix/)

难度简单22

给你一个大小为 `m * n` 的方阵 `mat`，方阵由若干军人和平民组成，分别用 1 和 0 表示。

请你返回方阵中战斗力最弱的 `k` 行的索引，按从最弱到最强排序。

如果第 ***i*** 行的军人数量少于第 ***j*** 行，或者两行军人数量相同但 ***i*** 小于 ***j***，那么我们认为第 ***i*** 行的战斗力比第 ***j*** 行弱。

军人 **总是** 排在一行中的靠前位置，也就是说 1 总是出现在 0 之前。

 

**示例 1：**

```
输入：mat = 
[[1,1,0,0,0],
 [1,1,1,1,0],
 [1,0,0,0,0],
 [1,1,0,0,0],
 [1,1,1,1,1]], 
k = 3
输出：[2,0,3]
解释：
每行中的军人数目：
行 0 -> 2 
行 1 -> 4 
行 2 -> 1 
行 3 -> 2 
行 4 -> 5 
从最弱到最强对这些行排序后得到 [2,0,3,1,4]
```

**示例 2：**

```
输入：mat = 
[[1,0,0,0],
 [1,1,1,1],
 [1,0,0,0],
 [1,0,0,0]], 
k = 2
输出：[0,2]
解释： 
每行中的军人数目：
行 0 -> 1 
行 1 -> 4 
行 2 -> 1 
行 3 -> 1 
从最弱到最强对这些行排序后得到 [0,2,3,1]
```

**代码：**

```c++
int getareaLen(vector<int>& mat ){
    int left = 0 , right = mat.size() - 1 ;
    while(left <= right){
        int mid = left + (right - left)/2 ;
        if(mat[mid] == 1)
            left = mid + 1;
        else if(mat[mid] == 0)
            right = mid - 1;
    }
    return left;
}
vector<int> kWeakestRows(vector<vector<int>>& mat, int k) {
    int nLen = mat.size();
    vector<int> res;
    std::set<int> vecTmp;
    for(int i = 0 ; i < nLen ; i++){
        int nAreaSize = getareaLen(mat[i]);
        // 将军队个数*1000 然后加上行数 ，这个问题是当行数超过1000 或者单行太长的时候容易出现问题
        vecTmp.insert(nAreaSize*1000 + i);
    }
    int nPos = 0 ; 
    for(set<int>::iterator itTmp = vecTmp.begin() ; itTmp != vecTmp.end()&&nPos < k ; ++itTmp){
        res.push_back(*itTmp%1000);
        nPos++;
    }
    return res;
}
```

#### [475. 供暖器](https://leetcode-cn.com/problems/heaters/)

难度简单124收藏分享切换为英文关注反馈

冬季已经来临。 你的任务是设计一个有固定加热半径的供暖器向所有房屋供暖。

现在，给出位于一条水平线上的房屋和供暖器的位置，找到可以覆盖所有房屋的最小加热半径。

所以，你的输入将会是房屋和供暖器的位置。你将输出供暖器的最小加热半径。

**说明:**

1. 给出的房屋和供暖器的数目是非负数且不会超过 25000。
2. 给出的房屋和供暖器的位置均是非负数且不会超过10^9。
3. 只要房屋位于供暖器的半径内(包括在边缘上)，它就可以得到供暖。
4. 所有供暖器都遵循你的半径标准，加热的半径也一样。

**示例 1:**

```
输入: [1,2,3],[2]
输出: 1
解释: 仅在位置2上有一个供暖器。如果我们将加热半径设为1，那么所有房屋就都能得到供暖。
```

**示例 2:**

```
输入: [1,2,3,4],[1,4]
输出: 1
解释: 在位置1, 4上有两个供暖器。我们需要将加热半径设为1，这样所有房屋就都能得到供暖。
```

**代码:**

```c++
int findRadius(vector<int>& houses, vector<int>& heaters) {
    sort(houses.begin(),houses.end());
    sort(heaters.begin(),heaters.end());
    int res=0 , houPos = 0 , heaPos = 0 , len = houses.size();
    heaters.insert(heaters.begin(),-1000000000);// 添加一个最小值  最大值 保证边界情况
    heaters.push_back(INT_MAX);
    for( ; houPos<len ; houPos++){
        // 当houses[i] 不位于 hea[j] 和hea[j+1]范围内  往后走 找到合适的位置
        while(houses[houPos] < heaters[heaPos] || houses[houPos] > heaters[heaPos+1]) 
            heaPos++;
        res=max(res,min(houses[houPos]-heaters[heaPos],heaters[heaPos+1]-houses[houPos]));
    }

    return res;
}

int findRadius(vector<int>& houses, vector<int>& heaters) {
    sort(heaters.begin(),heaters.end());                //先对数组进行排序
    int max = INT_MIN;
    for(int i=0;i<houses.size();i++){
        int left = 0 , right = heaters.size()-1 , mid = 0 ,temp = INT_MAX;
        while(left<=right){            
            mid = left+(right-left)/2;              //这样写防止left+right 超出限制
            if(heaters[mid] == houses[i]){
                temp = 0;
                break;                              //相等直接不用比，距离为0
            }else if(heaters[mid] > houses[i]){
                temp = min(temp,heaters[mid]-houses[i]);
                right = mid-1;                      //往heaters左子树走
            }else if(heaters[mid] < houses[i]){
                temp = min(temp,houses[i]-heaters[mid]);
                left = mid +1;                      //往heaters右子树走
            }
        }
        max = max<temp?temp:max;            //每趟比出的min值与已有的max再进行对比
    }
    return max;
}
```

#### [911. 在线选举](https://leetcode-cn.com/problems/online-election/)

难度中等16收藏分享切换为英文关注反馈

在选举中，第 `i` 张票是在时间为 `times[i]` 时投给 `persons[i]` 的。

现在，我们想要实现下面的查询函数： `TopVotedCandidate.q(int t)` 将返回在 `t` 时刻主导选举的候选人的编号。

在 `t` 时刻投出的选票也将被计入我们的查询之中。在平局的情况下，最近获得投票的候选人将会获胜。

**示例：**

```
输入：["TopVotedCandidate","q","q","q","q","q","q"], [[[0,1,1,0,0,1,0],[0,5,10,15,20,25,30]],[3],[12],[25],[15],[24],[8]]
输出：[null,0,1,1,0,0,1]
解释：
时间为 3，票数分布情况是 [0]，编号为 0 的候选人领先。
时间为 12，票数分布情况是 [0,1,1]，编号为 1 的候选人领先。
时间为 25，票数分布情况是 [0,1,1,0,0,1]，编号为 1 的候选人领先（因为最近的投票结果是平局）。
在时间 15、24 和 8 处继续执行 3 个查询。
```

 

**提示：**

1. `1 <= persons.length = times.length <= 5000`
2. `0 <= persons[i] <= persons.length`
3. `times` 是严格递增的数组，所有元素都在 `[0, 10^9]` 范围中。
4. 每个测试用例最多调用 `10000` 次 `TopVotedCandidate.q`。
5. `TopVotedCandidate.q(int t)` 被调用时总是满足 `t >= times[0]`。

**代码：**

```c++
vector<int> newtimes;
vector<int> votewinner;
TopVotedCandidate(vector<int> &persons, vector<int> &times)
{
    unordered_map<int, int> votemap;
    votewinner = vector<int>(persons.size(), 0);
    newtimes = times;
    int winner = 0 , maxvalue = 0;
    for (int i = 0; i < persons.size(); i++) {
        votemap[persons[i]]++;
        if(votemap[persons[i]]>=maxvalue){
            maxvalue = votemap[persons[i]];
            winner = persons[i];
        }
        votewinner[i] = winner;
    }
}
int q(int t)
{
    // upper_bound 从数组的begin位置到end-1位置二分查找第一个大于或等于num的数字，找到返回该数字的地址，不存在则返回end。采用的为二分查找
    int index = upper_bound(newtimes.begin(), newtimes.end(), t) - newtimes.begin();
    return votewinner[index-1];
}
```

 