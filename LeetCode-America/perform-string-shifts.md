## LeetCode : Perform String Shifts


You are given a string `s` containing lowercase English letters, and a matrix `shift`, where `shift[i] = [direction, amount]`:

* `direction` can be 0 (for left shift) or `1` (for right shift). 
* `amount` is the amount by which string `s` is to be shifted.
* A left shift by 1 means remove the first character of `s` and append it to the end.
* Similarly, a right shift by 1 means remove the last character of `s` and add it to the beginning.


Return the final string after all operations.

 

**Example 1:**

```
Input: s = "abc", shift = [[0,1],[1,2]]
Output: "cab"
Explanation: 
[0,1] means shift to left by 1. "abc" -> "bca"
[1,2] means shift to right by 2. "bca" -> "cab"
```

**Example 2:**

```
Input: s = "abcdefg", shift = [[1,1],[1,1],[0,2],[1,3]]
Output: "efgabcd"
Explanation:  
[1,1] means shift to right by 1. "abcdefg" -> "gabcdef"
[1,1] means shift to right by 1. "gabcdef" -> "fgabcde"
[0,2] means shift to left by 2. "fgabcde" -> "abcdefg"
[1,3] means shift to right by 3. "abcdefg" -> "efgabcd"
```

Constraints:

* 1 <= s.length <= 100
* s only contains lower case English letters.
* 1 <= shift.length <= 100
* shift[i].length == 2
* 0 <= shift[i][0] <= 1
* 0 <= shift[i][1] <= 100

## Solve


计算总的偏移数量，如果总的偏移为负数，说明该字符串应 **向左偏移** 。相反如果为正数，则 **向右偏移** 。

根据总偏移数对字符串进行分割，将应发生偏移的字符串部分放置该偏移的位置。


**Code:**

```c++
class Solution {
public:
    string stringShift(string s, vector<vector<int>>& shift) {
        int totalShift = 0;
        int size = s.size();
        for(int i = 0; i < shift.size(); i++) {
            int direction = shift[i][0];
            int amount = shift[i][1];
            if(direction == 0)
                totalShift -= amount;
            else 
                totalShift += amount;
        }

        string newFront;
        string newBack;
        if(totalShift < 0) {
            // left shift
            totalShift = abs(totalShift) % size;
            newFront = s.substr(totalShift);
            newBack = s.substr(0, totalShift);
        } else {
            // right shift
            totalShift = totalShift % size;
            newFront = s.substr(size - totalShift, totalShift);
            newBack = s.substr(0, size - totalShift);
        }

        return newFront + newBack;
    }
};
```

* Time complexity : O(n)
* Space complexity : O(1)
