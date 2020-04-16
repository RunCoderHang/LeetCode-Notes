## LeetCode每日一题：合并区间

给出一个区间的集合，请合并所有重叠的区间。

示例 1:

```
输入: [[1,3],[2,6],[8,10],[15,18]]
输出: [[1,6],[8,10],[15,18]]
解释: 区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
```

示例 2:

```
输入: [[1,4],[4,5]]
输出: [[1,5]]
解释: 区间 [1,4] 和 [4,5] 可被视为重叠区间。
```



## Solve

▄█▀█●，这个表情很好玩，很像我在看大佬的题解时的状态。

#### 排序 + 双指针

先将二维数组中的元素进行排序，利用两个变量记录区间的左右端点。

从第一组区间开始循环遍历，比较区间的右端点与下一组区间的左端点大小。例如： `[1, 4], [3, 5], [2, 6]` ，需要循环遍历判断最后合并成 `[1, 6]` 。

```c++
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        int size = intervals.size();
        vector<vector<int>> res;
        if(size == 0) return res;
        sort(intervals.begin(), intervals.end());
        for(int i = 0; i < size; i++) {
            int left = intervals[i][0];
            int right = intervals[i][1];
            while(i < size - 1 && intervals[i+1][0] <= right) {
                right = max(right, intervals[i+1][1]);
                i++;
            }
            res.push_back({left, right});
        }
        return res;
    }
};
```

* 时间复杂度：O(nlog n)，其中 n 为区间的数量。除去排序的开销，我们只需要一次线性扫描，所以主要的时间开销是排序的 O(nlogn)。
* 空间复杂度：O(log n)，为排序所需要的空间复杂度。

