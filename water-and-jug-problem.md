## LeetCode每日一题（二十）：最小的k个数

有两个容量分别为 x 升 和 y 升 的水壶以及无限多的水。请判断能否通过使用这两个水壶，从而可以得到恰好 z 升 的水？

如果可以，最后请用以上水壶中的一或两个来盛放取得的 z 升 水。

你允许：

* 装满任意一个水壶
* 清空任意一个水壶
* 从一个水壶向另外一个水壶倒水，直到装满或者倒空

示例 1: (From the famous "Die Hard" example)

```
输入: x = 3, y = 5, z = 4
输出: True
```

示例 2:

```
输入: x = 2, y = 6, z = 5
输出: False
```

## Solve

老实说，一开始我并没有读懂这个题目。后来，我看到了MIT公开课中的 <龙胆虎威>，如果有人想看可以点击[链接](https://www.bilibili.com/video/av46506390?p=4)。

其实，我们把这个问题放到实际中去。例如：如何用 3升的水壶和 5升的水壶，在仅有这两个水壶和无限水的条件下获取 4升的水。

过程：假设 `x` ，`y` 分别代表 3升水壶中的水和5升水壶的水，`<x, y>`，则代表变化的状态。而刚开始，两个水壶都是没有水的。过程如下：

`<0, 0>` -> `<0, 5>` -> `<3, 2>` -> `<0, 2>` -> `<2, 0>` -> `<2, 5>` -> `<3, 4>` 

此时，5升的水壶就盛装有 4升的水。有人就会问：步骤这么多，而且看不出有什么规律，如何要用代码实现啊？

我想说的是：[裴蜀定理](https://baike.baidu.com/item/%E8%A3%B4%E8%9C%80%E5%AE%9A%E7%90%86)。若 `a`，`b` 是整数,且 `a`，`b` 的最大公约数为 `d`，那么对于任意的整数 `x`,`y`,`ax+by` 都一定是 `d` 的倍数，特别地，一定存在整数 `x`,`y` ，使 `ax+by=d` （裴蜀等式）成立。

根据裴蜀等式以及水壶的容量 `x` , `y` 可以找到一对整数 `a` ，`b` 使得： **`ax + by = z`** 且 `x + y <= z`

`z` 则是 `x`,`y` 的 **最大公约数的倍数** 。因此我们只需要找到 `x`,`y` 的最大公约数并判断 `z` 是否是它的倍数即可。

例如上例中 `x=3` 与 `y=5` 最大公约数为 1，符合 `z` 的还有：1 、 3 、 5 、 7。过程嘛，这里就不啰嗦啦。

```c++
class Solution {
public:
    bool canMeasureWater(int x, int y, int z) {
        if(x + y < z)   // 若两水壶容量相加比给定水的容量还小，肯定达不到目的
            return false;
        if(x == z || y == z || x + y == z) // 若有其中一壶或者总容量等于给定水的容量，直接达到目的
            return true;
        return z % gcd(x, y) == 0;  // 判断x y的最大公约数是否为z的倍数
    }
};
```

* 时间复杂度：O(log(min(x,y)))，取决于最大公约数使用的辗转相除法
* 空间复杂度：O(1)

<div align="center">
    <hr style="height:1px;"/>
    <br>
    <img width="200px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/wxgzh-hang.png"></img>
</div>