## LeetCode每日一题：跳跃游戏

给定一个非负整数数组，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个位置。

示例 1:

```
输入: [2,3,1,1,4]
输出: true
解释: 我们可以先跳 1 步，从位置 0 到达 位置 1, 然后再从位置 1 跳 3 步到达最后一个位置。
```

示例 2:

```
输入: [3,2,1,0,4]
输出: false
解释: 无论怎样，你总会到达索引为 3 的位置。但该位置的最大跳跃长度是 0 ， 所以你永远不可能到达最后一个位置。
```


## Solve

>想象你是那个在格子上行走的小人，格子里面的数字代表“能量”，你需要能量才能继续行走。
>
>每次走到一个格子的时候，检查现在格子里面的“能量”和你自己拥有的“能量”哪个更大，取更大的“能量”。如果你有更多的能量，你就可以走的更远啦！
>
>—— 摘自ylem的评论


```c++
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int size = nums.size();
        if(size == 0) return true;
        int cur = nums[0];
        int i = 1;
        while(cur != 0 && i < size) {
            cur--;
            if(cur < nums[i])
                cur = nums[i];
            i++;
        }
        return i == nums.size();
    }
};
```

* 时间复杂度：O(n)
* 空间复杂度：O(1)


然而我的代码...

```c++
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int size = nums.size();
        if(size == 0) return false;
        int cur = nums[0];
        if(cur == 0 && size == 1) return true;
        int i = 0;
        while(i < size) {
            if(cur == 0) {
                cur = nums[i];
                if(cur == 0 && i < size - 1) { // [1,0,1,0]
                    return false;
                } else if(i == size - 1) return true;
            } else {
                if(i == size - 1) return true;
                cur--;
                i++;
                if(cur < nums[i]) cur = nums[i];
            }
        }
        return false;
    }
};
```

* 时间复杂度：O(n)
* 空间复杂度：O(1)