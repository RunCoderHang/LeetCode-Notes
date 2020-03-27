## LeetCode每日一题（二十七）：卡牌分组

给定一副牌，每张牌上都写着一个整数。

此时，你需要选定一个数字 `X` ，使我们可以将整副牌按下述规则分成 `1` 组或更多组：

每组都有 `X` 张牌。
组内所有的牌上都写着相同的整数。
仅当你可选的 `X >= 2` 时返回 `true`。


示例 1：

```
输入：[1,2,3,4,4,3,2,1]
输出：true

解释：可行的分组是 [1,1]，[2,2]，[3,3]，[4,4]
```

示例 2：

```
输入：[1,1,1,2,2,2,3,3]
输出：false

解释：没有满足要求的分组。
```

示例 3：

```
输入：[1]
输出：false

解释：没有满足要求的分组。
```

示例 4：

```
输入：[1,1]
输出：true

解释：可行的分组是 [1,1]
```

示例 5：

```
输入：[1,1,2,2,2,2]
输出：true

解释：可行的分组是 [1,1]，[2,2]，[2,2]
```

提示：

* 1 <= deck.length <= 10000
* 0 <= deck[i] < 10000


## Solve

#### 1.暴力解法

计算元素出现的个数，再通过枚举可整除整除分组的数量，来确定该卡牌分组。

```c++
class Solution {
    int count[10000];
public:
    bool hasGroupsSizeX(vector<int>& deck) {
        vector<int> values;
        int size = deck.size();
        for(int el : deck) 
            count[el]++;
        for(int i = 0; i < 10000; i++) {
            if(count[i] > 0) values.push_back(count[i]);
        }
        for(int x = 2; x <= size; x++) {
            if(size % x == 0) {
                bool flag = 1;
                for(int el : values) {
                    if(el % x != 0) {
                        flag = 0;
                        break;
                    }
                }
                if(flag) return true;
            }
        }
        return false;
    }
};
```

* 时间复杂度：**O(n^2)**
* 空间复杂度：**O(n)**

#### 2.最大公约数

既然是通过整除来确定是否可分组，就可以使用最大公约数的方法，找出所有元素数量的最大公约数。

只要最大公约数是大于 `2` 的都可以分组。因为所有的数的公约数都是 `1` ，故不能使用 1。

```c++
bool hasGroupsSizeX(vector<int>& deck) {
        int counts[10000] = {0};
        int g = 0;
        for (int el : deck) {
            counts[el]++;
        }
        for (int x : counts){
            if(x == 0) continue;
            g = gcd(x, g);
            if(g == 1) return false;
        }
        return g >= 2;
    }
```

* 时间复杂度：**O(nlogn)**
* 空间复杂度：**O(n)**

<div align="center">
    <hr style="height:1px;"/>
    <br>
    <img width="200px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/wxgzh-hang.png"></img>
</div>

