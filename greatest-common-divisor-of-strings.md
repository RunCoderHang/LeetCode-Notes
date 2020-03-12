## 一、LeetCode每日一题（十二）：字符串的最大公因子

对于字符串 `S` 和 `T` ，只有在 `S = T + ... + T`（T 与自身连接 1 次或多次）时，我们才认定 “T 能除尽 S”。

返回最长字符串 `X` ，要求满足 `X` 能除尽 `str1` 且 `X` 能除尽 `str2` 。

示例 1：

```
输入：str1 = "ABCABC", str2 = "ABC"
输出："ABC"
```

示例 2：

```
输入：str1 = "ABABAB", str2 = "ABAB"
输出："AB"
```

示例 3：

```
输入：str1 = "LEET", str2 = "CODE"
输出：""
```

提示：

1. 1 <= str1.length <= 1000
2. 1 <= str2.length <= 1000
3. str1[i] 和 str2[i] 为大写英文字母


## 二、代码

**枚举**

```c
#define min(a, b) (a < b ? a : b)
bool check(char* str1, char* str2) {
    int length1 = strlen(str1);
    int length2 = strlen(str2);
    int m = length1 / length2;
    char str3[length1 + 1];

    str3[0] = '\0';

    for (int i = 0; i < m; ++i) {
        strcat(str3, str2);
    }

    if (strcmp(str1, str3) == 0) {
        return true;
    }
    else {
        return false;
    }
}
char * gcdOfStrings(char * str1, char * str2){

    int len1 = strlen(str1);
    int len2 = strlen(str2);

    for (int i = min(len1, len2); i > 0; i--) {
        if (len1 % i == 0 && len2 % i == 0) {

            char temp[i + 1];
            for (int k = 0; k < i; ++k) {
                temp[k] = str1[k];
            }
            temp[i] = '\0';

            if (check(str1, temp) && check(str2, temp)) {
                char* s = (char*)malloc((i + 1) * sizeof(char));
                strcpy(s, temp);
                return s;
            }
        }
    }
    return "";
}
```

**使用数学方法**

```c
char * gcdOfStrings(char * str1, char * str2){

    int len1 = strlen(str1);
    int len2 = strlen(str2);

    char temp_s1[len1 + len2 + 1];
    char temp_s2[len1 + len2 + 1];

    strcpy(temp_s1, str1);
    strcat(temp_s1, str2);
    
    strcpy(temp_s2, str2);
    strcat(temp_s2, str1);

    if (strcmp(temp_s1, temp_s2) != 0) {
        return "";
    }

    int t;

    if (len1 < len2) {
        t = len1;
        len1 = len2;
        len2 = t;
    }

    while (len2 != 0) { // 辗转相除法求最大公约数
        t = len1 % len2;
        len1 = len2;
        len2 = t;
    }

    char* s = (char*)malloc((len1 + 1) * sizeof(char));
    for (int i = 0; i < len1; ++i) {
        s[i] = str1[i];
    }
    s[len1] = '\0';
    return s;    
}
```

```c++
class Solution {
public:
    string gcdOfStrings(string str1, string str2) {
        if (str1 + str2 != str2 + str1) return "";
        // __gcd() 为c++自带的求最大公约数的函数
        return str1.substr(0, __gcd((int)str1.length(), (int)str2.length()));
    }
};

// ——自LeetCode
```

<div align="center">
    <hr style="height:1px;"/>
    <br>
    <img width="200px" src="https://runcoderhang.github.io/thumbnails/wxgzh-hang.png"></img>
</div>