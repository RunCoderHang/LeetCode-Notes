## LeetCode : Leftmost Column with at Least a One

*(This problem is an **interactive problem**.)*

A binary matrix means that all elements are `0` or `1`. For each **individual** row of the matrix, this row is sorted in non-decreasing order.

Given a row-sorted binary matrix binaryMatrix, return leftmost column index(0-indexed) with at least a `1` in it. If such index doesn't exist, return `-1`.

**You can't access the Binary Matrix directly.**  You may only access the matrix using a BinaryMatrix interface:

* `BinaryMatrix.get(x, y)` returns the element of the matrix at index `(x, y)` (0-indexed).
* `BinaryMatrix.dimensions()` returns a list of 2 elements `[n, m]`, which means the matrix is `n * m`.


Submissions making more than `1000` calls to `BinaryMatrix.get` will be judged Wrong Answer.  Also, any solutions that attempt to circumvent the judge will result in disqualification.

For custom testing purposes you're given the binary matrix `mat` as input in the following four examples. You will not have access the binary matrix directly.


**Example 1:**

<div align="left">
    <img width="81px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/untitled-diagram-3.jpg"></img>
</div>

```
Input: mat = [[0,0],[1,1]]
Output: 0
```

**Example 2:**

<div align="left">
    <img width="81px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/untitled-diagram-4.jpg"></img>
</div>

```
Input: mat = [[0,0],[0,1]]
Output: 1
```

**Example 3:**

<div align="left">
    <img width="81px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/untitled-diagram-5.jpg"></img>
</div>

```
Input: mat = [[0,0],[0,0]]
Output: -1
```

**Example 4:**

<div align="left">
    <img width="161px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/untitled-diagram-6.jpg"></img>
</div>

```
Input: mat = [[0,0,0,1],[0,0,1,1],[0,1,1,1]]
Output: 1
```

**Constraints:**

* `1 <= mat.length, mat[i].length <= 100`
* `mat[i][j]` is either `0` or `1`.
* `mat[i]` is sorted in a non-decreasing way.


## Solve

Code:

```c++
/**
 * // This is the BinaryMatrix's API interface.
 * // You should not implement it, or speculate about its implementation
 * class BinaryMatrix {
 *   public:
 *     int get(int x, int y);
 *     vector<int> dimensions();
 * };
 */

class Solution {
public:
    int leftMostColumnWithOne(BinaryMatrix &binaryMatrix) {
        vector<int> dim = binaryMatrix.dimensions();
        int rows = dim[0];
        int cols = dim[1];
        int res = -1;
        if(rows == 0 || cols == 0) return res;

        int r = 0, c = cols - 1;
        while(r < rows && c >= 0) {
            if(binaryMatrix.get(r, c) == 1) {
                res = c;
                c--;
            } else {
                r++;
            }
        }
        return res;
    }
};
```

