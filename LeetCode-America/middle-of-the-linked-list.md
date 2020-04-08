## LeetCode : Middle of the Linked List

Given a non-empty, singly linked list with head node `head`, return a middle node of linked list.

If there are two middle nodes, return the second middle node.


**Example 1:**

```
Input: [1,2,3,4,5]
Output: Node 3 from this list (Serialization: [3,4,5])
The returned node has value 3.  (The judge's serialization of this node is [3,4,5]).
Note that we returned a ListNode object ans, such that:
ans.val = 3, ans.next.val = 4, ans.next.next.val = 5, and ans.next.next.next = NULL.
```

**Example 2:**

```
Input: [1,2,3,4,5,6]
Output: Node 4 from this list (Serialization: [4,5,6])
Since the list has two middle nodes with values 3 and 4, we return the second one.
```

**Note:**

* The number of nodes in the given list will be between 1 and 100.

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

* Time Complexity: O(n)
* Space Complexity: O(1)

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

* Time Complexity: O(n)
* Space Complexity: O(n)

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

* Time Complexity: O(n)
* Space Complexity: O(1)


