## LeetCode : Single Number

Given a **non-empty** array of integers, every element appears twice except for one. Find that single one.

**Note:**

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

**Example 1:**

```
Input: [2,2,1]
Output: 1
```

**Example 2:**

```
Input: [4,1,2,1,2]
Output: 4
```


## Solve

#### 1.Hash Table

```c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        unordered_map<int, int> mp;
        for(int i = 0; i < nums.size(); i++) {
            mp[nums[i]]++;
        }
        for(auto el : mp) {
            if(el.second == 1)
                return el.first;
        }
        return -1;
    }
    
};
```

* Time complexity : O(n)
* Space complexity : O(n)


#### 2.**XOR** (异或)

```
a ⊕ 0 = a

a ⊕ a = 0

a ⊕ b ⊕ a = (a ⊕ a) ⊕ b = b
```

对于这个方法，有个评论很有意思：

>Seeing the solution 4,maybe is the difference between the intelligent and me.

```c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int a = 0;
        for(int i : nums) {
            a ^= i;
        }
        return a;
    }
};
```

* Time complexity : O(n)
* Space complexity : O(1)
