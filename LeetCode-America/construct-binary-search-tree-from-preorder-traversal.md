## LeetCode : Construct Binary Search Tree from Preorder Traversal


Return the root node of a binary **search** tree that matches the given `preorder` traversal.

*(Recall that a binary search tree is a binary tree where for every node, any descendant of `node.left` has a value < `node.val`, and any descendant of `node.right` has a value > `node.val`.  Also recall that a preorder traversal displays the value of the `node` first, then traverses `node.left`, then traverses `node.right`.)*


**Example 1:**

```
Input: [8,5,1,7,10,12]
Output: [8,5,10,1,7,null,12]
```

<div align="center">
    <img width="500px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/construct-binary-search-tree-from-preorder-traversal.png"></img>
</div>


**Note:**

* `1 <= preorder.length <= 100`
* The values of `preorder` are distinct.

#### 1. 迭代

先建立头结点，值为数组的第一个元素。另外定义指向头结点的指针 `p` 。从而在构建二叉查找树时，`p` 指针引导元素插入。

不过，插入完元素后 `p` 指针都会会到根结点。因此，这是边遍历边构建二叉树的过程。

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
    TreeNode* bstFromPreorder(vector<int>& preorder) {
        if(preorder.size() == 0) return nullptr;
        TreeNode *root = new TreeNode(preorder[0]);
        TreeNode *p = root;
        for(int i = 1; i < preorder.size(); i++) {
            while((preorder[i] < p -> val && p -> left != NULL) || (preorder[i] > p -> val && p -> right != NULL)) {
                if(preorder[i] < p -> val)
                    p = p -> left;
                else
                    p = p -> right;
            }
            if(preorder[i] < p -> val) {
                p -> left = new TreeNode(preorder[i]);
            } else {
                p -> right = new TreeNode(preorder[i]);
            }
            p = root;
        }
        return root;
    }
};

```

* Time complexity: O(nlogn)
* Space complexity: O(1)



#### 2. 递归

```c++
class Solution {
    int index = 0;
public:
    TreeNode* constructTree(TreeNode *root, int val) {
        if(root == nullptr) 
            return new TreeNode(val);
        else if(val < root -> val)
            root -> left = constructTree(root -> left, val);
        else
            root -> right = constructTree(root -> right, val);

        return root;
    }
    TreeNode* bstFromPreorder(vector<int>& preorder) {
        TreeNode *root = new TreeNode();
        if(index == preorder.size()) return root;
        root -> val = preorder[0];
        for(int i = 1; i < preorder.size(); i++) {
            constructTree(root, preorder[i]);
        }
        return root;
    }
};
```

* Time complexity: O(nlogn)
* Space complexity: O(n)

