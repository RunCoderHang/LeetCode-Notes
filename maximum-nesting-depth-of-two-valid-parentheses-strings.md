## LeetCode每日一题：有效括号的嵌套深度

**有效括号字符串** 定义：对于每个左括号，都能找到与之对应的右括号，反之亦然。详情参见题末「**有效括号字符串**」部分。

**嵌套深度** `depth` 定义：即有效括号字符串嵌套的层数，`depth(A)` 表示有效括号字符串 `A` 的嵌套深度。详情参见题末「**嵌套深度**」部分。

给你一个「有效括号字符串」 seq，请你将其分成两个不相交的有效括号字符串，`A` 和 `B`，并使这两个字符串的深度最小。

* 不相交：每个 `seq[i]` 只能分给 `A` 和 `B` 二者中的一个，不能既属于 `A` 也属于 `B` 。
* `A` 或 `B` 中的元素在原字符串中可以不连续。
* `A.length + B.length = seq.length`
* `max(depth(A), depth(B))` 的可能取值最小。

划分方案用一个长度为 seq.length 的答案数组 answer 表示，编码规则如下：

* `answer[i] = 0，seq[i]` 分给 `A` 。
* `answer[i] = 1，seq[i]` 分给 `B` 。

如果存在多个满足要求的答案，只需返回其中任意 一个 即可。


示例 1：

```
输入：seq = "(()())"
输出：[0,1,1,1,1,0]
```

示例 2：

```
输入：seq = "()(())()"
输出：[0,0,0,1,1,0,1,1]
```

提示：

* 1 <= text.size <= 10000
 

**有效括号字符串：**

>仅由 "(" 和 ")" 构成的字符串，对于每个左括号，都能找到与之对应的右括号，反之亦然。
>下述几种情况同样属于有效括号字符串：
>
>  1. 空字符串
>  2. 连接，可以记作 AB（A 与 B 连接），其中 A 和 B 都是有效括号字符串
>  3. 嵌套，可以记作 (A)，其中 A 是有效括号字符串

**嵌套深度：**

>类似地，我们可以定义任意有效括号字符串 s 的 嵌套深度 depth(S)：
>
>  1. s 为空时，depth("") = 0
>  2. s 为 A 与 B 连接时，depth(A + B) = max(depth(A), depth(B))，其中 A 和 B 都是有效括号字符串
>  3. s 为嵌套情况，depth("(" + A + ")") = 1 + depth(A)，其中 A 是有效括号字符串
>
>例如：""，"()()"，和 "()(()())" 都是有效括号字符串，嵌套深度分别为 0，1，2，而 ")(" 和 "(()" 都不是有效括号字符串。


## Solve

内心OS：这TM到底在说什么啊！

`LeetCode` 上的有些题目感觉就是在做阅读理解一样。其实题意就是：给定一个有效括号字符串，然后就根据左括号去找右括号。

当然，肯定会有括号嵌套括号的情况，这就要根据嵌套深度判断谁嵌套谁了。最后返回的数组中，每个元素对应着每个括号的嵌套深度。例如：

```
括号序列   ( ( ) ( ( ) ) ( ) )
下标编号   0 1 2 3 4 5 6 7 8 9
嵌套深度   1 2 2 2 3 3 2 2 2 1 
```

根据示例，`0` 代表奇数深度， `1` 代表偶数深度。但是在测试中发现：只要用 `0` `1` 区别出哪个是奇数，哪个是偶数即可。所以答案不唯一。

#### 1.栈

既然是括号匹配问题，就不得不提到栈了。因为根据先进后出的特性，用栈来作括号匹配很合适不过了。

```c++
class Solution {
public:
    vector<int> maxDepthAfterSplit(string seq) {
        stack<char> stk;
        vector<int> res(seq.size(), 0); // 记录括号对的深度。0代表奇数深度，1代表偶数深度
        for(int i = 0; i < seq.size(); i++) {
            char c = seq[i];
            if(c == '(') {
                res[i] = stk.size() % 2;
                stk.push(c);
            } else {
                stk.pop();
                res[i] = stk.size() % 2;
            }
        }
        return res;
    }
}
```

* 时间复杂度：**O(n)**
* 空间复杂度：**O(n)**

**优化：**

在一例我们发现，我其实需要的是栈的深度。所以，我们只需要用一个数字来表示栈的深度即可。

```c++
class Solution {
public:
    vector<int> maxDepthAfterSplit(string seq) {
        vector<int> res(seq.size(), 0);
        int depth = 0;
        for(int i = 0; i < seq.size(); i++) {
            if(seq[i] == '(') {
                res[i] = ++depth % 2; // 1代表奇数深度 0代表偶数深度。因为我们的初始深度为0，而栈的深度为1
            } else {
                res[i] = depth-- % 2;
            }
        }
        return res;
    }
}
```

* 时间复杂度：**O(n)**
* 空间复杂度：**O(1)**

<div align="center">
    <hr style="height:1px;"/>
    <br>
    <img width="200px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/wxgzh-hang.png"></img>
</div>
