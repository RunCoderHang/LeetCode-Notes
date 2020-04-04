## LeetCode : Move Zeros

Given an array `nums`, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.

**Example:**

```
Input: [0,1,0,3,12]
Output: [1,3,12,0,0]
```

Note:

* You must do this in-place without making a copy of the array.
* Minimize the total number of operations.

## Solve

#### 默哀

<div align="center">
    <img width="661px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/silence-tribute.jpg"></img>
    <p style="font-weight: 600; text-align: center;">英烈为国家舍生忘死，逝者同胞亦盼山河无恙。</p>
</div>

<br>

#### 1.

定义额外的指针 `zeroPosition`，用于指向 0 元素。每当遍历至 0 元素时，使用该指针指向 0 元素，都会与后面的非零元素进行替换。

最后所得的数组不会打乱顺序，但是如果给 0 编号顺序的话，后面的 0 元素的顺序是打乱的。

```c++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int size = nums.size();
        if(size == 0) return;
        for(int zeroPosition = 0, cur = 0; i < size; i++) {
            if(nums[cur] != 0) {
                swap(nums[zeroPosition], nums[cur]);
                zeroPosition++;
            }
        }
    }
};
```

* Space Complexity : **O(1)**
* Time Complexity: **O(n)**


#### 2.

同样是定义额外的指针 `zeroPosition` 。不同的是其原理是将所有的非零元素向前按顺序复制。

该指针指向第一个 0 元素后，后面的元素则会将 0 赋值覆盖。但同时会出现两个连续相同的元素，这时需要将后面非零元素依次向前赋值移动。

最后，`zeroPosition` 指向的位置以及之后的位置都是剩余的应该存放 0 元素的位置。

```c++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int size = nums.size();
        if(size == 0) return;
        int zeroPosition = 0;
        for(int i = 0; i < size; i++) {
            if(nums[i] != 0) {
                nums[zeroPosition] = nums[i];
                zeroPosition++;
            }
        }
        for(int i = zeroPosition; i < size; i++) {
            nums[i] = 0;
        }
    }
};
```

* Space Complexity : **O(1)**
* Time Complexity: **O(n)**



