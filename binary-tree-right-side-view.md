## LeetCode每日一题：二叉树的右视图

给定一棵二叉树，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

示例:

```
输入: [1,2,3,null,5,null,4]
输出: [1, 3, 4]
解释:

   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
```


## Solve

#### 1. BFS

```c++
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        if(!root) return {};
        // bfs 层序遍历 将每层最后一个加入结果数组
        vector<int> ans;
        queue<TreeNode*> que;
        que.push(root);
        while(!que.empty())
        {
            int counts = que.size();
            // 当前层结点遍历
            for(int i = 0; i < counts; ++i)
            {
                auto node = que.front();
                que.pop();

                if(node -> left)
                    que.push(node -> left);
                if(node -> right)
                    que.push(node -> right);
                if(i == counts - 1)
                    ans.push_back(node->val);
            }
        }
        return ans;
    }
};
```

* 时间复杂度：O(n)
* 空间复杂度：O(n)


#### 2. DFS

```c++
class Solution {
public:
    void solve(TreeNode *root, int high, vector<int> &num) {
        if(root == NULL) return;
        if(high > num.size()) {
            num.push_back(root -> val);
        }
        solve(root -> right, high+1, num);
        solve(root -> left, high+1, num);
    }
    vector<int> rightSideView(TreeNode *root) {
        vector<int> num;
        solve(root, 1, num);
        return num;
    }
}
```

* 时间复杂度：O(n)
* 空间复杂度：O(n)


