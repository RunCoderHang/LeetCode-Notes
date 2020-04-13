## LeetCode : Contiguous Array

Given a binary array, find the maximum length of a contiguous subarray with equal number of 0 and 1.

**Example 1:**

```
Input: [0,1]
Output: 2
Explanation: [0, 1] is the longest contiguous subarray with equal number of 0 and 1.
```

**Example 2:**

```
Input: [0,1,0]
Output: 2
Explanation: [0, 1] (or [1, 0]) is a longest contiguous subarray with equal number of 0 and 1.
```

**Note:** The length of the given binary array will not exceed 50,000.


## Solve

#### 1.暴力解法 (Brute Force) [Time Limit Exceeded]


```c++
class Solution {
    int findMaxLength(vector<int>& nums) {
        int size = nums.size();
        int maxlen = 0;
        for (int i = 0; i < size; i++) {
            int zeroes = 0, ones = 0;
            for (int j = i; j < size; j++) {
                if (nums[j] == 0) {
                    zeroes++;
                } else {
                    ones++;
                }
                if (zeroes == ones) {
                    maxlen = max(maxlen, j - i + 1);
                }
            }
        }
        return maxlen;
    }
};
```

#### 2.哈希表 (Hash Table)

原问题可转化为求最长子数组和为0(0看为-1)

我们定义一个 `unordered_map` 用于存放子数组的和及其对应的值

每次更新结果 maxlen 与和为1的子数组长度中的最大值，即为所求。


```c++
class Solution {
public:
    int findMaxLength(vector<int>& nums) {
        int size = nums.size();
        int maxlen = 0;
        unordered_map<int, int> hash;
        hash[0] = 0;
        int x = 0; // 子数组的和
        for(int i = 0; i < size; i++) {
            if(nums[i]) x++;
            else x--;
            if(hash.find(x) != hash.end()) 
                maxlen = max(maxlen, i - hash[x] + 1);
            else
                hash[x] = i + 1;
        }
        return maxlen;
    }
};
```

* Time complexity : O(n)
* Space complexity : O(n)

