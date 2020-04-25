## LeetCode每日一题：全排列

给定一个 没有重复 数字的序列，返回其所有可能的全排列。

示例:

```
输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```


## Solve

#### 1. 回溯算法

```c++
class Solution {
public:
    void dfs(vector<vector<int>> &res, vector<int> &re, vector<int> &used, vector<int> &nums) {
        if(re.size() == nums.size()) {
            res.push_back(re);
            return;
        }
        for(int i = 0; i < nums.size(); i++) {
            if(used[i] != 0) 
                continue;
            else {
                re.push_back(nums[i]);
                used[i] = 1;
                
                dfs(res, re, used, nums);

                re.pop_back();
                used[i] = 0;
            }
        }
    }

    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> res;
        vector<int> re;
        vector<int> used(nums.size(), 0);
        dfs(res, re, used, nums);
        return res;
    }
};
```

* 时间复杂度：O(n * n!)
* 空间复杂度：O(n)


#### 2. STL next_permutation

```c++
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> result;
        do {
            result.emplace_back(nums);
        } while (next_permutation(nums.begin(), nums.end()));
        return result;
    }
};
```
