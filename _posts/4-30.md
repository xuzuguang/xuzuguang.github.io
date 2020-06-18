# LeetCode:4-30

#### [49. 字母异位词分组](https://leetcode-cn.com/problems/group-anagrams/)

给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。

**示例:**

```
输入: ["eat", "tea", "tan", "ate", "nat", "bat"]
输出:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```

**说明：**

- 所有输入均为小写字母。
- 不考虑答案输出的顺序。

**代码：**

~~~~c++
vector<vector<string>> groupAnagrams(vector<string>& strs) {
    /**
    	大体思路：
    	1：将单个字符串排序。如果是字母异位词,排序后应该为同一个字符串
    	2：遍历字符串数组，使用hashMap记录排序后的异位词以及所对应的下标并得到结果
    **/
    vector<vector<string>> res ;
    if(strs.size() == 0)
        return res;
    unordered_map<string , int> unMap;
    for(string str:strs){
        string tmp = str;
        sort(tmp.begin() , tmp.end());//排序
        if(unMap.find(tmp) != unMap.end()){
            res[unMap[tmp]].push_back(str);
        }else{
            vector<string> vecTmp;
            vecTmp.push_back(str);
            res.push_back(vecTmp);
            unMap[tmp] = res.size()-1;
        }
    }
    return res;
}
~~~~



#### [138. 复制带随机指针的链表](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)

给定一个链表，每个节点包含一个额外增加的随机指针，该指针可以指向链表中的任何节点或空节点。

要求返回这个链表的 **[深拷贝](https://baike.baidu.com/item/深拷贝/22785317?fr=aladdin)**。 

我们用一个由 `n` 个节点组成的链表来表示输入/输出中的链表。每个节点用一个 `[val, random_index]` 表示：

- `val`：一个表示 `Node.val` 的整数。
- `random_index`：随机指针指向的节点索引（范围从 `0` 到 `n-1`）；如果不指向任何节点，则为 `null` 。

 

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e1.png)

```
输入：head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
输出：[[7,null],[13,0],[11,4],[10,2],[1,0]]
```

**示例 2：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e2.png)

```
输入：head = [[1,1],[2,1]]
输出：[[1,1],[2,1]]
```

**示例 3：**

**![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e3.png)**

```
输入：head = [[3,null],[3,0],[3,null]]
输出：[[3,null],[3,0],[3,null]]
```

**示例 4：**

```
输入：head = []
输出：[]
解释：给定的链表为空（空指针），因此返回 null。
```

**代码：**

```c++
/**
	遍历链表，同时维护一个hashMap。如果next和random不在hashmap中，则新建。
**/
Node* copyRandomList(Node* head) {
    unordered_map<Node* , Node*> unMap;
    Node* res = new Node(0);
    Node* pos = res;
    while(head!= NULL){
        if(unMap.find(head) == unMap.end()){
            Node* tmp = new Node(head->val);
            unMap[head] = tmp;
        }
        if(unMap[head]->random == NULL && head->random != NULL){
            if(unMap.find(head->random) == unMap.end()){
                Node* tmp1 = new Node(head->random->val);
                unMap[head->random] = tmp1;
            }
            unMap[head]->random = unMap[head->random];
        }
        pos->next = unMap[head];
        pos = pos->next;
        head = head->next;
    }
    return res->next;
}
```

#### [187. 重复的DNA序列](https://leetcode-cn.com/problems/repeated-dna-sequences/)

所有 DNA 都由一系列缩写为 A，C，G 和 T 的核苷酸组成，例如：“ACGAATTCCG”。在研究 DNA 时，识别 DNA 中的重复序列有时会对研究非常有帮助。

编写一个函数来查找 DNA 分子中所有出现超过一次的 10 个字母长的序列（子串）。

 

**示例：**

```
输入：s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"
输出：["AAAAACCCCC", "CCCCCAAAAA"]
```

**代码：**

```c++
//遍历字符串，一次截取10个字符，更新hashMap （字符串 以及字符串出现次数），统计字符串出现次数超过1的即可
vector<string> findRepeatedDnaSequences(string s) {
    vector<string> res;
    int s_Len = s.size();
    if(s_Len <= 10) return res;
    unordered_map<string , int >  unMap;
    for(int i = 0 ; i < s_Len ; i++){
        string tmp = s.substr(i , 10);
        if(unMap[tmp] == 1)
            res.push_back(tmp);
        unMap[tmp]++;
    }
    return res;
}
```

#### [274. H 指数](https://leetcode-cn.com/problems/h-index/)

给定一位研究者论文被引用次数的数组（被引用次数是非负整数）。编写一个方法，计算出研究者的 *h* 指数。

[h 指数的定义](https://baike.baidu.com/item/h-index/3991452?fr=aladdin)：h 代表“高引用次数”（high citations），一名科研人员的 h 指数是指他（她）的 （N 篇论文中）**至多**有 h 篇论文分别被引用了**至少** h 次。（其余的 *N - h* 篇论文每篇被引用次数 **不超过** *h* 次。）

例如：某人的 h 指数是 20，这表示他已发表的论文中，每篇被引用了至少 20 次的论文总共有 20 篇。

 

**示例：**

```
输入：citations = [3,0,6,1,5]
输出：3 
解释：给定数组表示研究者总共有 5 篇论文，每篇论文相应的被引用了 3, 0, 6, 1, 5 次。
     由于研究者有 3 篇论文每篇 至少 被引用了 3 次，其余两篇论文每篇被引用 不多于 3 次，所以她的 h 指数是 3。
```

**分析:**

> 我们想象一个直方图，其中 x 轴表示文章，y 轴表示每篇文章的引用次数。如果将这些文章按照引用次数降序排序并在直方图上进行表示，那么直方图上的最大的正方形的边长 h 就是我们所要求的 h。(如下图所示)
>
> 首先我们将引用次数降序排序，在排完序的数组 citations 中，如果citations[i] >i，那么说明第 0 到 i 篇论文都有至少 i+1 次引用。
>
> 因此我们只要找到最大的 i 满足citations[i] >i，那么 h 指数即为 i+1。
>
> 找到最大的 i 的方法有很多，可以对数组进行线性扫描，也可以使用二分查找。由于排序的时间复杂度已经为 O(n \log n)，因此无论是线性扫描 O(n)还是二分查找 O(log n)，都不会改变算法的总复杂度。

![h-index](https://pic.leetcode-cn.com/Figures/274_H_index.svg)





**代码：**

~~~~c++
int hIndex(vector<int>& citations) {
    // 先进行排序
    sort(citations.begin() , citations.end());
    int res = 0;
    if(citations.size() == 0) return res;
    // 找最大的正方形，那 y值一定要比x值大
    while( res < citations.size() && citations[citations.size()-res-1] > res )
        res++;
    return res;
}
~~~~

**优化**

> 基于比较的排序算法存在时间复杂度下限 O(n log n)，如果想得到时间复杂度更低的算法，就必须考虑不基于比较的排序。
>
> 在这题中， 我们可以通过一个不难发现的结果来进行优化。
>
> -》如果一篇文章的引用次数超过论文的总数n，那么将它的引用次数降低为n也不会改变h指数的值。 如下图所示：

<img src="https://pic.leetcode-cn.com/Figures/274_H_index_2.svg" alt="h-index cut off" style="zoom:80%;" />

> 也就是说，
>
> 对于citations = {1,3,2,3,100} 和citations= {1,3,2,3,5}的结果没有区别。

~~~~c++
int hIndex(vector<int>& citations) {
    int res = 0;
    if(citations.size() == 0) return res;
    vector<int> sum(citations.size()+1 , 0);
    for(auto i : citations){
        sum[(citations.size() > i ? i : citations.size())] += 1;
    }
    res = citations.size() ;
    for(int s = sum[res] ; res > s ; s += sum[res] )
        res--;
    return res;
}
~~~~

