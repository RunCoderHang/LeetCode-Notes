## LeetCode每日一题：括号生成

数字 n 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 有效的 括号组合。


示例：

```
输入：n = 3
输出：[
       "((()))",
       "(()())",
       "(())()",
       "()(())",
       "()()()"
     ]
```



## Solve

根据提议我们可以得出如下结论：

1. 当前左右括号都有大于0个可以使用的时候，才产生分支
2. 右边剩余的括号数量一定严格大于左边剩余的括号数量的时候，才可以产生分支
3. 左边和右边的括号数量都等于0的时候结算



```c++
class Solution {
public:
    void dfs(string cur_string, int left, int right, int n, vector<string>& res) {
        if(left == n && right == n) {
            res.push_back(cur_string);
            return;
        }

        // 剪枝。此时右边剩余的括号数量小于左边剩余的括号数量
        if(left < right) {
            return;
        }

        if(left < n) {
            dfs(cur_string + "(", left + 1, right, n, res);
        }

        if(right < n) {
            dfs(cur_string + ")", left, right + 1, n, res);
        }
    }
    
    vector<string> generateParenthesis(int n) {
        vector<string> res;
        if(n == 0) return res;
        dfs("", 0, 0, n, res);
        return res;
    }
};
```
