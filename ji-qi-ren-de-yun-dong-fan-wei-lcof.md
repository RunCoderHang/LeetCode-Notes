## LeetCode每日一题：机器人的运动范围

地上有一个m行n列的方格，从坐标 `[0,0]` 到坐标 `[m-1,n-1]` 。一个机器人从坐标 `[0, 0]` 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

 

示例 1：

```
输入：m = 2, n = 3, k = 1
输出：3
```

示例 2：

```
输入：m = 3, n = 1, k = 0
输出：1
```

提示：

* 1 <= n,m <= 100
* 0 <= k <= 20


## Solve

#### BFS

```c++
class Solution {
public:
    int get_sum( pair<int, int> P ) {
        int res = 0;
        while ( P.first ) {
            res += P.first % 10;
            P.first /= 10;
        }
        while ( P.second ) {
            res += P.second % 10;
            P.second /= 10;
        }

        return res;
    }
    
    int movingCount(int m, int n, int k) {
        if ( m == 0 || n == 0 )
            return 0;
        
        queue<pair<int, int>> Q;
        vector<vector<bool>> visited( m, vector<bool>( n, false ) );
        // 只需判断右、下两个方向即可
        int dx[2] = { 0, 1 };
        int dy[2] = { 1, 0 };
        int count = 0;

        Q.push( { 0, 0 } );
        while ( !Q.empty() ) {
            auto temp = Q.front();
            Q.pop();
            
            if ( visited[temp.first][temp.second] || get_sum( temp ) > k )
                continue;
            
            ++count;
            visited[temp.first][temp.second] = true;
            for ( int i = 0; i < 2; ++i ) {
                int x = temp.first + dx[i];
                int y = temp.second + dy[i];
                
                if ( x >= 0 && x < m && y >= 0 && y < n )
                    Q.push( { x, y } );
            }
        }
        
        return count;       
    }
};
```

* 时间复杂度：O(mn)，其中 m 为方格的行数，n 为方格的列数。考虑所有格子都能进入，那么搜索的时候一个格子最多会被访问的次数为常数。
* 空间复杂度：O(mn)，其中 m 为方格的行数，n 为方格的列数。搜索的时候需要一个大小为 O(mn) 的标记结构用来标记每个格子是否被走过。
