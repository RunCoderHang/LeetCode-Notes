## LeetCode : First Bad Version

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have `n` versions `[1, 2, ..., n]` and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API `bool isBadVersion(version)` which will return whether `version` is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

**Example:**

```
Given n = 5, and version = 4 is the first bad version.

call isBadVersion(3) -> false
call isBadVersion(5) -> true
call isBadVersion(4) -> true

Then 4 is the first bad version. 
```


## Solve

看似题目很长，其实就是张纸老虎。

#### 1. 迭代 (Linear Scan)

```c++
// The API isBadVersion is defined for you.
// bool isBadVersion(int version);

class Solution {
public:
    int firstBadVersion(int n) {
        for(int i = 1; i < n; i++) {
            if(isBadVersion(i)) return i;
        }
        return n;
    }
};
```

* Time complexity: O(n)
* Space complexity: O(1)


#### 2. 二分查找 (Binary Search)

数组递增，可以用二分查找。

`mid = left + (right - left) / 2` 的意义在于：`left` 和 `right` 相加有可能超过 `INT_MAX` 。

```c++
class Solution {
public:
    int firstBadVersion(int n) {
        int left = 1, right = n;
        while(left < right) {
            int mid = left + (right - left) / 2;
            if(isBadVersion(mid)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
};
```

* Time complexity: O(logn)
* Space complexity: O(1)


