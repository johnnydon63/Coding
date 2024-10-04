# 链表

<!-- GFM-TOC -->
* 很多链表问题可以用递归来处理
    * [1. 160 找出两个链表的交点 ](#1-找出两个链表的交点) Intersection of Two Linked Lists (Easy) [Leetcode](https://leetcode.com/problems/intersection-of-two-linked-lists/description/)
    * [2. 206 链表反转 ](#2-链表反转) Reverse Linked List (Easy) [Leetcode](https://leetcode.com/problems/reverse-linked-list/description/) 
    * [3. 21 归并两个有序的链表 ](#3-归并两个有序的链表) Merge Two Sorted Lists (Easy)  [Leetcode](https://leetcode.com/problems/merge-two-sorted-lists/description/) 
    * [4. 83 从有序链表中删除重复节点 ](#4-从有序链表中删除重复节点) Remove Duplicates from Sorted List (Easy) [Leetcode](https://leetcode.com/problems/remove-duplicates-from-sorted-list/description/)
    * [5. 19 删除链表的倒数第 n 个节点 ](#5-删除链表的倒数第-n-个节点) Remove Nth Node From End of List (Medium) [Leetcode](https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/)
    * [6. 24 交换链表中的相邻结点 ](#6-交换链表中的相邻结点) Swap Nodes in Pairs (Medium) [Leetcode](https://leetcode.com/problems/swap-nodes-in-pairs/description/)
    * [7. 445 链表求和 ](#7-链表求和) Add Two Numbers II (Medium)[Leetcode](https://leetcode.com/problems/add-two-numbers-ii/description/)
    * [8. 234 回文链表 ](#8-回文链表) Palindrome Linked List (Easy) [Leetcode](https://leetcode.com/problems/palindrome-linked-list/description/)
    * [9. 725 分隔链表 ](#9-分隔链表) Split Linked List in Parts(Medium) [Leetcode](https://leetcode.com/problems/split-linked-list-in-parts/description/) 
      ```html
      Input:
      root = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10], k = 3
      Output: [[1, 2, 3, 4], [5, 6, 7], [8, 9, 10]]
      Explanation:
      The input has been split into consecutive parts with size difference at most 1,
      and earlier parts are a larger size than the later parts.
      ```
    * [10. 328 链表元素按奇偶聚集 ](#10-链表元素按奇偶聚集) Odd Even Linked List (Medium) [Leetcode](https://leetcode.com/problems/odd-even-linked-list/description/)
      ```html
      Example:
      Given 1->2->3->4->5->NULL,
      return 1->3->5->2->4->NULL.
      ```
    * [11. 876 Middle_of_the_linked_list](#11-middle-of-the-linked-list) Middle of the linked list (Easy) [Leetcode](https://leetcode.com/problems/middle-of-the-linked-list/description/)
      
<!-- GFM-TOC -->


##  1. 找出两个链表的交点
```c
struct ListNode *getIntersectionNode(struct ListNode *headA, struct ListNode *headB) {
    struct ListNode *ha = headA, *hb = headB;
    while(ha != hb) {
        if(ha == NULL) {
            ha = headB;
        }
        else
            ha = ha->next;
        if(hb == NULL) {
            hb = headA;
        }
        else
            hb = hb->next;
        printf("%x vs %x\n", ha, hb);
    }
    return ha;
}
```

##  2. 链表反转
递归

```java
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }
    ListNode next = head.next;
    ListNode newHead = reverseList(next);
    next.next = head;
    head.next = null;
    return newHead;
}
```



```c
struct ListNode* reverseList(struct ListNode* head) {
    struct ListNode *pre = NULL, *cur = head, *nex = head;
    while(nex != NULL) {
        nex = cur->next;
        cur->next = pre;
        pre = cur;
        cur = nex;
    }
    return pre;
}
```

##  3. 归并两个有序的链表
```java
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    if (l1 == null) return l2;
    if (l2 == null) return l1;
    if (l1.val < l2.val) {
        l1.next = mergeTwoLists(l1.next, l2);
        return l1;
    } else {
        l2.next = mergeTwoLists(l1, l2.next);
        return l2;
    }
}
```
```c
struct ListNode* mergeTwoLists(struct ListNode* list1, struct ListNode* list2) {
    struct ListNode *head = NULL, *iter = NULL;
    head = malloc(sizeof(struct ListNode));
    iter = head;
    while(list1 && list2) {
        if(list1->val <= list2->val) {
            iter->next = list1;
            list1 = list1->next;
        }
        else {
            iter->next = list2;
            list2 = list2->next;
        }
        iter = iter->next;
    }
    if(list1 == NULL) 
        iter->next = list2;
    else
        iter->next = list1;
    iter = head->next;
    free(head);
    return iter;
}
```
##  4. 从有序链表中删除重复节点
```java
public ListNode deleteDuplicates(ListNode head) {
    if (head == null || head.next == null) return head;
    head.next = deleteDuplicates(head.next);
    return head.val == head.next.val ? head.next : head;
}
```
```c
struct ListNode* deleteDuplicates(struct ListNode* head) {
    struct ListNode* out = head, *dup = NULL;
    if(!head)
        return head;
    while(head != NULL && head->next != NULL) {
        if(head->val == head->next->val) {
            dup = head->next;
            head->next = head->next->next;
            free(dup);
        }
        else
            head = head->next;
    }
    return out;
}
```
##  5. 删除链表的倒数第 n 个节点
```c
struct ListNode* removeNthFromEnd(struct ListNode* head, int n) {
    struct ListNode pre;
    pre.next = head;
    struct ListNode *slow = &pre, *fast = &pre, *tmp = NULL;
    
    for(int i = 0; i <= n; i++) {
        if(fast == NULL)
            return NULL;
        fast = fast->next;
    }
    while(fast) {
        slow = slow->next;
        fast = fast->next;
    }
    tmp = slow->next;
    slow->next = tmp->next;
    free(tmp);
    return pre.next;
}
```

##  6. 交换链表中的相邻结点
```c
struct ListNode* swapPairs(struct ListNode* head) {
    struct ListNode dummy;
    dummy.next = head;
    struct ListNode *pre = &dummy, *nex = head, *n1 = NULL, *n2 = NULL;
    if(nex == NULL || nex->next == NULL)
        return head;
    else
        nex = nex->next->next;
    while(nex && nex->next) {
        n1 = pre->next;
        n2 = n1->next;
        pre->next = n2;
        n2->next = n1;
        n1->next = nex;
        pre = n1;
        nex = nex->next->next;
    }
    n1 = pre->next;
    n2 = n1->next;
    pre->next = n2;
    n2->next = n1;
    n1->next = nex;
    return dummy.next;
}
```

##  7. 链表求和
```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* reverse(struct ListNode* head) {
    struct ListNode *pre = NULL, *cur = head, *nex = head;
    while(cur) {
        nex = cur->next;
        cur->next = pre;
        pre = cur;
        cur = nex;
    }
    return pre;
}
struct ListNode* addTwoNumbers(struct ListNode* l1, struct ListNode* l2) {
    int carry = 0, val = 0, v1 = 0, v2 = 0;
    struct ListNode* pre_tail = NULL;
    l1 = reverse(l1);
    l2 = reverse(l2);
    while(l1 || l2 || carry == 1) {
        struct ListNode* n = malloc(sizeof(struct ListNode));
        v1 = l1 == NULL ? 0 : l1->val;
        v2 = l2 == NULL ? 0 : l2->val;
        if(l1){
            v1 = l1->val;
            l1 = l1->next;
        }
        else {
            v1 = 0;
        }
        if(l2){
            v2 = l2->val;
            l2 = l2->next;
        }
        else {
            v2 = 0;
        }
        n->val = (v1 + v2 + carry) % 10;
        carry = (v1 + v2 + carry) / 10;
        n->next = pre_tail;
        pre_tail = n;
    }
    return pre_tail;
}
```

##  8. 回文链表
```c
struct ListNode* reverse(struct ListNode* head) {
    struct ListNode *pre = NULL, *cur= head, *nex = head;
    while(cur) {
        nex = cur->next;
        cur->next = pre;
        pre = cur;
        cur = nex;
    }
    return pre;
 }
bool isPalindrome(struct ListNode* head) {
    struct ListNode *mid = head, *full = head, *mid_b = NULL;
    while(full && full->next) {
        mid = mid->next;
        full = full->next->next;
    }
    mid = reverse(mid);
    mid_b = mid;
    while(mid != NULL && head != mid_b) {
        if(head->val != mid->val)
            return false;
        head = head->next;
        mid = mid->next;
    }
    return true;
}
```

##  9. 分隔链表
```c
int ListLen(struct ListNode* head) {
    if(head == NULL) 
        return 0;
    return 1 + ListLen(head->next);
}
struct ListNode** splitListToParts(struct ListNode* head, int k, int* returnSize) {
    int len = ListLen(head);
    struct ListNode** out = malloc(sizeof(struct ListNode*) * k);
    struct ListNode* tmp = NULL;
    *returnSize = k;
    int m = len / k, r = len % k, ext = 0;
    for(int i = 0; i < k; i++) {
        out[i] = head;
        ext = i < r ? 1 : 0;
        for(int j = 1; j < m + ext; j++) {
            head = head->next;
        }
        if(head == NULL)
            continue;
        tmp = head->next;
        head->next = NULL;
        head = tmp;
    }
    return out;
}
```

##  10. 链表元素按奇偶聚集
```c
struct ListNode* oddEvenList(struct ListNode* head) {
    struct ListNode *odd = head, *even = head;
    if(head == NULL || head->next == NULL)
        return head;
    struct ListNode odd_h, even_h;
    odd = &odd_h;
    even = &even_h;
    for(int i = 1; head != NULL; i++) {
        if(i % 2 == 1) {
            odd->next = head;
            odd = odd->next;
        }
        else {
            even->next = head;
            even = even->next;
        }
        head = head->next;
    }
    odd->next = NULL;
    even->next = NULL;
    head = odd_h.next;
    while(head->next != NULL)
        head = head->next;
    head->next = even_h.next;
    return odd_h.next;
}
```

##  11. Middle of the linked list
```c
struct ListNode* middleNode(struct ListNode* head) {
    struct ListNode *slow = head, *fast = head;
    while(fast  && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
    }
    return slow;
}
```
