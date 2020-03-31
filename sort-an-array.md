## LeetCode每日一题（三十一）：排序数组

给你一个整数数组 `nums`，请你将该数组升序排列。

示例 1：

```
输入：nums = [5,2,3,1]
输出：[1,2,3,5]
```

示例 2：

```
输入：nums = [5,1,1,2,0,0]
输出：[0,0,1,1,2,5]
```

提示：

* 1 <= nums.length <= 50000
* -50000 <= nums[i] <= 50000


## Solve

看到题目后，感觉就是为了提醒我：喂！这个月最后一个题了，别忘了打卡！

那就 `sort` 一下吧（滑稽）。

```c++
class Solution {
public:
    vector<int> sortArray(vector<int>& nums) {
        sort(nums.begin(), nums.end()); // 不行，这样太堕落了
        return nums;
    }
};
```


开玩笑，怎么可能就酱完事儿。首先，列出常用的排序算法。


|   算法种类   | 最好情况 | 平均情况 | 最坏情况 | 空间复杂度 | 是否稳定 |
|--------------|----------|----------|----------|------------|----------|
| 直接插入排序 | n        | n^2      | n^2      | 1          | ✓        |
| 冒泡排序     | n        | n^2      | n^2      | 1          | ✓        |
| 简单选择排序 | n^2      | n^2      | n^2      | 1          |          |
| 希尔排序     | nlogn    | n^{4/3}  | n^{4/3}  | 1          |          |
| 快速排序     | nlogn    | nlogn    | n^2      | logn       |          |
| 堆排序       | nlogn    | nlogn    | nlogn    | 1          |          |
| 归并排序     | nlogn    | nlogn    | nlogn    | n          | ✓        |
| 基数排序     | -        | n*k/d    | n*k/d    | n+2^d      | ✓        |
| 计数排序     | -        | n+r      | n+r      | n+r        | ✓        |
| 桶排序       | -        | n+r      | n+r      | n+r        | ✓        |
| 二叉树排序   | nlogn    | nlogn    | n^2      | n          | ✓        |


根据 `leetcode` 中测试，发现冒泡排序、选择排序、插入排序都超时了。好吧，只能用时间复杂度较低的了。

然而我会的也就那么几个了...（大哭）


#### 1.快速排序

**思想：** 以最后一个元素作为划分元素，大于等于它的放右边，小的放左边。

```c++
class Solution {
public:
    int Partition(vector<int>& nums, int low, int high) {
        int pivot = nums[low]; // 当前表中第一个元素设为枢轴值，对表进行划分
        while(low < high) {
            while(low < high && nums[high] >= pivot)
                --high;
            nums[low] = nums[high];
            while(low < high && nums[low] <= pivot)
                ++low;
            nums[high] = nums[low];
        }
        nums[low] = pivot;
        return low;
    }
    void QuickSort(vector<int>& nums, int low, int high) {
        if(low < high) {
            int pivotpos = Partition(nums, low, high); // Partition 是划分操作，将表划分为两个子表
            QuickSort(nums, low, pivotpos - 1);
            QuickSort(nums, pivotpos + 1, high);
        }
    }

    vector<int> sortArray(vector<int>& nums) {
        QuickSort(nums, 0, nums.size()-1);
        return nums;
    }
};
```


#### 2.堆排序

**思路：**有点类似选择排序，先划分到最小每组只有一个元素，然后合并这两个元素，选择小的放到辅助数组中。依次进行。

```c++
class Solution {
public:
    // 建立大根堆
    void AdjustDown(vector<int>& nums, int index, int size) {
        while(index * 2 + 1 < size) {
            int j = index * 2 + 1;
            if(j < size && j + 1 < size && nums[j+1] > nums[j]) j++;
            if(nums[index] >= nums[j]) break;
            swap(nums[index], nums[j]);
            index = j;
        }
    }
    void BuildMaxHeap(vector<int>& nums) {
        int size = nums.size();
        // size/2 -1 是最后一个非叶子节点，它以下都是叶子节点，不用下沉了
        for(int i = size / 2 - 1; i >= 0; i--) // 这里为什么等于0？当运行至根结点时，就找到根的孩子节点
            AdjustDown(nums, i, size);
    }
    // 堆排序
    void HeapSort(vector<int>& nums) {
        int size = nums.size();
        BuildMaxHeap(nums);
        for(int i = size - 1; i > 0; i--) {
            swap(nums[0], nums[i]);
            AdjustDown(nums, 0, i);
        }
    }
    vector<int> sortArray(vector<int>& nums) {
        HeapSort(nums);
        return nums;
    }
};
```


#### 3.计数排序

**思想：**从名字就可以看出算法的思路。把每个出现的数值都做一个计数，然后根据计数从小到大输出得到有序数组。

```c++
class Solution {
public:
    vector<int> CountSort(vector<int> &nums) {
        int max = nums[0];
        int min = nums[0];
        for(auto e : nums) {
            max = max > e ? max : e;
            min = min < e ? min : e;
        }
        int size = max - min + 1;
        vector<int> vcount(size, 0);
        for(auto e : nums) {
            vcount[e-min]++;//使用元素作为新数组索引
        }
        
        for(int i = 1; i < size; i++) {
            vcount[i] += vcount[i-1];//计算不大于当前元素的个数，即这个元素的最终位置
        }
        vector<int> res(nums.size(), 0);
        for(auto e : nums) {
            int index = --vcount[e-min];//重复元素递减
            res[index] = e;//放入最终返回数组
        }
        return res;
    }

    vector<int> sortArray(vector<int>& nums) {
        nums = CountSort(nums);
        return nums;
    }
};
```

<div align="center">
    <hr style="height:1px;"/>
    <br>
    <img width="200px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/wxgzh-hang.png"></img>
</div>
