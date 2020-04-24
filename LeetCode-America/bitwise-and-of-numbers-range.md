## LeetCode : Bitwise AND of Numbers Range

Given a range `[m, n]` where 0 <= m <= n <= 2147483647, return the bitwise AND of all numbers in this range, inclusive.

**Example 1:**

```
Input: [5,7]
Output: 4
```

**Example 2:**

```
Input: [0,1]
Output: 0
```


## Solve

#### 1. 暴力解法 (超时)

```c++
class Solution {
public:
    int rangeBitwiseAnd(int m, int n) {
        int res = m;
        for(int i = m + 1; i <= n; i++) {
            res &= i;
        }
        return res;
    }
};
```


#### 2.

```c++
class Solution {
public:
    int rangeBitwiseAnd(int m, int n) {
        int offset = 0;
        while(m < n) {
            m >>= 1;
            n >>= 1;
            offset++;
        }
        return m<<offset;
    }
};
```

