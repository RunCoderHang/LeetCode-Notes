## LeetCode : Product of Array Except Self

Given an array `nums` of n integers where n > 1,  return an array `output` such that `output[i]` is equal to the product of all the elements of `nums` except `nums[i]`.

**Example:**

```
Input:  [1,2,3,4]
Output: [24,12,8,6]
```

**Constraint:** It's guaranteed that the product of the elements of any prefix or suffix of the array (including the whole array) fits in a 32 bit integer.

**Note:** Please solve it **without division** and in O(n).

**Follow up:**

Could you solve it with constant space complexity? (The output array does not count as extra space for the purpose of space complexity analysis.)


## Solve

#### 1.假如除法可用

虽然时间复杂度为 `O(n)` ，但是违反了规则，因为规则上说 **without division** 不能用除法。

如果除法可用的话，就直接考虑0的个数：

* 个数为0时，用总乘积除以每个元素；
* 个数为1时，为0的元素则替换为无零乘积，其他元素都为0；
* 个数大于1时，全部元素都为0；

```c++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int size = nums.size();
        if(size == 0) return nums;
        int product = 1;
        int cnt = 0;
        for(int i = 0; i < size; i++) {
            if(nums[i] != 0) {
                product *= nums[i];
            } else {
                cnt++;
            }
        }
        if(cnt == size) return nums;
        for(int i = 0; i < size; i++) {
            if(cnt == 0) {
                nums[i] = product / nums[i];
            } else if(cnt == 1) {
                if(nums[i] != 0)
                    nums[i] = 0;
                else
                    nums[i] = product;
            } else {
                nums[i] = 0;
            }
        }
        return nums;
    }
};
```

* Time complexity : O(n)
* Space complexity : O(1)

#### 2.双辅助空间

```c++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int size = nums.size();
        if(size == 0) return nums;
        int right[size];
        int left[size];
        vector<int> res(size, 0);
        left[0] = 1;
        for(int i = 1; i < size; i++)
            left[i] = nums[i-1] * left[i-1];
        
        right[size-1] = 1;
        for(int i = size-2; i >= 0; i--)
            right[i] = nums[i+1] * right[i+1];
        
        for(int i = 0; i < size; i++)
            res[i] = left[i] * right[i];
        
        return res;
    }
};
```

* Time complexity : O(n)
* Space complexity : O(n)


#### 3.优化：一个辅助空间

```c++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int size = nums.size();
        if(size == 0) return nums;
        vector<int> res(size, 0);
        res[0] = 1;
        for(int i = 1; i < size; i++)
            res[i] = nums[i-1] * res[i-1];
        
        int right = 1;
        for(int i = size-1; i >= 0; i--) {
            res[i] = res[i] * right;
            right *= nums[i];
        }
        return res;
    }
};
```

* Time complexity : O(n)
* Space complexity : O(1)

