## LeetCode : Diameter of Binary Tree

Given a binary tree, you need to compute the length of the diameter of the tree. The diameter of a binary tree is the length of the **longest** path between any two nodes in a tree. This path may or may not pass through the root.

**Example:**

Given a binary tree

```
          1
         / \
        2   3
       / \     
      4   5    
```

Return **3**, which is the length of the path [4,2,1,3] or [5,2,1,3].

**Note:** The length of path between two nodes is represented by the number of edges between them.


## Solve

**深度优先搜索(DFS)**

根据题目的说明，要找最大直径长度，但不一定要过根结点。如下图：

<div align="center">
    <br>
    <img width="276px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/diameter-of-binary-tree01.png"></img>
    <br>
</div>

根据上图二叉树，我们可以找到它的最大直径长度：3 。其中有 `[3, 2, 5, 4]` 和 `[3, 2, 5, 6]` 两个路径。

为什么我们要使用深度优先搜索的方法？这里的深度优先搜索类似于 **树的后序遍历（左右根）** ，它是尽可能“深”地搜索一棵树，直到所有结点均被访问过。最后我们可以得出这棵树的高度与深度。

首先区别一下什么是 “树的高度与深度” 还有 “结点的深度与高度” 。

- **结点的高度** ：是从叶子结点开始自底向上逐层累加的。
- **结点的深度** ：是从结点开始开始自顶向下逐层累加的 **（本题不计入该结点这一层）**
- **树的高度（深度）**：就是树中结点的最大层数。

<div align="center">
    <br>
    <img width="535px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/diameter-of-binary-tree02.png"></img>
    <br>
</div>

而本题中，我们对二叉树进行递归遍历，寻找某结点左右子树的最大深度。即每次都返回： **`max(L, R) + 1`**

左右子树的最大深度值进行计算：**`L + R + 1`**，可以得出该二叉树的最大直径。

<div align="center">
    <br>
    <img width="280px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/diameter-of-binary-tree03.gif"></img>
    <br>
</div>

**Code:**

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
    int diam = 0;
public:
    int dfs(TreeNode* root) {
        if(root == NULL) return 0;
        int leftDepth = dfs(root -> left);
        int rightDepth = dfs(root -> right);
        
        diam = max(leftDepth + rightDepth, diam);
        
        return max(leftDepth, rightDepth) + 1;
    }
    
    int diameterOfBinaryTree(TreeNode* root) {
        if(root != NULL){
            dfs(root);
            return diam;
        }
        return 0;
    }
};
```


* Time Complexity: O(n)
* Space Complexity: O(n)


