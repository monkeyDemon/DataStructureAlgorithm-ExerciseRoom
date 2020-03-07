# leetcode 21. 合并两个有序链表

## 题目

链接：https://leetcode-cn.com/problems/merge-two-sorted-lists

将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

示例：

输入：1->2->4, 1->3->4

输出：1->1->2->3->4->4


## C++

### 解法1

一个最简单的思路，新建一个指针p

将p->next指向l1和l2所指元素中较小的那个，并将对应的指针l1或l2后移

重复上面的过程直至l1或l2为NULL，将剩余部分接到p上面即可

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
        if(l1 == NULL) return l2;
        if(l2 == NULL) return l1;
        ListNode* head = new ListNode(0);
        ListNode* pi = head;
        // l1,l2一直向后遍历元素，向head中按序插入，直至l1或l2为NULL
        while(l1 && l2){
            if(l1->val < l2->val){
                pi->next = l1;
                l1 = l1->next;
                pi = pi->next;
            }else{
                pi->next = l2;
                l2 = l2->next;
                pi = pi->next;
            }
        }
        // l1或l2为NULL,此时将不会空的链表接到最后即可
        pi->next = l1 ? l1 : l2;
        return head->next;
    }
};
```

执行用时: 12ms, 在所有 C++ 提交中击败了 41.17% 的用户

内存消耗: 16.8MB, 在所有 C++ 提交中击败了 5.07% 的用户

不知道大家是否能够看出上面的代码中的问题

这个问题说小不小，说大也蛮严重，我们生成的头节点不再被我们所维护，没有任何的指针指向它，也就是发生了内存泄漏。

因此我们将代码进行一些修改：

``` c++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if(l1 == NULL) return l2;
        if(l2 == NULL) return l1;
        ListNode* head;
        if (l1->val < l2->val){
            head = l1;
            l1 = l1->next;
        }else{
            head = l2;
            l2 = l2->next;
        }
        ListNode* pi = head;
        // l1,l2一直向后遍历元素，向head中按序插入，直至l1或l2为NULL
        while(l1 && l2){
            if(l1->val < l2->val){
                pi->next = l1;
                l1 = l1->next;
                pi = pi->next;
            }else{
                pi->next = l2;
                l2 = l2->next;
                pi = pi->next;
            }
        }
        // l1或l2为NULL,此时将不会空的链表接到最后即可
        pi->next = l1 ? l1 : l2;
        return head;
    }
};
```

### 解法2：递归

另一种很巧妙的方法是递归

我们让函数只处理好当前l1和l2节点的大小关系，后面的排序通过递归的调用自己求解一个子问题来完成。

``` c++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if (l1 == NULL) {
            return l2;
        }
        if (l2 == NULL) {
            return l1;
        }
        if (l1->val <= l2->val) {
            l1->next = mergeTwoLists(l1->next, l2);
            return l1;
        }
        l2->next = mergeTwoLists(l1, l2->next);
        return l2;
    }
};
```

两种方法耗时基本一致，第一种更直观，第二种方法更巧妙。

## Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        if l1 and l2:
            if l1.val > l2.val: l1, l2 = l2, l1
            l1.next = self.mergeTwoLists(l1.next, l2)
        return l1 or l2
```
