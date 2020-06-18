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

#### 