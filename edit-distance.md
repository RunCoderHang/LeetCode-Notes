## LeetCode每日一题：编辑距离

给你两个单词 word1 和 word2，请你计算出将 word1 转换成 word2 所使用的最少操作数 。

你可以对一个单词进行如下三种操作：

1. 插入一个字符
2. 删除一个字符
3. 替换一个字符

示例 1：

```
输入：word1 = "horse", word2 = "ros"
输出：3

解释：
horse -> rorse (将 'h' 替换为 'r')
rorse -> rose (删除 'r')
rose -> ros (删除 'e')
```

示例 2：

```
输入：word1 = "intention", word2 = "execution"
输出：5
解释：
intention -> inention (删除 't')
inention -> enention (将 'i' 替换为 'e')
enention -> exention (将 'n' 替换为 'x')
exention -> exection (将 'n' 替换为 'c')
exection -> execution (插入 'u')
```


## Solve

#### 动态规划

`dp[i][j]` 代表 `word1` 到 `i` 位置转换成 `word2` 到 `j` 位置需要最少步数。

1. 替换，`dp[i][j] = dp[i - 1][j - 1] + 1`
2. 插入，`dp[i][j] = dp[i][j - 1] + 1`
3. 删除，`dp[i][j] = dp[i - 1][j] + 1`

<br>

| `DP[i][j]` |    |   |   |   |
|------------|----|---|---|---|
| word1word2 | '' | r | o | s |
| ''         | 0  | 1 | 2 | 3 |
| h          | 1  | 1 | 2 | 3 |
| o          | 2  | 2 | 1 | 2 |
| r          | 3  | 2 | 2 | 2 |
| s          | 4  | 3 | 3 | 2 |
| e          | 5  | 4 | 4 | 3 |
    

```c++
class Solution {
public:
    int minDistance(string word1, string word2) {
        // 将 word1 转换为 word2
        int n = word1.size();
        int m = word2.size();
        int dp[n+1][m+1];

        for(int i = 0; i <= n; i++) 
            dp[i][0] = i;
        for(int j = 0; j <= m; j++) 
            dp[0][j] = j;

        for(int i = 1; i <= n; i++) {
            for(int j = 1; j <= m; j++) {
                // 如果遇到字符相同，操作数不会增加。
                if(word1[i-1] == word2[j-1]) {
                    dp[i][j] = dp[i-1][j-1];
                    continue;
                }
                int replace = dp[i-1][j-1] + 1;
                int remove = dp[i-1][j] + 1;
                int insert = dp[i][j-1] + 1;
                dp[i][j] = min(replace, min(remove, insert));
            }
        }
        return dp[n][m];
    }
};
```

* 时间复杂度：**O(mn)**
* 空间复杂度：**O(mn)**


<div align="center">
    <hr style="height:1px;"/>
    <br>
    <img width="200px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/wxgzh-hang.png"></img>
</div>