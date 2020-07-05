---
layout: post
title:  "LeetCode学习之树"
date:   2018-04-15
categories: 算法学习  
tags: 算法学习

---

* content
{:toc}


本文为LeetCode中关于树的题目学习



## 平衡二叉树

1. 概念

   平衡二叉树是基于二分法的策略提高数据的查找速度的二叉树的数据结构

2. 特点

   平衡二叉树是采用二分法思维将数据按照规则装成一个树形结构的数据，用这个树形结构的数据减少无关数据的检索，大大的提高数据检索的速度；平衡二叉树的数据结构组装过程有以下规则：

   1. 非叶子节点只能允许最多两个子节点存储
   2. 每个非叶子节点数据分布规则为左边的子节点小于当前节点值，而右边的子节点大于当前节点的值。

   ![img](https://xuzuguang.men//images/posts/btree1.jpg)

   平衡树的层级结构：因为平衡二叉树查询性能和树的层级（h高度）成反比，h值越小查询越快。为了保证树的结构左右两端数据大体平衡降低二叉树的查询难度一般会采用一种算法机制来实现节点数据结构的平衡，实现了这种算法的有比如Treap、红黑树、使用平衡二叉树能保证数据的左右两边的节点层级相差不会大于1，通过这样避免树形结构由于删除、增加变成线性链表影响查询效率，从而保证数据平衡的情况下查找数据的速度近于二分法查找；

   ![img](https://xuzuguang.men//images/posts/btree2.jpg)

3. 总结平衡二叉树特点：

   1. 非叶子节点最多拥有两个子节点；
   2. 非叶子节点值大于左边子节点，小于右边子节点
   3. 树的左右两边的层级树相差不会大于1
   4. 没有值相等重复的节点

## B树

1. 概念

   B树和平衡二叉树稍有不同的是B树属于多叉树又名平衡多路查找树，数据库索引计数中大量使用B树和B+树的数据结构

2. 规则

   1. 排序方式：所有节点关键字是按照递增次序排列，并遵循左小右大原则
   2. 子节点树：1 < 非叶节点的子节点树  <= M (M >= 2)，空树除外。在这里，M阶代表一个树节点最多i有多少个查找路径，对应这二叉树  三叉树  等。
   3. 关键字数：ceil(m/2)-1 <= 枝节点的关键字数量 <= M-1
   4. 所有叶子节点均在同一层，叶子节点除了包含关键字和关键字记录的指针，也有指向其子节点的指针只不过其指针地址都为null

   ![img](https://xuzuguang.men//images/posts/btree3.jpg)

3. B树的查询流程

   如上图所示需要查找E字母，流程如下：

   1. 获取根节点的关键字进行比较，E < M ， 所以往左子树寻找。
   2. 拿到左子树关键字D和G， D<E<G ，所以直接往D和G的中间节点找。
   3. 拿到E和F，因为E=E，所以直接返回关键字和指针信息

4. B树的插入节点流程

   定义一个5阶数，现在我们需要将 3 ，8，31，11，23，29，50，28这些数字构建出一个5阶树来。

   遵循规则：

   1. 节点拆分规则：当前是要组成一个5路查找树，那么此时 m = 5 ，关键字树必须<= 5-1

   2. 排序规则：满足节点本身比左边节点大，比右边节点小的排序规则。

   3. 先插入3、8、31、11

      ![img](https://xuzuguang.men//images/posts/btree4.jpg)

   4. 再插入23、29

      ![img](https://xuzuguang.men//images/posts/btree5.jpg)

   5. 再插入50、28

      ![img](https://xuzuguang.men//images/posts/btree6.jpg)

5. B树节点的删除

   （1）节点合并规则：当前是要组成一个5路查找树，那么此时m=5,关键字数必须大于等于ceil（5/2）（这里关键字数<2就要进行节点合并）；

   （2）满足节点本身比左边节点大，比右边节点小的排序规则;

   （3）关键字数小于二时先从子节点取，子节点没有符合条件时就向向父节点取，取中间值往父节点放；

   ![img](https://xuzuguang.men//images/posts/btree13.jpg)

6. 特点

   B树相对于平衡二叉树的不同是，每个节点包含的关键字增多了，特别是再B树应用到数据库中的时候，数据库充分利用了磁盘块的原理（磁盘数据存储是采用块的形式存储的，每个块的大小为4K， 每次IO进行数据读写的时候，同一个磁盘块的数据可以一次性读取出来）把树的节点关键字增多后树的层级比原来的二叉树少了，减少了数据查找次数和复杂度。

## B+树

B+树是B树的一个升级版，相对于B树来说B+树更充分的利用了节点的空间，让查询速度更加稳定，其速度完全接近于二分法查找。

**规则：**

1. B+树的非叶子节点不保存关键字记录的指针，只进行数据索引，这样使得B+树每个非叶子节点所能保存的关键字大大增加。

2. B+树叶子节点保存了父节点的所有关键字记录的指针，所有数据地址必须要到叶子节点才能获取到。

3. B+树子节点的关键字从小到大有序排列，左边结尾数据都会保存右边节点开始数据的指针。

4. 非叶子节点的子节点数 = 关键字数 

   ![img](https://xuzuguang.men//images/posts/btree7.jpg)

**特点：**

1. B+树的层级更少：相比较B树  B+树的每个非叶子节点存储的关键字更多，树的层级更少所以查询数据更快。
2. B+树的查询速度更稳定：B+所有关键字数据地址都再叶子节点上，所以每次查找的次数都相同所以查询速度要比B树更稳定。
3. B+树天然具有排序功能：B+树所有的叶子节点数据构成了一个有序链表，再查询大小区间的数据时候更加方便，数据紧密型很高，缓存的命中率也会高。
4. B+树全节点遍历更快：B+树遍历整棵树只需要遍历所有的叶子节点既可，而不需要B树一样对每一层进行遍历，有利于数据库做全表扫描。
5. B树相对于B+树的优点是，如果经常访问的数据离根节点很近，而B树的非叶子几点本身存储了关键字其数据的地址，所以这种数据检索的时候会要比B+树快。

**总结：**

1. 相同思想和策略

   总体来说，B树、B+树它们贯彻的思路都是一样的，都是采用二分法和数据平衡策略来提升查找数据的速度。

2. 不同的方式的磁盘空间利用

## 红黑树

R-B树，又称为“红黑树”，它是一种特殊的二叉查找树。红黑树的每个节点上都有存储位标识节点的颜色，可以是红或黑。

红黑树的特性：

1. 每个节点 是黑色或者红色。

2. 根节点是黑色。

3. 每个NIL叶子节点是黑色。

4. 如果一个节点是红色的，那么它的子节点必须是黑色的。

5. 从一个节点到该节点的子孙节点上的所有路径上包含相同数目的黑节点。

   ![img](https://xuzuguang.men//images/posts/btree8.jpg)

**基本操作：**

红黑树的基本操作是添加、删除。在对红黑树进行添加或者删除之后，都会用到旋转方法，为什么呢？很简单，添加或者删除红黑树中的节点后，红黑树就发生了变化，可能不满足红黑树的5条性质，也就不再是一颗红黑树了，而是一颗普通的树。通过旋转，可以使得这颗树重新成为红黑树。简单来说，旋转的目的是让树保持红黑树的特性。旋转包括两种：**左旋**和**右旋**。

1. **左旋**

   ![img](https://xuzuguang.men//images/posts/btree9.jpg)

   对x进行左旋，意味着将x变成一个左节点

   ![img](https://xuzuguang.men//images/posts/btree10.jpg)

2. **右旋**

   ![img](https://xuzuguang.men//images/posts/btree11.jpg)

   对Y进行右旋，意味着将Y变成右节点。

   ![img](https://xuzuguang.men//images/posts/btree12.jpg)

#### [1367. 二叉树中的列表](https://leetcode-cn.com/problems/linked-list-in-binary-tree/)

难度中等33

给你一棵以 `root` 为根的二叉树和一个 `head` 为第一个节点的链表。

如果在二叉树中，存在一条一直向下的路径，且每个点的数值恰好一一对应以 `head` 为首的链表中每个节点的值，那么请你返回 `True` ，否则返回 `False` 。

一直向下的路径的意思是：从树中某个节点开始，一直连续向下的路径。

 

**示例 1：**

**![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/29/sample_1_1720.png)**

```
输入：head = [4,2,8], root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]
输出：true
解释：树中蓝色的节点构成了与链表对应的子路径。
```

**示例 2：**

**![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/29/sample_2_1720.png)**

```
输入：head = [1,4,2,6], root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]
输出：true
```

**示例 3：**

```
输入：head = [1,4,2,6,8], root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]
输出：false
解释：二叉树中不存在一一对应链表的路径。
```

 

**提示：**

- 二叉树和链表中的每个节点的值都满足 `1 <= node.val <= 100` 。
- 链表包含的节点数目在 `1` 到 `100` 之间。
- 二叉树包含的节点数目在 `1` 到 `2500` 之间。

**代码：**

```c++
// 方法：枚举 ，枚举二叉树每一个节点向下的路径中是否右和链表相匹配
// 一共四种情况：
// 1：链表匹配完 ， 匹配成功 ， 返回true
// 2: 二叉树访问到空节点，匹配失败 ， 返回false
// 3:当前匹配的值与链表中值不相等，匹配失败，返回false
// 4:匹配  继续往下遍历
bool backTrace(ListNode *head , TreeNode *root){
    // 从一个节点开始往下遍历
    if(head == NULL)
        return true;
    if(root == NULL)
        return false;
    if(head->val != root->val)
        return false;
    return backTrace(head->next , root->left) || backTrace(head->next , root->right);
}
// 递归的方式
bool isSubPath(ListNode* head, TreeNode* root) {
    if(head == NULL || root == NULL)
        return false;
    // 从根节点往下遍历  从左节点和右节点往下遍历
    return backTrace(head , root) || isSubPath(head , root->left) || isSubPath(head , root->right);
}

// 使用栈来模拟递归的方式
bool isSubPath(ListNode* head, TreeNode* root) {
    bool res = false;
    if(root == NULL)return res;
    stack<TreeNode*> st;
    st.push(root);
    while(!st.empty() && !res){
        TreeNode *tmp = st.top();
        st.pop();
        if(tmp->val == head->val)
            res = backTrace(head , tmp);
        if(tmp->left)
            st.push(tmp->left);
        if(tmp->right)
            st.push(tmp->right);
    }
    return res;
}
```

#### [102. 二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

难度中等516

给你一个二叉树，请你返回其按 **层序遍历** 得到的节点值。 （即逐层地，从左到右访问所有节点）。

 

**示例：**
二叉树：`[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

返回其层次遍历结果：

```
[
  [3],
  [9,20],
  [15,7]
]
```

**代码：**

```c++
//此题依然采用前序遍历的方式，唯一的改变是同一层的数据需要放到同一个vector中，因此针对每一层增加一个层号即可
void backTrace(TreeNode *root , int level , vector<vector<int>>& res){
    if(root == NULL) return ;
    if(res.size() < level)
        res.resize(level);
    res.at(level-1).push_back(root->val);
    backTrace(root->left , level+1 , res);
    backTrace(root->right , level+1 , res);
}
// 递归的方式
vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> res;
    backTrace(root , 1 , res);
    return res;
}
// 栈的方式
vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> res;
    if(root == NULL) return res;
    stack<pair<TreeNode* , int>> st;
    st.push(make_pair(root , 1));
    while(!st.empty()){
        TreeNode* tmp = st.top().first;
        int lev = st.top().second;
        st.pop();
        if(res.size() < lev)
            res.resize(lev);
        res.at(lev-1).push_back(tmp->val);
        if(tmp->right)
            st.push(make_pair(tmp->right , lev+1));
        if(tmp->left)
            st.push(make_pair(tmp->left , lev+1));
    }
    return res;
}
```

 [103. 二叉树的锯齿形层次遍历](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)

难度中等191

给定一个二叉树，返回其节点值的锯齿形层次遍历。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。

例如：
给定二叉树 `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

返回锯齿形层次遍历如下：

```
[
  [3],
  [20,9],
  [15,7]
]
```

**代码：**

```c++
vector<vector<int>> res;
vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
    DFS(root , 1);
    return res;
}

void DFS(TreeNode* root , int level ){
    if(root == NULL)
        return ;
    if(level > res.size())
        res.resize(level);
    //根据level 改变数据插入vector的顺序
    if(level % 2 == 0){
        res[level - 1].insert(res[level -1].begin() , root->val);
    }else{
        res[level - 1].push_back(root->val);
    }
    // 不能根据level ，改变调用DFS的顺序
    DFS(root->left , level+1);
    DFS(root->right , level+1);

}
```

#### [105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

难度中等515

根据一棵树的前序遍历与中序遍历构造二叉树。

**注意:**
你可以假设树中没有重复的元素。

例如，给出

```
前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
```

返回如下的二叉树：

```
    3
   / \
  9  20
    /  \
   15   7
```

**代码：**

```c++
/**

**/
TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
    int pos = 0;
    return DFS(preorder , pos , inorder , 0 , inorder.size()-1);
}

TreeNode* DFS(vector<int>& preorder , int& pos , vector<int>& inorder , int left , int right){
    if(pos >= preorder.size())
        return NULL;
    int i = left;
    for(; i <= right ; i++){
        if(preorder[pos] == inorder[i])
            break;
    }
    TreeNode* tmp = new TreeNode(preorder[pos]);
    if(i >= left +1){
        tmp->left = DFS(preorder , ++pos , inorder , left , i-1);
    }
    if(i <= right -1){
        tmp->right = DFS(preorder , ++pos , inorder , i+1 , right);
    }
    return tmp;
}
```

