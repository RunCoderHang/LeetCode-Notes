## LeetCode : Counting Elements

Given an integer array `arr`, count element `x` such that `x + 1` is also in `arr`.

If there're duplicates in `arr`, count them seperately.


**Example 1:**

```
Input: arr = [1,2,3]
Output: 2
Explanation: 1 and 2 are counted cause 2 and 3 are in arr.
```

**Example 2:**

```
Input: arr = [1,1,3,3,5,5,7,7]
Output: 0
Explanation: No numbers are counted, cause there's no 2, 4, 6, or 8 in arr.
```

**Example 3:**

```
Input: arr = [1,3,2,3,5,0]
Output: 3
Explanation: 0, 1 and 2 are counted cause 1, 2 and 3 are in arr.
```

**Example 4:**

```
Input: arr = [1,1,2,2]
Output: 2
Explanation: Two 1s are counted cause 2 is in arr.
```

Constraints:

* 1 <= arr.length <= 1000
* 0 <= arr[i] <= 1000


## Solve

#### Hash Table

```c++
class Solution {
public:
    int countElements(vector<int>& arr) {
        unordered_map<int, int> _map;
        for(int a : arr) {
            _map[a]++;
        }
        int result = 0;
        for(auto n : _map) {
            // find:查找成功返回一个指向指定元素的迭代器，查找失败返回end迭代器。
            if(_map.find(n.first + 1) != _map.end()) {
                result += n.second;
            }
        }
        return result;
    }
};
```

* Time Complexity: O(n)
* Space Complexity: O(1)


