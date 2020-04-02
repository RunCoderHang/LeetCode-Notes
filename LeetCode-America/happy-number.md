## LeetCode : Happy Number

Write an algorithm to determine if a number is "happy".

A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.

**Example:**

```
Input: 19
Output: true
Explanation: 
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
```


## Solve

列出非快乐数。

```
1^2 + 1^2 = 2
2^2 = 4
4^2 = 16
1^2 + 6^2 = 37
3^2 + 7^2 = 58
5^2 + 8^2 = 89
8^2 + 9^2 = 145
1^2 + 4^2 + 5^2 = 42
4^2 + 2^2 = 20
2^2 + 0^2 = 4

最后成了一个循环，所以我们寻找快乐数时不能进入此循环
4 → 16 → 37 → 58 → 89 → 145 → 42 → 20 → 4 
```



```c++
class Solution {
public:
    bool isHappy(int n) {
        
        while(n != 1 && n != 4) {
            int sum = 0;
            while(n != 0) {
                int m = n % 10;
                sum += pow(m, 2);
                n /= 10;
            }
            n = sum;
        }
        return n == 1;
    }
};
```



