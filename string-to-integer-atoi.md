## LeetCode每日一题：字符串转换整数(atoi)

请你来实现一个 `atoi` 函数，使其能将字符串转换成整数。

首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。接下来的转化规则如下：

* 如果第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字字符组合起来，形成一个有符号整数。
* 假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成一个整数。
* 该字符串在有效的整数部分之后也可能会存在多余的字符，那么这些字符可以被忽略，它们对函数不应该造成影响。

注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换，即无法进行有效转换。

在任何情况下，若函数不能进行有效的转换时，请返回 0 。

提示：

* 本题中的空白字符只包括空格字符 ' ' 。
* 假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 `[−231,  231 − 1]`。如果数值超过这个范围，请返回  `INT_MAX (231 − 1)` 或 `INT_MIN (−231)` 。
 

示例 1:

```
输入: "42"
输出: 42
```

示例 2:

```
输入: "   -42"
输出: -42
解释: 第一个非空白字符为 '-', 它是一个负号。
     我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。
```

示例 3:

```
输入: "4193 with words"
输出: 4193
解释: 转换截止于数字 '3' ，因为它的下一个字符不为数字。
```

示例 4:

```
输入: "words and 987"
输出: 0
解释: 第一个非空字符是 'w', 但它不是数字或正、负号。
     因此无法执行有效的转换。
```

示例 5:

```
输入: "-91283472332"
输出: -2147483648
解释: 数字 "-91283472332" 超过 32 位有符号整数范围。 
     因此返回 INT_MIN (−231) 。
```


## Solve

看题解时，真是百家争鸣、各有千秋。比如：官方自动机、位运算、正则表达式、暴力造轮子、內库调用等等。

我一边看，一边跪在床上，同时泪水滑过脸庞，心里低吟一声：我好菜...

#### 1.遍历一遍

每个字符进行匹配。当遇到连续的数字时，依次 `*10` 并相加。

但每一次计算结果之前，都要确定是否超过边界。

```c++
class Solution {
public:
    int myAtoi(string str) {
        // 测试给定的字符串会很长，使用最大类型
        unsigned long size = str.size();
        int index = 0;

        // 去掉前面的空格
        while(index < size) {
            if(str[index] != ' ') {
                break;
            }
            index++;
        }

        // 空串情况
        if(index == size) return 0;
        
        // sign：正负标志
        int sign = 1;
        if(str[index] == '+') {
            index++;
        } else if(str[index] == '-') {
            sign = -1;
            index++;
        }

        int res = 0;
        for(; index < size; index++) {
            char ch = str[index];
            if(ch < '0' || ch > '9') break;
            // 提前判断乘以10以后是否越界
            if(res > INT_MAX / 10 || (res == INT_MAX / 10 && (ch - '0') > INT_MAX % 10))
                return INT_MAX;
            if(res < INT_MIN / 10 || (res == INT_MIN / 10 && (ch - '0') > -(INT_MIN % 10)))
                return INT_MIN;
            res = res * 10 + sign * (ch - '0'); // ASCII的字符转换数字
        }
        return res;
    }
};
```

* 时间复杂度：**O(n)**
* 空间复杂度：**O(1)**

#### 2.调用內库 `istringstream`

`istringstream` 是将字符串变成字符串迭代器一样，将字符串流在依次拿出，比较好的是，它不会将空格作为流。这样就实现了字符串的空格切割。

本题中输出的数字是不能用空格隔开。 最后使用使用整型来抽取值，所得结果也就是整型。

```c++
class Solution {
public:
    int myAtoi(string str) {
        int num = 0;
        istringstream stream(str);
        stream >> num;
        if (num > INT_MAX)
            return INT_MAX;
        else if (num < INT_MIN)
            return INT_MIN;
        else
            return num;
    }
};
```

#### 3.正则表达式

第一次写python，参考大佬的代码。

```python
class Solution:
    def myAtoi(self, str: str) -> int:
        import re
        matches=re.match('[ ]*([+-]?\d+)',str)
        if not matches:
            return 0
        res=int(matches.group(1))
        return min(max(res, -2**31), 2**31-1)
```

```java
import java.util.regex.*;

class Solution {
    final static Pattern pattern = Pattern.compile("[-+]??[0-9]++");
    public int myAtoi(String str) {
        String strTrim = str.trim();
        Matcher matcher = pattern.matcher(strTrim);
        if (matcher.lookingAt()) {
            String strNum = strTrim.substring(0, matcher.end());
            // 如果直接转32位int出现NFE那么就只要判断是Integer的最大值还是最小值就好了
            try {
                return Integer.parseInt(strNum);
            } catch (NumberFormatException nfe) {
                return strNum.charAt(0) == '-' ? Integer.MIN_VALUE : Integer.MAX_VALUE;
            }
        }
        return 0;
    }
}
```

<div align="center">
    <hr style="height:1px;"/>
    <br>
    <img width="200px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/wxgzh-hang.png"></img>
</div>
