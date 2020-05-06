## LeetCode : Majority Element

Given an array of size n, find the majority element. The majority element is the element that appears more than ⌊ n/2 ⌋ times.

You may assume that the array is non-empty and the majority element always exist in the array.

**Example 1:**


    Input: [3,2,3]
    Output: 3



**Example 2:**


    Input: [2,2,1,1,1,2,2]
    Output: 2



## Solve

#### 1. Hash Table

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {      
        int size = nums.size();
        unordered_map<int, int> hash;
        for(int i = 0; i < size; i++) {
            hash[nums[i]]++;
        }
        for(auto &x : hash) {
            cout << x.second << " ";
            if(x.second > size / 2)
                return x.first;
        }
        return 0;
    }
};
```

* Time complexity: O(n)
* Space complexity: O(n)


#### 2. sort

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        return nums[nums.size()/2];
    }
};
```

* Time complexity: O(nlogn)
* Space complexity: O(n) or O(1)


#### 3. Boyer-Moore Voting Algorithm


```java
class Solution {
    public int majorityElement(int[] nums) {
        int count = 0;
        Integer candidate = null;

        for (int num : nums) {
            if (count == 0) {
                candidate = num;
            }
            count += (num == candidate) ? 1 : -1;
        }

        return candidate;
    }
}
```

* Time complexity: O(nlogn)
* Space complexity: O(1)


