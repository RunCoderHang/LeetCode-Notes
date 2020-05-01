## LeetCode : Check If a String Is a Valid Sequence from Root to Leaves Path in a Binary Tree

Given a binary tree where each path going from the root to any leaf form a **valid sequence**, check if a given string is a **valid sequence** in such binary tree. 

We get the given string from the concatenation of an array of integers `arr` and the concatenation of all values of the nodes along a path results in a **sequence** in the given binary tree.


**Example 1:**

<div align="left">
    <img width="333px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/leetcode_testcase_1.png"></img>
</div>


```
Input: root = [0,1,0,0,1,0,null,null,1,0,0], arr = [0,1,0,1]
Output: true
Explanation: 
The path 0 -> 1 -> 0 -> 1 is a valid sequence (green color in the figure). 
Other valid sequences are: 
0 -> 1 -> 1 -> 0 
0 -> 0 -> 0
```


**Example 2:**

<div align="left">
    <img width="333px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/leetcode_testcase_2.png"></img>
</div>

```
Input: root = [0,1,0,0,1,0,null,null,1,0,0], arr = [0,0,1]
Output: false 
Explanation: The path 0 -> 0 -> 1 does not exist, therefore it is not even a sequence.
```


**Example 3:**

<div align="left">
    <img width="333px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/leetcode_testcase_3.png"></img>
</div>

```
Input: root = [0,1,0,0,1,0,null,null,1,0,0], arr = [0,1,1]
Output: false
Explanation: The path 0 -> 1 -> 1 is a sequence, but it is not a valid sequence.
```

**Constraints:**

* `1 <= arr.length <= 5000`
* `0 <= arr[i] <= 9`
* Each node's value is between `[0 - 9]`.


## Solve

#### 递归

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool isValid(TreeNode* root, vector<int>& arr, int idx){
        if(root -> val != arr[idx]) return false;
        if(idx == arr.size() - 1)
            return !root -> left && !root -> right;
        return ((root -> left && isValid(root -> left, arr, idx+1))||
                (root -> right && isValid(root -> right, arr, idx+1)));
    }
    
    bool isValidSequence(TreeNode* root, vector<int>& arr) {
        if(root == NULL) return arr.size() == 0;
        return isValid(root, arr, 0);
    }
};
```
