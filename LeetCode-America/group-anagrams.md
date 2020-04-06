## LeetCode : Goup Anagrams

Given an array of strings, group anagrams together.

**Example:**

```
Input: ["eat", "tea", "tan", "ate", "nat", "bat"],
Output:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```

**Note:**

* All inputs will be in lowercase.
* The order of your output does not matter.


## Solve

#### Hash Table

遍历数组中的每个字符串。将每个字符串以字母顺序进行排序，最后将排好序的字符串作为哈希表的 `key`，对应的 `value` 则是一个或多个的字母相同的单词。

```c++
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        vector<vector<string>> v;
        //  first  second
        map<string,vector<string>> m;
        for(int i=0;i<strs.size();i++)
        {
            string x=strs[i];
            sort(x.begin(),x.end());
            // map[first].push_back(second)
            m[x].push_back(strs[i]);
        }
        map<string,vector<string>> :: iterator it;
        for(it=m.begin();it!=m.end();it++)
            v.push_back(it->second);
        return v;
    }
};
```

* Time Complexity: O(NKlogK), where N is the length of strs, and K is the maximum length of a string in strs.
* Space Complexity: O(NK)

