## LeetCode每日一题：翻转字符串里的单词

给定一个字符串，逐个翻转字符串中的每个单词。

 
示例 1：

```
输入: "the sky is blue"
输出: "blue is sky the"
```

示例 2：

```
输入: "  hello world!  "
输出: "world! hello"
解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
```

示例 3：

```
输入: "a good   example"
输出: "example good a"
解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
```

说明：

* 无空格字符构成一个单词。
* 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
* 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
 

进阶：

请选用 C 语言的用户尝试使用 `O(1)` 额外空间复杂度的原地解法。



## Solve

巧了，这题我前几天刚在 `PAT` 上做过。

#### 1.辅助数组

正序遍历字符串，并利用辅助数组存储字符串中的每个单词，最后倒序遍历辅助数组，将单词添加至新的字符串。

```c++
class Solution {
public:
    string reverseWords(string s) {
        if(s.size() == 0) return "";
        vector<string> words;
        string sub = "";
        for(int i = 0; i < s.size(); i++) {
            if(s[i] != ' ') {
                sub += s[i];
                // 最后一个单词，进入数组
                if(i == s.size() - 1) 
                    words.push_back(sub);
            } else {
                if(sub.size() > 0) {
                    words.push_back(sub);
                    sub = "";
                }
            }
        }
        string res = "";
        int size = words.size();
        for(int i = size - 1; i >= 0; i--){
            res += words[i];
            if(i != 0) res += " ";
        }

        return res;
    }
};
```

* 时间复杂度：O(n)
* 空间复杂度：O(n)


#### 2.原地解决

倒序遍历字符串，在拼接字符时是单词的正序拼接方式。

每遇到空格并且拼接的字符串不为空时，则正序添加至最后的字符串结果中。

```c++
class Solution {
public:
    string reverseWords(string s) {
        if(s.size() == 0) return "";
        string res = "";
        string sub = "";
        for(int i = s.size() - 1; i >= 0; i--) {
            if(s[i] != ' ') {
                // 倒序遍历，这里位置不可改变
                sub = (s[i] + sub);
                if(i == 0) 
                    res += sub;
            } else { // 倒序，到达第一个单词的开头
                if(sub.size() > 0) {
                    res += (sub + " ");
                    sub = "";
                }
            }
        }
        if(res.size() > 0 && res[res.size() - 1] == ' ')
            // 将最后的空格删除
            res.pop_back();

        return res;
    }
}
```

* 时间复杂度：O(n)
* 空间复杂度：O(1)


<div align="center">
    <hr style="height:1px;"/>
    <br>
    <img width="200px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/wxgzh-hang.png"></img>
</div>

