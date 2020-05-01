## LeetCode每日一题：合并两个有序链表

将两个升序链表合并为一个新的升序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

示例：

```
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```

## Solve

#### 1. 迭代

新建一个链表，逐个比较添加。

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
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if(!l1) return l2;
        if(!l2) return l1;
        ListNode head, *p = &head;
        while(l1 != NULL && l2 != NULL) {
            ListNode *newPtr = nullptr;
            if(l1 -> val <= l2 -> val) {
                newPtr = new ListNode(l1 -> val);
                l1 = l1 -> next;
            } else {
                newPtr = new ListNode(l2 -> val);
                l2 = l2 -> next;
            }
            p -> next = newPtr;
            p = newPtr;
        }
        p -> next = l1 ? l1 : l2;

        return head.next;
    }
};
```

* 时间复杂度： O(n + m)
* 空间复杂度： O(1)

#### 2. 递归

妙啊！

```c++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if(!l1) return l2;
        if(!l2) return l1;
        if(l1 -> val <= l2 -> val){
            l1 -> next = mergeTwoLists(l1 -> next, l2);
            return l1;
        }else{
            l2 -> next = mergeTwoLists(l1, l2 -> next);
            return l2;
        }
    }
};
```

* 时间复杂度： O(n + m)
* 空间复杂度： O(n + m)


