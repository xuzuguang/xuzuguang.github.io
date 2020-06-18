# LeetCode 5-25

#### [94. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

难度中等501

给定一个二叉树，返回它的*中序* 遍历。

**示例:**

```
输入: [1,null,2,3]
   1
    \
     2
    /
   3

输出: [1,3,2]
```

**代码:**

~~~c++
// 递归方式
// 中序遍历   左 -> 中 -> 右
vector<int> res;
void DFS(TreeNode* root){
    if(root == NULL)
        return;
    DFS(root->left);
    res.push_back(root->val);
    DFS(root->right);
}
vector<int> inorderTraversal(TreeNode* root) {
    DFS(root);
    return res;
}
~~~

~~~c++
//迭代方式
int WITHE = 0 , GRAY = 1;
vector<int> inorderTraversal(TreeNode* root) {
    vector<int> res;
    stack<pair<TreeNode* , int> > st;
    st.push(std::make_pair(root , WITHE));
    while(!st.empty()){
        TreeNode* tmp = st.top().first;
        int nT = st.top().second;
        st.pop();
        if(tmp == NULL)
            continue;
        if(nT == WITHE){
            st.push(std::make_pair(tmp->right , WITHE));
            st.push(std::make_pair(tmp , GRAY));
            st.push(std::make_pair(tmp->left , WITHE));
        }else
            res.push_back(tmp->val);
    }
    return res;
}
~~~

