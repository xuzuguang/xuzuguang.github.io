---
layout: post
title: "Leetcode 151总结"
description: ""
category:
tags: [ algorithm ]
---

刷了若干天leetcode, 总算弄完了。代码在[这里](https://github.com/openinx/algorithm-solution/tree/master/leetcode)。

* [Reverse Words in a String](https://oj.leetcode.com/problems/reverse-words-in-a-string/)
模拟 字符串

* [Evaluate Reverse Polish Notation](https://oj.leetcode.com/problems/evaluate-reverse-polish-notation/)
模拟 后缀表达式求值

* [Max Points on a Line](https://oj.leetcode.com/problems/max-points-on-a-line/)
平面给出N个点，找一个直线，使得经过的点数最多。枚举每个点，以此为原点坐标，求出相对原点坐标，然后计算y/x，用hash表计数求出最大的重复值。O(N^2)

* [Sort List](https://oj.leetcode.com/problems/sort-list/)
QuickSort和MergeSort链表版本. O(NlogN) 值得注意的情况是所有元素都相同时，假设qsort分段从左到右的话，qsort会退化O(N^2).

* [Insertion Sort List ](https://oj.leetcode.com/problems/insertion-sort-list/)
插入排序链表实现. O(N^2)

* [LRU Cache ](https://oj.leetcode.com/problems/lru-cache/)
LRU-Cache算法。最有复杂度保证每次get,set操作都为O(1). 双向链表+Hash。 用C++10的STL的LIST和MAP的GET，SET复杂度O(logN)

* [Binary Tree Postorder Traversal ](https://oj.leetcode.com/problems/binary-tree-postorder-traversal/)
智商着急，写个栈模拟后序遍历都卡半天。 网上有很简洁的写法。

```cpp
void postOrderTraversalIterativeTwoStacks(BinaryTree *root) {
  if (!root) return;
  stack<BinaryTree*> s;
  stack<BinaryTree*> output;
  s.push(root);
  while (!s.empty()) {
    BinaryTree *curr = s.top();
    output.push(curr);
    s.pop();
    if (curr->left)
      s.push(curr->left);
    if (curr->right)
      s.push(curr->right);
  }
  while (!output.empty()) {
    cout << output.top()->data << " ";
    output.pop();
  }
}
```

* [Binary Tree Preorder Traversal](https://oj.leetcode.com/problems/binary-tree-preorder-traversal/)

* [Reorder List](https://oj.leetcode.com/problems/reorder-list/)
翻转后半段链表，然后间隔一个拼接。O(N)

* [Linked List Cycle II ](https://oj.leetcode.com/problems/linked-list-cycle-ii/)
设一个慢指针， 一个快指针。慢指针每次走一步，快指针每次走两步。假若有环，快指针必定和慢指针在某个环内点重合。然后证明找到重合点之后，再走相同的步数可以找到环的起始点。

* [Linked List Cycle](https://oj.leetcode.com/problems/linked-list-cycle/)
同上。

* [Word Break II ](https://oj.leetcode.com/problems/word-break-ii/)
给定一个单词集和一个字符串，判断字符串是否可以拆成多个单词，要求每个单词都是单词集里面的。很明显的DP， 问题是要回溯。我写了个递归回溯。复杂度O(N^3).

* [Word Break](https://oj.leetcode.com/problems/word-break/)
同上

* [Copy List with Random Pointer](https://oj.leetcode.com/problems/copy-list-with-random-pointer/)
这题答案很巧妙。a->b->c这样的链表，每个元素后面插入一个前一个元素。a->A->b->B->c->C . 然后再设置A，B,C的random指针，最后把A->B->C这个链表剥离出来即可。 

* [Binary Tree Maximum Path Sum](https://oj.leetcode.com/problems/binary-tree-maximum-path-sum/) 
一颗树，每个节点都有一个数。求一条路径，使得路径上各点权值相加最大。动态规划 + 树遍历

* [Single Number II](https://oj.leetcode.com/problems/single-number-ii/)
32位二进制对齐，求N个数各BIT位之和MOD 3形成的二进制转10进制。即答案。

* [Single Number](https://oj.leetcode.com/problems/single-number/)
XOR运算满足交换律和结合律。直接XOR各数求和即答案。

* [Candy](https://oj.leetcode.com/problems/candy/)
算法1:每次选择低谷点，然后按照低谷点从小到大排序，一次发各上坡路线的糖果。O(N*logN)
算法2: 左扫上坡路，保存到Array里面，右扫上坡路，与左扫上坡路比较取较大值。O(N)
犯了个傻逼错误：

```cpp
int a[3] ; memset(a, 0, sizeof(a)) ; // OK 
int *a = new int(3) ; memset(a, 0, sizeof(a)) ; // ERROR 
int *a = new int(3) ; memset(a, 0, sizeof(int) * 3 ) ; // OK 
```

* [Gas Station ](https://oj.leetcode.com/problems/gas-station/)

* [Clone Graph](https://oj.leetcode.com/problems/clone-graph/) 
图广度优先遍历， 写个深度优先遍历的版本？

* [Palindrome Partitioning II](https://oj.leetcode.com/problems/palindrome-partitioning-ii/)
DP：将一个字符串切割成若干个回文串，求最小切割次数。O(N^2)

* [Palindrome Partitioning ](https://oj.leetcode.com/problems/palindrome-partitioning/)
DP

* [Surrounded Regions ](https://oj.leetcode.com/problems/surrounded-regions/)
走迷宫问题。有个优化是只要对O在边界上的情况做搜索拓展即可。另外此题卡了DFS的内存，只能用BFS水之。

* [Sum Root to Leaf Numbers ](https://oj.leetcode.com/problems/sum-root-to-leaf-numbers/)
DP + 树遍历

* [Longest Consecutive Sequence ](https://oj.leetcode.com/problems/longest-consecutive-sequence/)
为什么直接排序，然后水过去了?

* [Word Ladder II](https://oj.leetcode.com/problems/word-ladder-ii/)
BFS 然后按照步数DFS回溯找路径。

* [Word Ladder ](https://oj.leetcode.com/problems/word-ladder/)
BFS

* [Valid Palindrome](https://oj.leetcode.com/problems/valid-palindrome/)
水题

* [Binary Tree Maximum Path Sum](https://oj.leetcode.com/problems/binary-tree-maximum-path-sum/)
DP + 树遍历。 
dp[i]表示以i为根节点的子树中，经过i节点的路径的最大和值。
maxsum[i] 表示以i为根节点的子树中，路径最大的和值。 这条路径可能经过i节点，也可能不经过i节点。

```
dp[root] = max(root->val,
               root->val + dp[root->left],
               root->val + dp[root->right],
               root->val + dp[root->left] + dp[root->right]);
maxsum[root] = max( maxsum[root->left], maxsum[root->right], dp[root]);
```

* [Best Time to Buy and Sell Stock III](https://oj.leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)
将序列分割成两段，分别转化成[Best Time to Buy and Sell Stock](https://oj.leetcode.com/problems/best-time-to-buy-and-sell-stock/)这个问题的最优值。二者求和取最大值即答案。
* [Best Time to Buy and Sell Stock II](https://oj.leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)
序列的连续递增增量之和即答案。
* [Best Time to Buy and Sell Stock](https://oj.leetcode.com/problems/best-time-to-buy-and-sell-stock/)
序列中两数之差最大值即答案。
* [Triangle](https://oj.leetcode.com/problems/triangle/)
DP + 滚动数组优化空间
* [Pascal's Triangle II](https://oj.leetcode.com/problems/pascals-triangle-ii/)
DP + 滚动数组优化空间。 Pascal数就是组合数，也可以称之为杨辉三角。这个图形推出一个组合公式: C(n,i) = C(n-1,i) + C(n-1,i-1) 
* [Pascal's Triangle](https://oj.leetcode.com/problems/pascals-triangle/)
同上

* [Populating Next Right Pointers in Each Node II ](https://oj.leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/)
为了达到O(1)的空间复杂度，根据k-1层的Next指针信息遍历，依次将k层的儿子组织成链表，直到到达最底层的叶子层。这个中间没有用到队列，因为next信息已经将k层的节点组织成队列了。
* [Populating Next Right Pointers in Each Node ](https://oj.leetcode.com/problems/populating-next-right-pointers-in-each-node/)
* [Distinct Subsequences](https://oj.leetcode.com/problems/distinct-subsequences/)
DP + 滚动数组优化空间

```cpp
dp[0,j] = 1 ; (0<=j<=strlen(T))
dp[i,0] = 1 ; (0<=i<=strlen(S))
if(s[i-1] == s[j-1])
    dp[i,j] = dp[i-1,j-1] + dp[i-1,j]
else
    dp[i,j] = dp[i-1,j]
```

* [Flatten Binary Tree to Linked List ](https://oj.leetcode.com/problems/flatten-binary-tree-to-linked-list/)
树遍历 + 链表拼接。 遍历子树得到的链表，应该同时保存链表的head和tail。 否则做左子树和右子树的链表拼接，会消耗O(N)的复杂度，导致算法最后的复杂度为O(N^2).

* [Path Sum II](https://oj.leetcode.com/problems/path-sum-ii/) 
给定一个值SUM, 和一颗树。求树上所有从根到叶子的路径，使得该路径所有节点值之和等于SUM. DFS遍历所有节点，并用vector保存当前路径上的点。
* [Path Sum](https://oj.leetcode.com/problems/path-sum/) 
同上

* [Minimum Depth of Binary Tree](https://oj.leetcode.com/problems/minimum-depth-of-binary-tree/)
求树的最小深度。用栈写一个？

* [Balanced Binary Tree](https://oj.leetcode.com/problems/balanced-binary-tree/)
判断一颗树是否平衡(两子树高度相差不超过1)

* [Convert Sorted List to Binary Search Tree ](https://oj.leetcode.com/problems/convert-sorted-list-to-binary-search-tree/)
链表转成高度平衡的二叉树，直接便利链表找中间点，然后递归构造BST，复杂度O(N*logN). 但有更好的写法, 复杂度O(N)： 

```cpp
TreeNode* buildTree(ListNode * &list, int start, int end){
	if(start > end ) return NULL ;
	int mid =  (start + end ) >> 1;
	TreeNode* left = buildTree(list, start, mid-1);
	TreeNode* root = new TreeNode(list->val);
	root->left = left;
	list=list->next;
	TreeNode *right = buildTree(list, mid+1, end);
	root->right = right;
	return root;
}

TreeNode *sortedListToBST(ListNode *head) {
    int n = 0 ; 
    for(ListNode *p = head ; p != NULL ; p=p->next , ++n);
   	if(n == 0) return NULL;
    return buildTree(head, 0, n-1);
}
```

* [Convert Sorted Array to Binary Search Tree](https://oj.leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)
将数组转成一颗平衡二叉树。O(N)

* [Binary Tree Level Order Traversal II ](https://oj.leetcode.com/problems/binary-tree-level-order-traversal-ii/)
按照层次依次输出树的各层节点。 BFS

* [Construct Binary Tree from Inorder and Postorder Traversal](https://oj.leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)
给定中序和后序还原二叉树。注意给定前序和后序，是无法还原二叉树的。比如前序为ABCD,后序为BCDA。 我们知道根节点为A,A的儿子们为BCD,但是不能确定左子树和右子树如何划分BCD，所以没法确定树的原型。 
* [Construct Binary Tree from Preorder and Inorder Traversal](https://oj.leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
给定前序和中序，还原二叉树。
* [Maximum Depth of Binary Tree](https://oj.leetcode.com/problems/maximum-depth-of-binary-tree/)
求树的最大深度。 用栈写一个？

* [Binary Tree Zigzag Level Order Traversal ](https://oj.leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)
按照层次序遍历二叉树。BFS可解。问题假设用两个栈而不用队列怎么解哦？

* [Binary Tree Level Order Traversal](https://oj.leetcode.com/problems/binary-tree-level-order-traversal/)
二叉树层次便利

* [Symmetric Tree](https://oj.leetcode.com/problems/symmetric-tree/)
判断二叉树是否左右对称。递归解决

* [Same Tree](https://oj.leetcode.com/problems/same-tree/)
判断两颗二叉树是否完全相同。类似于上题。

* [Recover Binary Search Tree](https://oj.leetcode.com/problems/recover-binary-search-tree/)
一颗BST有两个节点的val是颠倒的。怎么找到并纠正？

* [Validate Binary Search Tree](https://oj.leetcode.com/problems/validate-binary-search-tree/)
如何验证一颗树是否是BST。设计一个`int isBST(TreeNode*root, int &maxval, int &minval)`接口，表示每次递归解决root这颗子树的问题时，会把该子树所有节点的maxVal和minVal都找出来。这样父亲节点就能够通过比较自己的val和儿子的minval & maxval来判断是否是BST了。 

* [Interleaving String](https://oj.leetcode.com/problems/interleaving-string/)
DP: O(len(s1) * len(s2)) 滚动数组优化空间到O(min(len(s1) * len(s2)))

```
dp[0,0] = 1 
dp[0,j] = (s3[j-1] == s2[j-1] && dp[0,j-1]) ; when (1<=j<=len2); 
dp[i,0] = (s3[i-1] == s1[i-1] && dp[i-1,j]) ; when (1<=i<=len1);
dp[i,j] = (s3[i+j-1] == s1[i-1] && dp[i-1][j]) |
          (s3[i+j-1] == s2[j-1] && dp[i][j-1]) ; 
          when (1<=i<=len1 && 1<=j<=len2 )
```

* [Unique Binary Search Trees II](https://oj.leetcode.com/problems/unique-binary-search-trees-ii/)
给定N，求出所有节点号为1..N的BST的树。DFS递归求解。

* [Unique Binary Search Trees ](https://oj.leetcode.com/problems/unique-binary-search-trees/)
求N个节点的BST总数。经典的[Catalan Number](http://en.wikipedia.org/wiki/Catalan_number) C(2n,n)/(n+1). 递推式为

```
h[0] = 1 
h[1] = 1
h[n] = h[0]*h[n-1] + h[1]*[n-2] + ... + h[n-1] * h[0] ; 
```

* [Restore IP Addresses](https://oj.leetcode.com/problems/restore-ip-addresses/)
暴力水过, O(12^3)

* [Reverse Linked List II](https://oj.leetcode.com/problems/reverse-linked-list-ii/)
逆转链表的一段

* [Subsets II ](https://oj.leetcode.com/problems/subsets-ii/)
DFS


* [Scramble String](https://oj.leetcode.com/problems/scramble-string/)

* [Partition List](https://oj.leetcode.com/problems/partition-list/)
链表实现QuickSort的select函数。

* [Largest Rectangle in Histogram ](https://oj.leetcode.com/problems/largest-rectangle-in-histogram/)
求给定序列组成的柱形图中最大长方形面积。(单调栈 OR 并查集)

单调栈实现:

```cpp
int largestRectangleArea(vector<int> &height) {
	stack<int> s ; 
	height.push_back(0);
	int i = 0, maxArea = 0 ;
	while( i < height.size() ){
		if(s.empty() || height[s.top()] <= height[i]){
			s.push(i++);
		}else{
			int t = s.top(); s.pop();
			maxArea = max(maxArea, height[t] * (s.empty() ? i: i-s.top()-1));
		}
	}
	return maxArea;
}
```

并查集实现：

```cpp
for(i = 1 ; i<=n; ++i)  scanf("%d",&h[i]);
for(i = 1 ; i<=n; ++i)  r[i] = l[i] = i;
h[0] = h[n +1] = -1;
for(i = 1 ; i<=n; ++i)
    while( h[i] <= h[ l[i] - 1 ] ) l[i] = l[ l[i] - 1 ];
for(i = n ; i>= 1 ; --i)
    while( h[i] <= h[ r[i] + 1 ] )  r[i] =r[ r[i] + 1 ];
__int64 ans = 0;
for( i = 1 ; i<=n; ++i)
   ans  = max( ans , (__int64)(r[i] - l[i] + 1 ) * (__int64)h[i] );
```

* [3Sum](https://oj.leetcode.com/problems/3sum/)
给定一个序列，求有多少个三元组使得(a+b+c=0). 对Array排序，枚举c，然后再一个有序序列中寻找两个数之和是否等于C。有两种方法： 
1. 将a放在一个hash表里面，然后查找C-a 是否在hash表内。 
2. 设置一个左指针lptr，一个右指针rptr。 二者之和大于-C时， rptr左移。 小于-C时, lptr右移。比较trick的是对于序列`-2 0 1 1 2 2`这种情况，碰到第一个1时，`-2 0 1`不能组成三元组使得等于0，`-2 0 1 1`可以组成三元组等于0。所以碰到连续的相同的元素，只需要考虑连续数的最后一个数即可。 

* [3Sum Closest](https://oj.leetcode.com/problems/3sum-closest/)
类似[3Sum](https://oj.leetcode.com/problems/3sum/)的思路。可以证明：当`lptr + rptr > C`时, 不可能通过lptr左移使得abs(lptr+rptr-C)变小, 只能是rptr右移使之变小 。所以实现方法和[3Sum](https://oj.leetcode.com/problems/3sum/)一样。


* [Insert Interval](https://oj.leetcode.com/problems/insert-interval/)
* [Wildcard Matching](https://oj.leetcode.com/problems/wildcard-matching/)
KMP + 贪心 假设*的个数为K, 复杂度O(K*N) 其实写个_看毛片算法_解这个问题真的是坑无数。比如最后一个'hi'和'*?'这种情况，‘*’不能贪婪匹配'hi'。 归纳起来，最后一个'*'是不能贪婪的，只要求'*'之后的字串能严格和主串的最后一段匹配即可。关于这个问题的更详细的各种解法，可参考[一个老外数十年的辛勤劳作](http://xoomer.virgilio.it/acantato/dev/wildcard/wildmatch.html)。 

* [Pow(x, n)](https://oj.leetcode.com/problems/powx-n/)二分求`P^n`值。值得注意的是，当int n = -2147483648 时，调用函数abs(n) = 2147483648时，会导致INT溢出。 _这是调用int类型的abs函数道不尽说不完的坑啊。_

* [Container With Most Water](https://oj.leetcode.com/problems/container-with-most-water/)
DP ： 关键在于慧眼发现这样一个性质： 假设`3 5 2 4 3 5`这个序列, 两个端点，3和5。 对3来讲，最优解肯定是5，3不可能和其他的某个数达到最优解。所以3就排除掉了， 左端点右移，转化成一个较小规模的子问题了。 复杂度O(N)

* [Merge k Sorted Lists](https://oj.leetcode.com/problems/merge-k-sorted-lists/)两种方法： 两两合并复杂度为O(K*N), 假设K为要合并的链表的个数，N为K个链表的所有元素之和;用一个元素个数为K的堆维护K个链表。复杂度为O(N*logK)。

纠结了好一会儿堆的cmp重载。C++默认的堆是最大堆。

```cpp
class classcmp{
public:
	bool operator() (const ListNode* a, const ListNode* b)const{
		return a->val > b->val ;
	}
};
priority_queue<ListNode*, vector<ListNode*>, classcmp> que ;
```

* [Combination Sum](https://oj.leetcode.com/problems/combination-sum/)
DFS
* [Combination Sum II](https://oj.leetcode.com/problems/combination-sum-ii/)
DFS: 对于`1 1 1 2 5 6 7 10`这种序列，防止出现重复的`1 1 2`这个序列很重要。其实只要保证选择的`1`全部都在`1 1 1`序列的最前端就可以了。当DFS到depth这个深度时，走没有选择`Element[depth]`这个元素的分支时，后面所有与`Element[depth]`相等的元素，就都不考虑放到解之中了。 走选择了`Element[depth]`这条分支时，按照正常递归下去就OK了。 这样可以保证无重复解。

* [Multiply Strings ](https://oj.leetcode.com/problems/multiply-strings/)
大数乘法

* [Permutations ](https://oj.leetcode.com/problems/permutations/) 生成序列[1,2,3..,n]的全排列。
* [permutations-ii](https://oj.leetcode.com/problems/permutations-ii/) 生成有重复元素的的全排列，要求不能输出重复的排列。 我用的方法是： 对每个数都用了一个cnt计数器，当递归到当前深度时，试探所有cnt值大于1的数，然后将当前深度的值填为该数。

* [N-Queens](https://oj.leetcode.com/problems/n-queens/) N皇后问题，求所有解。
* [N-Queens II ](https://oj.leetcode.com/problems/n-queens-ii/) N皇后问题， 求解数。尝试几种写法：DFS; 迭代；位运算。 位运算代码最少，如下（答案调用dfs(0,0,0,n,sum)，sum值即答案）：

```cpp
#define LOWBIT(x) ((x)&(-x))
void dfs(int row, int ld, int rd, int n, int &sum){
    int M = (1<<n)-1, pos, p;
    if(row == M) {  ++ sum; return;} 
    pos = ((row|ld|rd) & M) ^ M; 
    while(pos){
        p = LOWBIT(pos);
        dfs(row|p, (ld|p)<<1, (rd|p)>>1, n, sum);
        pos -= pos & p;
    }
}
```

* [Add Binary](https://oj.leetcode.com/problems/add-binary/) 水题一枚。 调程序一个BUG很不好找： 

```cpp
(temp & 1) + '0' // a = temp & 1 ; b = a + '0'  ;
temp & 1 + '0'   // a = 1 + '0'  ; b = temp & 1 ; 
```

* [Sort Colors](https://oj.leetcode.com/problems/sort-colors/)

智力题。看下图这种状态，有r,w,b三个指针。[0,r)的都是0，[r,w)都是1, [b,+oo)都是2。 现在考虑w指针所在的数，分三种情况：
(0) w指的数为0,交换w和r所在的数，r和w指针同时右移。
(1) w指的数为1,w右移;
(2) w指的数为2,b左移，交换w和b指针所在的数;

```cpp
void sortColors(int A[], int n) {
    for (int r = 0, w = 0, b = n; w < b; )
      if (A[w] == 0)
        swap(A[r++], A[w++]);
      else if (A[w] == 2)
        swap(A[--b], A[w]);
      else
        w++;
  }

```
看到该题[背景](http://www.iis.sinica.edu.tw/~scm/ncs/2010/10/dutch-national-flag-problem/)顿时吓傻


```cpp
# start
0001111********2222
   ^   ^       ^
   r   w       b 

# case.0
00001111*******2222
    ^   ^      ^
    r   w      b 

# case.1
00011111*******2222
   ^    ^      ^
   r    w      b 

# case.2
0001111*******22222
   ^   ^      ^
   r   w      b 
```

* [First Missing Positive](https://oj.leetcode.com/problems/first-missing-positive/)
智力题，找出一个无序序列中第一个缺失的正整数。要求时间O(N)，空间O(1). 其实只要将每个在1～N之前的整数，把i这个数值存放到A[i]这里存放。然后遍历即可。代码如下,复杂度分析：while循环每次swap都会把A[i]这个数放到A[i]-1这个Index对应的位置存放，最多有N个数，所以for循环内所有while进行的swap操作之和不超过N. 摊分下来每个for操作都swap一次。总的复杂度为O(N).

```cpp
int firstMissingPositive(int A[], int n) {
	for(int i = 0 ; i < n ; ++ i)
		while(A[i] > 0 && A[i] <= n && A[A[i]-1] != A[i])
			swap(A[A[i]-1], A[i]);
	for(int i = 0 ; i < n ; ++ i)
		if(A[i] != i + 1)
			return i + 1;
	return n + 1;
}
```

* [Anagrams](https://oj.leetcode.com/problems/anagrams/)  anagrams这个单词真是个费解的单词。指的是对应字符个数相等的两个单词，称为anagrams. 例如aaaab和baaaa,aaaba都是anagrams。
每个单词字符排个序，最小字典序唯一。直接hash表判重即可。复杂度O(N*M*logM), 单词最大长度为M， 共有N个单词。


* [Edit Distance](https://oj.leetcode.com/problems/edit-distance/)
求两个字符串的最小编辑距离，从S串到T串，有三种操作： insert; delete; replace. dp[i,j]表示s[1..i]到t[1..j]的最小编辑距离:

```cpp
dp[0,0] = 0 ; 
dp[0,j] = j (1<=j<=len(t))
dp[i,0] = i (1<=i<=len(s))
dp[i,j] = min(dp[i-1,j] + 1, dp[i,j-1] + 1, dp[i-1,j-1] + 1) // delete, insert, replace
if(s[i] == t[j]) dp[i,j] = min(dp[i,j], dp[i-1,j-1]); 
```

* [Trapping Rain Water](https://oj.leetcode.com/problems/trapping-rain-water/)智力题。关键考虑每个i对答案的贡献值求和。能用一次遍历并且用O(1)的空间计算出答案吗？  这里还有个[悲剧的故事](http://qandwhat.apps.runkite.com/i-failed-a-twitter-interview/)

* [Set Matrix Zeroes ](https://oj.leetcode.com/problems/set-matrix-zeroes/) matrix[0,i]和matrix[i,0]保存maxtrix[i,j]的0状态,另设两个遍历row,col记录matrix[0,0..n-1]和matrix[0..n-1,0]是否为0.即可。


* [Median of Two Sorted Arrays](https://oj.leetcode.com/problems/median-of-two-sorted-arrays/)两个有序序列A[0..M-1]和B[0..N-1]找第k小的数问题。假设A[k/2-1]<B[k/2-1]，那么A[k/2-1]排在合并序列中的序号小于K。那么搜索第K值的时候，可以丢弃掉`A[k/2-1]`之前（包括自己）的一段。题目找的是第`（m+n)/2`小，每次减少一半的规模，所以总复杂都`log(m+n)`.

```cpp
double findKth(int a[], int m, int b[], int n, int k){
	//always assume that m is equal or smaller than n
	if (m > n)
		return findKth(b, n, a, m, k);
	if (m == 0)
		return b[k - 1];
	if (k == 1)
		return min(a[0], b[0]);
	//divide k into two parts
	int pa = min(k / 2, m), pb = k - pa;
	if (a[pa - 1] < b[pb - 1])
		return findKth(a + pa, m - pa, b, n, k - pa);
	else if (a[pa - 1] > b[pb - 1])
		return findKth(a, m, b + pb, n - pb, k - pb);
	else
		return a[pa - 1];
}
```

* [Longest Palindromic Substring](https://oj.leetcode.com/problems/longest-palindromic-substring/) [Manacher’s Algorithm](http://leetcode.com/2011/11/longest-palindromic-substring-part-ii.html)算法。能在O(N)的复杂度内找到一个字符串的最长回文子串。假设用动态规划或者暴力，复杂度都为O(N^2). 后缀数组也能解决这个问题。


* [Regular Expression Matching ](https://oj.leetcode.com/problems/regular-expression-matching/)没找到什么线性算法。 暴力算法来源于leetcode，假设`s=abbbbbbbbbbbbbbbbbb`, `t=ab*cd`这样的话。可能会出现如下情况：当有一个`*`字符时，最坏的情况下会达到O（n*m),这还的用KMP算法去做字串匹配. 当多个 `\*`字符时，就是`\*`的指数级复杂度了。

```cpp
s = abbbbbbbbbbbbbbbbbb
     ^
t = acd

s = abbbbbbbbbbbbbbbbbb
      ^
t = a cd

s = abbbbbbbbbbbbbbbbbb
       ^
t = a  cd

s = abbbbbbbbbbbbbbbbbb
        ^
t = a   cd
```


* [Scramble String ](https://oj.leetcode.com/problems/scramble-string/) DFS 类似卡特兰解空间搜索加上减枝，减枝技巧包括： 两串长度相等； 两串无序hash值相等; 两串同类字符数必须相等。
复杂度分析： 

```
h[n] = 2 * sum( h[i] * h[n-i] ) (1<= i < n )
```

* [Recover Binary Search Tree ](https://oj.leetcode.com/problems/recover-binary-search-tree/) 

假设一个序列 , 其中 8 和 14 被误交换了。 如图， 导致序列会有两个`>` , 记录第1个`>`的前驱和第2个`>`的后继两个指针，交换即可.

```cpp
# noraml
  < < <  <  <  <
1 5 8 10 11 14 23 

# missing swap
  < <  >  <  > < 
1 5 14 10 11 8 23
    ^        ^

# after
  < <  >  <  > < 
1 5 8 10 11 14 23
    ^        ^
``` 


* [Minimum Window Substring](https://oj.leetcode.com/problems/minimum-window-substring/)  字符串滑动窗口， 维护4个变量： left 左端点 ； right 右端点 ; cur[256] 各字符当前窗口内的计数值； curlen 当前窗口内目标串字符的个数。 时间复杂度O(N). 类似的题： [Substring with Concatenation of All Words ](https://oj.leetcode.com/problems/substring-with-concatenation-of-all-words/), [Longest Substring Without Repeating Characters ](https://oj.leetcode.com/problems/longest-substring-without-repeating-characters/) . 

