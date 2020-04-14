## LeetCode每日一题：两数相加Ⅱ

给你两个 **非空** 链表来代表两个非负整数。数字最高位位于链表开始位置。它们的每个节点只存储一位数字。将这两数相加会返回一个新的链表。

你可以假设除了数字 0 之外，这两个数字都不会以零开头。


**进阶：**

如果输入链表不能修改该如何处理？换句话说，你不能对列表中的节点进行翻转。

**示例：**

```
输入：(7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 8 -> 0 -> 7
```


## Solve

#### 辅助栈

利用栈的后进先出，对数字的每一位进行相加计算。每相加一位，则出栈一位。相加之和添加至新的链表中（ **头插法** ）。

当然会出现位数相加后超过10的数字。我们只需要将大于10的数字拆分为个位与十位。个位添加至链表，而十位添加至下一位的和运算中。

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
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        if(l1 == NULL) return l2;
        if(l2 == NULL) return l1; 
        stack<int> s1;
        stack<int> s2;

        while(l1 != NULL) { s1.push(l1 -> val); l1 = l1 -> next; }
        while(l2 != NULL) { s2.push(l2 -> val); l2 = l2 -> next; }

        int mod = 0;
        ListNode *head = nullptr;
        while(!s1.empty() || !s2.empty() || mod != 0) {
            int sum = mod;
            if(!s1.empty()) { sum += s1.top(); s1.pop(); }
            if(!s2.empty()) { sum += s2.top(); s2.pop(); }
            // 链表头插法
            ListNode *newPtr = new ListNode(sum % 10);
            newPtr -> next = head;
            head = newPtr;
            
            mod = sum / 10;
        }
        return head;
    }
};
```

* 时间复杂度：O(max(m, n))，m 与 n 分别为两个链表的长度。
* 空间复杂度：O(m + n)


