## LeetCode每日一题：统计重复个数


由 n 个连接的字符串 s 组成字符串 S，记作 `S = [s,n]` 。例如，`["abc",3]="abcabcabc"` 。

如果我们可以从 s2 中删除某些字符使其变为 s1，则称字符串 s1 可以从字符串 s2 获得。例如，根据定义，"abc" 可以从 “abdbec” 获得，但不能从 “acbbe” 获得。

现在给你两个非空字符串 s1 和 s2（每个最多 100 个字符长）和两个整数 0 ≤ n1 ≤ 106 和 1 ≤ n2 ≤ 106。现在考虑字符串 S1 和 S2，其中 `S1=[s1,n1]` 、`S2=[s2,n2]` 。

请你找出一个可以满足使 `[S2,M]` 从 `S1` 获得的最大整数 `M` 。


示例：

```
输入：
s1 ="acb",n1 = 4
s2 ="ab",n2 = 2

返回：
2
```



## Solve

▄█▀█● 今天又是一个 ctrl+c & ctrl+v 的一天。

参考：[https://www.cnblogs.com/grandyang/p/6149294.html](https://www.cnblogs.com/grandyang/p/6149294.html)

Code:

```c++
class Solution{
public:
    int getMaxRepetitions(string s1, int n1, string s2, int n2){
        vector<int> repeatCnt(n1 + 1, 0);
        vector<int> nextIdx(n1 + 1, 0);
        int j = 0, cnt = 0;
        //k表示段数，i表示s1的下标，j表示s2的下标
        for(int k = 1; k <= n1; k ++){
            for(int i = 0; i < s1.size(); i ++){
                if(s1[i] == s2[j]){
                    j ++;
                    if(j == s2.size()){
                        j = 0; cnt ++;
                    }
                }
            }
            repeatCnt[k] = cnt; //记录下前k段中s2的个数
            nextIdx[k] = j;
            for(int start = 0; start < k; start ++){
                //如果存在重复的nextIndex
                if(nextIdx[start] == j){
                    int interval = k - start;
                    int repeat = (n1 - start)/interval;
                    int patternCnt = (repeatCnt[k] - repeatCnt[start])*repeat;
                    int remainCnt = repeatCnt[start + (n1 - start) % interval];
                    return (patternCnt + remainCnt)/n2;
                }
            }
        }
        return repeatCnt[n1] / n2;
    }
};
```


暴力解法会超时。

```c++
class Solution {
public:
    int getMaxRepetitions(string s1, int n1, string s2, int n2) {
        string Rs1;
        for (int i = 0; i < n1; ++i)
        {
            Rs1 += s1;
        }
        string Rs2;
        for (int i = 0; i < n2; ++i)
        {
            Rs2 += s2;
        }

        int count = 0;
        int Len_Rs2 = n2 * s2.length();
        int j = 0;
        for (int i = 0; i < Len_Rs2; ++i)
        {
            for (; j < Rs1.length(); ++j)
            {
                if (Rs1[j] == Rs2[i])
                {
                    if (i == Len_Rs2 - 1)
                    {
                        count++;
                        i = -1;
                    }
                    j++;
                    break;
                }
            }
        }
        return count;
    }
};
```
