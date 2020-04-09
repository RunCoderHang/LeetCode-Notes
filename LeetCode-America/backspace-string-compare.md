## LeetCode : Backspace String Compare

Given two strings `S` and `T`, return if they are equal when both are typed into empty text editors. `#` means a backspace character.

**Example 1:**

```
Input: S = "ab#c", T = "ad#c"
Output: true
Explanation: Both S and T become "ac".
```

**Example 2:**

```
Input: S = "ab##", T = "c#d#"
Output: true
Explanation: Both S and T become "".
```

**Example 3:**

```
Input: S = "a##c", T = "#a#c"
Output: true
Explanation: Both S and T become "c".
```

**Example 4:**

```
Input: S = "a#c", T = "b"
Output: false
Explanation: S becomes "c" while T becomes "b".
```

**Note:**

* 1 <= S.length <= 200
* 1 <= T.length <= 200
* S and T only contain lowercase letters and '#' characters.

**Follow up:**

Can you solve it in O(N) time and O(1) space?


## Solve

#### 1.栈 (Stack)


利用栈对字符串进行筛选，非 `#` 则入栈，而遇到 `#` 则出栈。栈中的字符串则为删除后的结果。

最后对两个结果进行对比即可。

```c++
class Solution {
public:
    string build(string s) {
        stack<char> _stack;
        for(int i = 0; i < s.size(); i++) {
            if(s[i] != '#') {
                _stack.push(s[i]);
            } else if(!_stack.empty()) {
                _stack.pop();
            }
        }
        string res;
        while(!_stack.empty()) {
            res += _stack.top();
            _stack.pop();
        }
        return res;
    }

    bool backspaceCompare(string S, string T) {
        return build(S) == build(T);
    }
};
```

* Time Complexity: O(M + N), where M, ,N are the lengths of S and T respectively.
* Space Complexity: O(M + N).



#### 2.双指针 (Two Pointer)

定义两个指针分别对两个字符串进行遍历。其中，定义一个变量用来对 `#` 进行计数。

每当指针指向 `#` 时，计数 `+1` ，此时指针仍然指向 `#` ，所以我们对计数进行 `-1` 的同时索引值也移动。这样，就可以跳过 `#` 字符，再对两字符串的字符进行对比是否相同。


```c++
class Solution {
public:
    bool backspaceCompare(string S, string T) {
        int i = S.size() - 1, j = T.size() - 1;
        int skipS = 0, skipT = 0;
        while(i >= 0 || j >= 0) {
            while(i >= 0) {
                if(S[i] == '#') {
                    skipS++; i--;
                } else if(skipS > 0) {
                    skipS--; i--;
                } else break;
            }
            while(j >= 0) {
                if(T[j] == '#') {
                    skipT++; j--;
                } else if(skipT > 0) {
                    skipT--; j--;
                } else break;
            }
            if(i >= 0 && j >= 0 && S[i] != T[j]) {
                return false;
            }

            if((i >= 0) != (j >= 0)) {
                return false;
            }
            i--;
            j--;
        }
        return true;
    }
}
```

* Time Complexity: O(M + N).
* Space Complexity: O(1).


