## LeetCode每日一题（十六）：字符串的压缩

字符串压缩。利用字符重复出现的次数，编写一种方法，实现基本的字符串压缩功能。比如，字符串aabcccccaaa会变为a2b1c5a3。若“压缩”后的字符串没有变短，则返回原先的字符串。你可以假设字符串中只包含大小写英文字母（a至z）。

示例1:

```
 输入："aabcccccaaa"
 输出："a2b1c5a3"
```

示例2:

```
 输入："abbccd"
 输出："abbccd"
 解释："abbccd"压缩后为"a1b2c2d1"，比原字符串长度更长。
```

提示：

* 字符串长度在[0, 50000]范围内。

## Solve

#### 1.双指针

```c++
class Solution {
public:
    string compressString(string S) {
        int N = S.length();
        string res;
        // 指向字符串头字符
        int i = 0;
        while (i < N) {
            // 负责寻找与 i 指向的相同的字符
            int j = i;
            while (j < N && S[j] == S[i]) {
                j++;
            }
            res += S[i];
            res += to_string(j - i);
            i = j;
        }

        if (res.length() < S.length()) {
            return res;
        } else {
            return S;
        }
    }
}
```

#### 2.模拟

```c++
class Solution {
public:
    string compressString(string S) {
        if ((int)S.length() == 0) return S; // 空串处理
        string ans = "";
        int cnt = 1;
        char ch = S[0];
        for (int i = 1; i < (int)S.length(); ++i){
            if (ch == S[i]) {
                cnt++;
            } else {
                ans += ch + to_string(cnt); // 注意 cnt 要转为字符串
                ch = S[i];
                cnt = 1;
            }
        }
        ans += ch + to_string(cnt);
        return ans.length() >= S.length() ? S : ans;
    }
};

/*
作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/compress-string-lcci/solution/zi-fu-chuan-ya-suo-by-leetcode-solution/
来源：力扣（LeetCode）
*/
```


<div align="center">
    <hr style="height:1px;"/>
    <br>
    <img width="200px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/wxgzh-hang.png"></img>
</div>