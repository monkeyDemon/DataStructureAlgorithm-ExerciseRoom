# leetcode No.2 两数相加

# 题目

链接：https://leetcode-cn.com/problems/add-two-numbers

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

示例：

输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)

输出：7 -> 0 -> 8

原因：342 + 465 = 807


# C++代码

这个题按照题意实现即可

有一个潜在的小坑，就是当l1和l2都为null时，别忘了再判断一次是否需要进位。

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
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        int val = l1->val + l2->val;
        int las = val / 10;
        val = val % 10;
        ListNode* head = new ListNode(val);
        ListNode* p = head;
        while(l1->next != NULL && l2->next != NULL){
            val = l1->next->val + l2->next->val + las;
            las = val / 10;
            val = val % 10;
            p->next = new ListNode(val);
            p = p->next;
            l1 = l1->next;
            l2 = l2->next;
        }
        if(l1->next == NULL && l2->next == NULL){
            // pass
        }else if(l1->next == NULL){
            while(l2->next != NULL){
                val = l2->next->val + las;
                las = val / 10;
                val = val % 10;
                p->next = new ListNode(val);
                p = p->next;
                l2 = l2->next;
            }
        }else if(l2->next == NULL){
            while(l1->next != NULL){
                val = l1->next->val + las;
                las = val / 10;
                val = val % 10;
                p->next = new ListNode(val);
                p = p->next;
                l1 = l1->next;
            }
        }
        if(las == 1)
            p->next = new ListNode(1);
        return head;
    }
};
```

这道题想到了一种潜在的优化方案：前提时允许对传入的l1对应的链表元素进行覆盖

这时我们将返回结果存储到l1里，这样既可以节省空间占用，还可以省出new ListNode的时间

```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        // pst中存储表头
        ListNode  *pst;
        pst = l1;
        // 直接将答案存储在l1中，las处理进位
        l1 -> val += l2 -> val;
        int las = l1 -> val / 10;
        l1 -> val %= 10;
        // l1和l2均为非空时的情况处理
        while(l1 -> next != NULL && l2 -> next != NULL)
        {
            l1 = l1 -> next;
            l2 = l2 -> next;
            l1 -> val += l2 -> val + las;
            las = l1 -> val / 10;
            l1 -> val %= 10;
        }
        // l1空而l2非空时的情况处理
        while(l2 -> next != NULL)
        {
            l2 = l2 -> next;
            l1 -> next = new ListNode(l2 -> val + las);
            l1 = l1 -> next;
            las = l1 -> val / 10;
            l1 -> val %= 10;
        }
        // 可以同时处理l1非空而l2空，以及最后las中需要进位的情况处理
        while(las > 0)
        {
            if(l1 -> next == NULL) l1 -> next = new ListNode(las);
            else l1 -> next -> val += las;
            l1 = l1 -> next;
            las = l1 -> val / 10;
            l1 -> val %= 10;
        }
        return pst;
    }
};
```

