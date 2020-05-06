## LeetCode : First Unique Character in a String

Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.

**Examples:**

```
s = "leetcode"
return 0.

s = "loveleetcode",
return 2.
```



## Solve

#### Hash Table

查询次数，不用多想，直接用哈希。

```c++
class Solution {
public:
    int firstUniqChar(string s) {
        unordered_map<char, int> hash;
        for(char ch : s) {
            hash[ch]++;
        }
        for(char ch : s) {
            if(hash[ch] == 1)
                return s.find_first_of(ch);
        }
        return -1;
    }
};
```

* Time complexity: O(n)
* Space complexity: O(n)


