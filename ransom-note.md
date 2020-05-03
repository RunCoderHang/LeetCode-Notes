## LeetCode : Ransom Note

Given an arbitrary ransom note string and another string containing letters from all the magazines, write a function that will return true if the ransom note can be constructed from the magazines ; otherwise, it will return false.

Each letter in the magazine string can only be used once in your ransom note.

**Note:**

You may assume that both strings contain only lowercase letters.

```
canConstruct("a", "b") -> false
canConstruct("aa", "ab") -> false
canConstruct("aa", "aab") -> true
```



## Solve

```c++
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        int hash[26] = {0};
        for(char ch : magazine)
            hash[ch - 'a']++;
        for(char ch : ransomNote) {
            hash[ch - 'a']--;
            if(hash[ch - 'a'] < 0) return false;
        }
        return true;
    }
};
```

* Time complexity: O(n + m), n:ransomNote.size(), m:magazine.size()
* Space complexity: O(m)


