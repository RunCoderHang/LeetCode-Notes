## LeetCode : Search in Rotated Sorted Array

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., `[0,1,2,4,5,6,7]` might become `[4,5,6,7,0,1,2]`).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.

Your algorithm's runtime complexity must be in the order of **O(log n)**.

**Example 1:**

```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

**Example 2:**

```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```



## Solve

题意：将一个升序数组从中间某个未知元素 pivot 分成两部分旋转，形成：前面升序序列元素大于后面的升序序列元素。

#### C++ STL

皮一下。

使用C++标准库中的 `max_element()` 找到最大最大值的位置。如果数组第一个元素比 target 大，说明 target 在最大值的后面。否则，在最大值的前面。

然而 `max_element()` 的时间复杂度为 O(n)，不符合题意。

```c++
#include <algorithm>

class Solution {
public:
    int search(vector<int>& nums, int target) {
        int size = nums.size();
        if(size == 0) return -1;
        int maxIndex = max_element(nums.begin(), nums.end()) - nums.begin();
        int index = 0;
        while(index < size) {
            if(target > nums[index]) {
                index++;
            }
            else if(target < nums[index]) {
                index = maxIndex;
                maxIndex++;
                index++;
            }
            else return index;
        }
        return -1;
    }
};
```

* Time complexity: O(n)
* Space complexity: O(1)


**优化**

根据上面的思路，我们可以利用二分法从中间寻找找，虽不一定是最大值，但可以利用开始元素、最后元素与 target 比较，判断 target 位置。

我们先将数组旋转的两个部分分别成为： **A** 、 **B** 。 A 中的元素大于 B 的元素，且所有元素都升序。

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int size = nums.size();
        if(size == 0) return -1;
        int left = 0, right = size - 1;
        while(left <= right) {
            int mid = (left + right) / 2;
            // 中间值就是目标，返回下标
            if(nums[mid] == target) return mid;

            // 中间值小于开始值，说明中间值在 B
            if(nums[mid] < nums[left]) {
                // 目标值小于开始值且大于中间值，向中间值右边
                if(target <= nums[right] && target > nums[mid]) {
                    left = mid + 1;
                // 否则，向中间值左边
                } else {
                    right = mid - 1;
                }

            // 中间值大于最后值，说明中间值在 A 或者 B
            } else if(nums[mid] > nums[right]) {
                // 目标值大于开始值且小于中间值，目标则在 A 中。向中间值左边找
                if(target >= nums[left] && target < nums[mid]) {
                    right = mid - 1;
                // 否则向中间值右边找
                } else {
                    left = mid + 1;
                }

            // 中间值小于等于最后值且大于等于开始值，说明没有发生旋转，元素全部升序
            } else {
                if(target > nums[mid])
                    left = mid + 1;
                else 
                    right = mid - 1;
            }
        }
        return -1;
    }
}
```

* Time complexity: O(logn)
* Space complexity: O(1)


