# leetcode No.141 hasCycle 环形链表

# 题目

链接：https://leetcode-cn.com/problems/linked-list-cycle

给定一个链表，判断链表中是否有环。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

**示例 1：**

输入：head = [3,2,0,-4], pos = 1

输出：true

解释：链表中有一个环，其尾部连接到第二个节点。

**示例 2：**

输入：head = [1,2], pos = 0

输出：true

解释：链表中有一个环，其尾部连接到第一个节点。

**示例 3：**

输入：head = [1], pos = -1

输出：false

解释：链表中没有环。


进阶：

你能用 O(1)（即，常量）内存解决此问题吗？


# C++代码

思路：使用快慢指针

慢指针一次移动一个节点

快指针一次移动两个节点

一旦快指针为NULL，则没有环，因为能够找到尽头

如果有环，那么整个循环过程是无穷尽的，快指针一定会追上慢指针，且不会错过。

```cpp
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
    bool hasCycle(ListNode *head) {
        if(head == NULL || head->next == NULL)
            return false;
        ListNode* slow = head;
        ListNode* fast = head->next;
        while(slow != fast){
            if(fast->next == NULL || fast->next->next == NULL)
                return false;
            fast = fast->next->next;
            slow = slow->next;
        }
        return true;
    }
};
```

执行用时: 16ms, 在所有 C++ 提交中击败了 54.04% 的用户

内存消耗: 9.8MB, 在所有 C++ 提交中击败了 33.30% 的用户

