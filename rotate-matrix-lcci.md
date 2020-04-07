## LeetCode每日一题：旋转矩阵

给你一幅由 `N × N` 矩阵表示的图像，其中每个像素的大小为 4 字节。请你设计一种算法，将图像旋转 90 度。

不占用额外内存空间能否做到？


**示例 1:**

```
给定 matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

原地旋转输入矩阵，使其变为:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```

**示例 2:**

```
给定 matrix =
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
], 

原地旋转输入矩阵，使其变为:
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]
```



## Solve

#### 1.辅助数组

```c++
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int N = matrix.size();
        vector<vector<int>> matrix_new = matrix;
        for(int i = 0; i < N; i++) {
            for(int j = 0; j < N; j++) {
                swap(matrix[i][j], matrix_new[N-j-1][i])
            }
        }
    }
};
```

* 时间复杂度：**O(N^2)**
* 空间复杂度：**O(N^2)**


#### 2.原地交换

**2.1顺时针旋转**

数字从左上角开始，顺时针交换。

位置如有：`(i,j)`, `(j, N-i-1)`, `(N-i-1, N-j-1)`, `(N-j-1, i)`。

```
| 1 | 2 | 3 |     | 7 | 2 | 1 |     | 7 | 4 | 1 |
| 4 | 5 | 6 | --> | 4 | 5 | 6 | --> | 8 | 5 | 2 |
| 7 | 8 | 9 |     | 9 | 8 | 3 |     | 9 | 6 | 3 |
```


```c++
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int N = matrix.size();
        if(N == 0) return;
        for(int i = 0; i < N / 2; i++) {
            for(int j = 0; j < (N + 1) / 2; j++) {
                swap(matrix[i][j], matrix[j][N-i-1]);
                swap(matrix[i][j], matrix[N-i-1][N-j-1]);
                swap(matrix[i][j], matrix[N-j-1][i]);
            }
        }
    }
};
```

* 时间复杂度：**O(N^2)**
* 空间复杂度：**O(1)**

<br>

**2.2对称轴交换**

这个方法真的太巧妙了！

首先根据斜对角线的对称轴两边交换，最后，根据列的对称轴两边交换。

```
| 1 | 2 | 3 |     | 1 | 4 | 7 |     | 7 | 4 | 1 |
| 4 | 5 | 6 | --> | 2 | 5 | 8 | --> | 8 | 5 | 2 |
| 7 | 8 | 9 |     | 3 | 6 | 9 |     | 9 | 6 | 3 |
```

```c++
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int N = matrix.size();
        if(N == 0) return;
        for(int i = 0; i < N; i++) {
            for(int j = i; j < N; j++) {
                swap(matrix[i][j], matrix[j][i]);
            }
        }

        for(int i = 0; i < N; i++) {
            int left = 0;
            int right = N - 1;
            while(left < right) {
                swap(matrix[i][left], matrix[i][right]);
                left++;
                right--;
            }
        }
    }
};
```

* 时间复杂度：**O(N^2)**
* 空间复杂度：**O(1)**


