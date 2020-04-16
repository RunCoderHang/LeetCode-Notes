## LeetCode : Valid Parenthesis String

Given a string containing only three types of characters: '(', ')' and '*', write a function to check whether this string is valid. We define the validity of a string by these rules:

1. Any left parenthesis `'('` must have a corresponding right parenthesis `')'`.
2. Any right parenthesis `')'` must have a corresponding left parenthesis `'('`.
3. Left parenthesis `'('` must go before the corresponding right parenthesis `')'`.
4. `'*'` could be treated as a single right parenthesis `')'` or a single left parenthesis `'('` or an empty string.
5. An empty string is also valid.


**Example 1:**

```
Input: "()"
Output: True
```

**Example 2:**

```
Input: "(*)"
Output: True
```

**Example 3:**

```
Input: "(*))"
Output: True
```

**Note:**

* The string size will be in the range `[1, 100]`.


## Solve

#### 双指针

```c++
class Solution {
public:
    bool checkValidString(string s) {
        int size = s.size() - 1;
        int left = 0, right = 0;
        for (int i = 0; i <= size; i++)
        {
            if (s[i] == '*' || s[i] == '(') left++;
            else left--;
            if (s[size - i] == '*' || s[size - i] == ')') right++;
            else right--;
            if (left < 0 || right < 0) return false;
        }
        return true;
    }
};
```

* Time complexity : O(n)
* Space complexity : O(1)

#### 两个栈

```c++
class Solution {
public:
    bool checkValidString(string s) {
        stack<int> left, star;
        for(int i = 0; i < s.size(); i++) {
            if(s[i] == ')') {
                if(!left.empty()) left.pop();
                else if(!star.empty()) star.pop();
                else return false;
            } else if(s[i] == '(') {
                left.push(i);
            } else {
                star.push(i);
            }
        }

        while(!left.empty() && !star.empty()) {
            if(left.top() > star.top()) return false;
            left.pop();
            star.pop();
        }

        return left.empty();
    }
};
```

* Time complexity : O(n)
* Space complexity : O(n)


