# leetcode No.83 删除排序链表中的重复元素

# 题目

链接：https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list

给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。

**示例 1:**

输入: 1->1->2

输出: 1->2

**示例 2:**

输入: 1->1->2->3->3

输出: 1->2->3



# C++代码


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
    ListNode* deleteDuplicates(ListNode* head) {
        ListNode* p = head;
        while(p != NULL){
            ListNode* n = p->next;
            int dup_val = p->val;
            while(n != NULL && n->val == dup_val)
                n = n->next;
            p->next = n;
            p = n;
        }
        return head;
    }
};
```

执行用时: 12ms, 在所有 C++ 提交中击败了 74.74% 的用户

内存消耗: 13.2MB, 在所有 C++ 提交中击败了 5.06% 的用户


# Python3代码 

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        p = head
        while(p != None):
            n = p.next
            dup_val = p.val
            while(n != None and n.val == dup_val):
                n = n.next
            p.next = n
            p = n
        return head
```
