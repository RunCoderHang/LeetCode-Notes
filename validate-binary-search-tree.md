## LeetCode每日一题：验证二叉搜索树

给定一个二叉树，判断其是否是一个有效的二叉搜索树。

假设一个二叉搜索树具有如下特征：

* 节点的左子树只包含小于当前节点的数。
* 节点的右子树只包含大于当前节点的数。
* 所有左子树和右子树自身必须也是二叉搜索树。


**示例 1:**

```
输入:
    2
   / \
  1   3
输出: true
```


**示例 2:**

```
输入:
    5
   / \
  1   4
     / \
    3   6
输出: false
解释: 输入为: [5,1,4,null,null,3,6]。
     根节点的值为 5 ，但是其右子节点值为 4 。
```


## 解题

中序遍历

#### 递归

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    long preVal = LONG_MIN;
    bool flag = true;
    bool isValidBST(TreeNode* root) {
        inOrderTravel(root);
        return flag;
    }
    void inOrderTravel(TreeNode* root) {
        if(root == NULL) return;
        inOrderTravel(root -> left);
        if(root -> val <= preVal) {
            flag = false;
            return;
        }
        preVal = root -> val;
        inOrderTravel(root -> right);
    }
};
```

* 时间复杂度： O(n)
* 空间复杂度： O(n)

#### 非递归

```c++
class Solution {
public:
    long preVal = LONG_MIN;
    bool flag = true;
    bool isValidBST(TreeNode* root) {
        inOrderTravel(root);
        return flag;
    }
    void inOrderTravel(TreeNode* root) {
        stack<TreeNode*> stk;
        TreeNode *p = root;
        while(p || !stk.empty()) {
            if(p) {
                stk.push(p);
                p = p -> left;
            } else {    
                p = stk.top();
                if(p -> val <= preVal) flag = false;
                preVal = p -> val;
                stk.pop();
                p = p -> right;
            }
        }
    }
};
```

* 时间复杂度： O(n)
* 空间复杂度： O(n)



