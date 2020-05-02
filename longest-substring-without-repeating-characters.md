## LeetCode每日一题：无重复字符的最长子串

给定一个字符串，请你找出其中不含有重复字符的 **最长子串** 的长度。


**示例 1:**

```
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```


**示例 2:**

```
输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```


**示例 3:**

```
输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```


## 分析

#### 哈希表 & 滑动窗口

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        if(s.size() == 0) return 0;
        unordered_map<char, int> hashTable;
        int maxLength = 0, curLength = 0;
        int start = 0;
        for(int i = 0; i < s.size(); i++) {
            // 哈希表中未找到该字符
            if(hashTable.find(s[i]) == hashTable.end()) {
                curLength++;
                hashTable[s[i]] = i;
            } else {
                maxLength = max(curLength, maxLength);
                start = max(hashTable[s[i]], start);
                curLength = i - start;
                hashTable[s[i]] = i;
            }
        }
        maxLength = max(curLength, maxLength);
        return maxLength;
    }
};
```

* 时间复杂度： O(n)
* 空间复杂度： O(|Σ|)，其中 Σ 表示字符集（即字符串中可以出现的字符），∣Σ∣ 表示字符集的大小。



