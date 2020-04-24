## LeetCode每日一题：数组中的逆序对

在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。


示例 1:

```
输入: [7,5,6,4]
输出: 5
```

限制：

* `0 <= 数组长度 <= 50000`


## Solve

#### 归并排序

```c++
class Solution {
public:
    int reversePairs(vector<int>& nums) {
        vector<int> temp(nums.size());
        return mergeSort(nums, 0, nums.size() - 1, temp);
    }

private:
    int mergeSort(vector<int>& nums, int begin, int end, vector<int>& temp) {
        if (begin < end) {
            int mid = (begin + end)/2;
            int ret = mergeSort(nums, begin, mid, temp) + mergeSort(nums, mid + 1, end, temp);

            int i = begin;
            int j = mid + 1;
            for (int k = 0; k <= end - begin; k++) {
                if (i <= mid && (j > end || nums[i] <= nums[j])) {
                    temp[k] = nums[i++];
                    ret += (j - mid - 1);
                } else {
                    temp[k] = nums[j++];
                }
            }

            memcpy(&nums[begin], &temp[0], (end - begin + 1) * sizeof(int));
            return ret;
        }
        return 0;
    }
};
```

* 时间复杂度：O(nlogn)。
* 空间复杂度：O(n)
