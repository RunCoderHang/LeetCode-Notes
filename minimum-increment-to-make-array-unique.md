## LeetCode每日一题（二十二）：使数组唯一的最小增量

给定整数数组 `A` ，每次 `move` 操作将会选择任意 `A[i]` ，并将其递增 `1` 。

返回使 `A` 中的每个值都是唯一的最少操作次数。

示例 1:

```
输入：[1,2,2]
输出：1
解释：经过一次 move 操作，数组将变为 [1, 2, 3]。
```

示例 2:

```
输入：[3,2,1,2,1,7]
输出：6
解释：经过 6 次 move 操作，数组将变为 [3, 4, 1, 2, 5, 7]。
可以看出 5 次或 5 次以下的 move 操作是不能让数组的每个值唯一的。
```

提示：

* `0 <= A.length <= 40000`
* `0 <= A[i] < 40000`

## Solve

1. 先排序，使数组中数字呈递增状态。

2. 逐个遍历数组元素，如果元素中有相同的元素，不可避免需要 `move` 。而 `move` 后，可能会大于后面的元素。

3. 找到因为 `move` 而打乱递增的元素，与后面的元素相减并且 `+1` ，可以得到后面元素需要 `move` 的增量。依次类推。

4. 所得增量全部相加，结果为最小增量。

```c++
class Solution {
public:
    int minIncrementForUnique(vector<int>& A) {
        if(A.size() == 0){
            return 0;
        }
        sort(A.begin(), A.end());
        int cur = A[0];
        int count = 0;
        int addNum = 0; //记录需要做的move操作的增量
        for(int i = 0; i < A.size() - 1; i++) { //当后面元素比前面的大，说明是前面元素做move操作导致的，后面的元素仍需要做move操作
            if(cur >= A[i+1]) {
                addNum = cur - A[i+1] + 1;
                A[i+1] += addNum;
                count += addNum;
            }
            cur = A[i+1];
        }
        return count;
    }
};
```

* 时间复杂度：O(nlogn)
* 空间复杂度：O(logn)

<div align="center">
    <hr style="height:1px;"/>
    <br>
    <img width="200px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/wxgzh-hang.png"></img>
</div>

