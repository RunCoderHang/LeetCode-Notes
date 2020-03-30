## LeetCode每日一题（三十）：圆圈中最后剩下的数字

0,1,,n-1这n个数字排成一个圆圈，从数字0开始，每次从这个圆圈里删除第m个数字。求出这个圆圈里剩下的最后一个数字。

例如，0、1、2、3、4这5个数字组成一个圆圈，从数字0开始每次删除第3个数字，则删除的前4个数字依次是2、0、4、1，因此最后剩下的数字是3。


示例 1：

```
输入: n = 5, m = 3
输出: 3
```

示例 2：

```
输入: n = 10, m = 17
输出: 2
```

限制：

* 1 <= n <= 10^5
* 1 <= m <= 10^6

## Solve

#### 1.模拟

还记得当初学习队列时，使用逻辑将顺序队列臆造为一个循环队列。区分队空与队满时，则使用：

* 队空： `queue.front == queue.rear`
* 队满： `(queue + 1) % MaxSize == queue.front`

说到这里，大家大概明白我在说什么了。没错，就是使用逻辑中的循环。定义一个指向删除元素的索引值，使用上述方法，依次“循环”指向应删除的元素。

使用 `C++` 中链表，模拟为循环链表。但是，在 `LeetCode` 中会超时。**猜测：**虽然链表删除操作的时间复杂度为： `O(1)`，但是，将删除指针指向应删除的元素时，还是需要遍历寻找元素。最后的时间复杂度为： `O(n^2)`

```c++
/*
* 超时，不可用
*/
class Solution {
public:
    int lastRemaining(int n, int m) {
        list<int> li;
        for(int i = 0; i < n; i++) {
            li.push_back(i);
        }
        // 删除元素的位置
        int index = 0;
        while(li.size() > 1) {
            // 定义指向应删除元素的指针
            auto del = li.begin();
            index = (index + m - 1) % li.size();
            // 将指针指向应删除元素的位置 时间复杂度：O(n)
            advance(del, index);
            li.erase(del);
        }
        return li.front();
    }
};
```

`Java` 中使用 `LinkedList` 同样会超时，但是 `ArrayList` 不会超时。两者的区别：

* `LinkedList`：双向链表结构。删除时，O(1)；查询时，O(n)。
* `ArrayList`：数组形式存储。删除时，O(n)；查询时，O(1)。

怎么看，两者差别不大啊。但是，根据 [Sweetiee大佬](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/solution/javajie-jue-yue-se-fu-huan-wen-ti-gao-su-ni-wei-sh/)的说法，`ArrayList` 的删除操作在后续移位时，其实是内存连续空间的拷贝的！相比于 `LinkedList` 大量非连续性地址访问，其性能优于后者。


```java
/*
* 不超时，可用
*/
class Solution {
    public int lastRemaining(int n, int m) {
        ArrayList<Integer> list = new ArrayList<>();
        for(int i = 0; i < n; i++) {
            list.add(i);
        }
        // 删除元素的位置
        int index = 0;
        while(n > 1) {
            index = (index + m - 1) % n;
            // remove删除，每当删除一个元素后，后面元素的索引值会依次 -1
            list.remove(index);
            n--;
        }
        return list.get(0);
    }
}
```


#### 2.数学

智商被按在地上来回摩擦。

看了题解知道这是著名的约瑟夫问题。代码所表示的其实是一个反推的过程。

比如，`ans = 0` ，是指最后胜利者的位置是 `0` 号位置。根据公式反推每一轮淘汰的过程，而 `ans` 一直是胜利者在每一轮淘汰时所处的位置。

当反推到第一轮时，也就是所有人未淘汰时退出循环，我们返回最终的结果就是胜利者的位置。如图所示：  

<div align="center">
    <img width="530px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/Joseph-question.png"></img>
</div>

  
第七轮： `f(3 + 0) % 2 = 1`  
第六轮： `f(3 + 1) % 3 = 1`  
第五轮： `f(3 + 1) % 4 = 0`  
第四轮： `f(3 + 0) % 5 = 3`  
第三轮： `f(3 + 3) % 6 = 0`  
第二轮： `f(3 + 0) % 7 = 3`  
第一轮： `f(3 + 3) % 8 = 6`  

```c++
class Solution {
public:
    int lastRemaining(int n, int m) {
        int ans = 0;
        for(int i = 2; i != n+1; i++) {
            ans = (m + ans) % i;
        }
        return ans;
    }
};
```

* 时间复杂度：**O(n)**
* 空间复杂度：**O(1)**


<div align="center">
    <hr style="height:1px;"/>
    <br>
    <img width="200px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/wxgzh-hang.png"></img>
</div>
