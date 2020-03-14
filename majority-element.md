## 一、LeetCode每日一题（十三）：多数元素

给定一个大小为 `n` 的数组，找到其中的多数元素。多数元素是指在数组中出现次数大于 `⌊n/2⌋` 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

示例 1:

```
输入: [3,2,3]
输出: 3
```

示例 2:

```
输入: [2,2,1,1,1,2,2]
输出: 2
```


## 二、Solve

#### 2.1 哈希表

使用哈希表来存储每个元素以及出现的次数。对于哈希映射中的每个键值对，键表示一个元素，值表示该元素出现的次数。

我们用一个循环遍历数组 `nums` 并将数组中的每个元素加入哈希映射中。在这之后，我们遍历哈希映射中的所有键值对，返回值最大的键。我们同样也可以在遍历数组 nums 时候使用打擂台的方法，维护最大的值，这样省去了最后对哈希映射的遍历。


```java
class Solution {
    private Map<Integer, Integer> countNums(int[] nums) {
        Map<Integer, Integer> counts = new HashMap<Integer, Integer>();
        for (int num : nums) {
            if (!counts.containsKey(num)) {
                counts.put(num, 1);
            }
            else {
                counts.put(num, counts.get(num)+1);
            }
        }
        return counts;
    }

    public int majorityElement(int[] nums) {
        Map<Integer, Integer> counts = countNums(nums);

        Map.Entry<Integer, Integer> majorityEntry = null;
        for (Map.Entry<Integer, Integer> entry : counts.entrySet()) {
            if (majorityEntry == null || entry.getValue() > majorityEntry.getValue()) {
                majorityEntry = entry;
            }
        }
        return majorityEntry.getKey();
    }
}
/*
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/majority-element
*/
```

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        unordered_map<int, int> counts;
        int majority = 0, cnt = 0;
        for (int num: nums) {
            ++counts[num];
            if (counts[num] > cnt) {
                majority = num;
                cnt = counts[num];
            }
        }
        return majority;
    }
};
/*
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/majority-element
*/
```


#### 2.2 排序

由于众数出现的频率大于 `n/2` ，所以在排序之后众数必存在于下标 `[n/2]` 处(本题默认数组中是一定存在众数的，所以返回下标 `[n/2]` 可行)

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        return nums[nums.size()/2];
    }
}
```


#### 2.3 摩尔投票法

如果我们把众数记为 `+1` ，把其他数记为 `−1` ，将它们全部加起来，显然和大于 0，从结果本身我们可以看出众数比其他数多。

根据题目知道，众数个数至少比非众数多一，把 count 加减当作一个众数抵消掉一个非众数，剩下的一定是众数

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int size = nums.size();
        int key = nums[0];
        int count = 1;
        for(int i = 1; i < size; i++) {
            if(count == 0) key = nums[i];
            if(key == nums[i]) {
                count++;
            } else {
                count--;
            }
        }
        return key;
    }
};
```


```c
int majorityElement(int* nums, int numsSize){
    int ans,count=0;
    for(int i=0;i<numsSize;i++) {
        if(count==0){
            ans=i;
            count++;
        } else {
            if(nums[ans]==nums[i])
                count++;
            else
                count--;
        } 
    }
    return nums[ans];
}
```

<div align="center">
    <hr style="height:1px;"/>
    <br>
    <img width="200px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/wxgzh-hang.png"></img>
</div>