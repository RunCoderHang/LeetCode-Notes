## LeetCode每日一题（二十三）：链表的中间结点

给定一个带有头结点 `head` 的非空单链表，返回链表的中间结点。

如果有两个中间结点，则返回第二个中间结点。 

示例 1：

```
输入：[1,2,3,4,5]
输出：此列表中的结点 3 (序列化形式：[3,4,5])

返回的结点值为 3 。 (测评系统对该结点序列化表述是 [3,4,5])。

注意，我们返回了一个 ListNode 类型的对象 ans，这样：
ans.val = 3, ans.next.val = 4, ans.next.next.val = 5, 以及 ans.next.next.next = NULL.
```

示例 2：

```
输入：[1,2,3,4,5,6]
输出：此列表中的结点 4 (序列化形式：[4,5,6])

由于该列表有两个中间结点，值分别为 3 和 4，我们返回第二个结点。
```

提示：

* 给定链表的结点数介于 1 和 100 之间。

## Solve

#### 1.快慢指针

慢指针一次走一步，而快指针一次走两步。如图所示：

<div align="center">
    <img width="457px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/slow-and-fast.gif"></img>
</div>

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        ListNode *slow = head;
        ListNode *fast = head;
        while(fast != NULL && fast -> next != NULL) {
            slow = slow -> next;
            fast = fast -> next -> next;
        }
        return slow;
    }
};
```

* 时间复杂度：O(n)
* 空间复杂度：O(1)

#### 2.使用数组

利用空间换时间。数组查询时间为 `O(1)` ，可快速寻找。

```c++
class Solution {
public:
    // 数组换时间
    ListNode* middleNode(ListNode* head) {
        vector<ListNode*> A;
        ListNode *p = head;
        while (p != NULL) {
            A.push_back(p);
            p = p -> next;
        }
        return A[A.size() / 2];
    }
};
```

* 时间复杂度：O(n)
* 空间复杂度：O(n)

#### 3.遍历一遍

将链表遍历一遍得到 `size` ，再根据长度找到中间元素。

```c++
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        ListNode *p = head;
        int size = 0;
        int count = 0;
        while(p != NULL) {
            p = p -> next;
            size++;
        }
        p = head;
        while(count < size / 2) {
            p = p -> next;
            count++;
        }
        return p;
    }
};
```

* 时间复杂度：O(n)
* 空间复杂度：O(1)


<div align="center">
    <hr style="height:1px;"/>
    <br>
    <img width="200px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/wxgzh-hang.png"></img>
</div>
