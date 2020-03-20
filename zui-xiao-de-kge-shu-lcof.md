## LeetCode每日一题（二十）：最小的k个数

输入整数数组 arr ，找出其中最小的 k 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。

示例 1：

```
输入：arr = [3,2,1], k = 2
输出：[1,2] 或者 [2,1]
```

示例 2：

```
输入：arr = [0,1,2,1], k = 1
输出：[0]
```

限制：

* 0 <= k <= arr.length <= 10000
* 0 <= arr[i] <= 10000

## Solve

#### 1. `c++` 自带的 `sort()` 排序

本题主要是考察排序，尽量选择复杂度少的排序。

```c++
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        vector<int> res;
        sort(arr.begin(), arr.end());
        res.assign(arr.begin(), arr.begin()+k);
        return res;
    }
};
```

* 时间复杂度：O(nlogn)
* 空间复杂度：O(logn)

#### 2.快速排序

可惜，实际测试中我的快排的时间复杂度没有 `sort` 快。

```c++
class Solution {
    void quickSort(vector<int>& nums, int low, int high) {
        if(low < high) {
            int pivotpos = Partition(nums, low, high);
            quickSort(nums, low, pivotpos - 1);
            quickSort(nums, pivotpos + 1, high);
        }
    }
    int Partition(vector<int>& nums, int low, int high) {
        int pivot = nums[low];
        while(low < high) {
            while(low < high && nums[high] >= pivot) --high;
            nums[low] = nums[high];
            while(low < high && nums[low] <= pivot) ++low;
            nums[high] = nums[low];
        }
        nums[low] = pivot;
        return low;
    }
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        vector<int> res;
        int left = 0;
        int right = arr.size() - 1;
        quickSort(arr, left, right);
        for(int i = 0; i < k; i++) {
            res.push_back(arr[i]);
        }
        return res;
    }
};
```

* 时间复杂度：O(nlogn)
* 空间复杂度：O(logn)

#### 3.堆排序

```c++
class Solution {
    // 堆排序
    void sink(vector<int> &a, int index, int size) {
        while(index * 2 + 1 < size) {//是否已经是叶子节点
            int j = index * 2 + 1;          
            if(j < size && j+1 < size && a[j+1] > a[j]) 
                j++;
            if(a[index] >= a[j]) break;
            swap(a[index], a[j]);
            index = j;
        }
    }
    //给定一个数组，建立大根堆
    void heapify(vector<int> &a) {
        int size = a.size();
        for(int i = size / 2 - 1; i >= 0; i--) {//size/2 -1 是最后一个非叶子节点，它以下都是叶子节点，不用下沉了
            sink(a, i, size);
        }
    }
    void heapSort(vector<int> &a) {
        int N = a.size();
        heapify(a);//建立大根堆
        while(N > 1) {
            swap(a[0], a[--N]);//选择最大元素（根）
            sink(a, 0, N);//修复堆
        }
    }
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        heapSort(arr);
        for(int i = 0; i < k; i++) {
            res.push_back(arr[i]);
        }
        return res;
    }
};
```

* 时间复杂度：O(nlogn)
* 空间复杂度：O(1)


<div align="center">
    <hr style="height:1px;"/>
    <br>
    <img width="200px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/wxgzh-hang.png"></img>
</div>
