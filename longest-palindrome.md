## LeetCode每日一题（十九）：最长回文串

给定一个包含大写字母和小写字母的字符串，找到通过这些字母构造成的最长的回文串。

在构造过程中，请注意区分大小写。比如 `"Aa"` 不能当做一个回文字符串。

注意:
假设字符串的长度不会超过 1010。

示例 1:

```
输入:
"abccccdd"

输出:
7

解释:
我们可以构造的最长的回文串是"dccaccd", 它的长度是 7。
```

## Solve

#### 1.哈希表

题目的意思为：在给定的字符串中，找出可以组成回文的最长字符串。而回文字符串，也就是左右对称，其中心点不是奇数就是偶数。

例如： `aacbbcaa` 、 `aacbbbcaa` 等。然而，除了中心点，左右字符都是偶数对称的。

如果中心点就是偶数，相当于字符串中没有奇数个字符，可以组成回文字符串。

如果中心点是奇数，不管奇数为多少，中心始终只有一个。把所有偶数字符与奇数 `-1` 的字符找到，最后再 `+1` 中心点即可。


```c++
class Solution {
public:
    int longestPalindrome(string s) {
        unordered_map<char, int> hash_map;
        int maxLength = 0;
        int center = 0;
        for(auto x : s) {
            hash_map[x]++;
        }
        for(auto c : hash_map) {
            // c++ : second 指hash_map中的int，也就是value值
            if(c.second % 2 == 0) {
                maxLength += c.second;
            } else {
                maxLength += c.second - 1;
                center = 1;
            }
        }
        return maxLength + center;
    }
};
```

* 时间复杂度：O(n)
* 空间复杂度：O(n)

#### 2.ASCII

题目只是使用大小写字母，使用 `ASCII` 码中的字符值。

利用不同字符的十进制与第一个字符 `A` 的十进制作差，差值则对应着不同的字符以及数组的索引值，而数组则存储不同字符出现的次数。 

```c
int longestPalindrome(char * s){
    // 最大字符z：122  最小字符A：65
    int cnt[60] = {0};
    int len = strlen(s);
    int maxLength = 0, center = 0;
    for(int i = 0; i < len; i++)
        cnt[s[i] - 'A']++;
    for(int i = 0; i < 60; i++) {
        if(cnt[i] % 2 == 0) {
            maxLength += cnt[i];
        } else {
            maxLength += cnt[i] - 1;
            center = 1;
        }
    }
    return maxLength + center;
}
```

* 时间复杂度：O(n)
* 空间复杂度：O(s) s为字符集大小，即 `cnt[]` 的大小。


<div align="center">
    <hr style="height:1px;"/>
    <br>
    <img width="200px" src="https://runcoderhang.github.io/thumbnails/wxgzh-hang.png">
</div>