# 合并两个有序链表

将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

**示例**：

输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4

```java
ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    if (l1 == null | l2 == null) {
        if (l1 == null & l2 == null) {
            return null;
        }
        if (l1 != null) {
            return l1;
        }
        if (l2 != null) {
            return l2;
        }
    }
    ListNode res = new ListNode(0);
    ListNode temp = res;
    while (l1 != null & l2 != null) {
        if (l1.val <= l2.val) {
            temp.val = l1.val;
            temp.next = new ListNode(0);
            temp = temp.next;
            l1 = l1.next;
        } else {
            temp.val = l2.val;
            temp.next = new ListNode(0);
            temp = temp.next;
            l2 = l2.next;
        }
    }

    if (l1 != null) {
        temp.val = l1.val;
        while (l1.next != null) {
            temp.next = new ListNode(l1.next.val);
            temp = temp.next;
            l1 = l1.next;
        }
    } else {
        temp.val = l2.val;
        while (l2.next != null) {
            temp.next = new ListNode(l2.next.val);
             temp = temp.next;
            l2 = l2.next;
        }
    }

    return res;

/* 
执行用时 :1 ms, 在所有 Java 提交中击败了83.58%的用户
内存消耗 :38.5 MB, 在所有 Java 提交中击败了51.34%的用户 
*/
}
```
The code is too complicated. I'm going on to simplify it.


Here is an updated code after I read the official document.
```java
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    ListNode res = new ListNode(0);// Head Pointer
    ListNode temp = res;
    while (l1 != null & l2 != null) {
        if (l1.val <= l2.val) {
            temp.next = l1;
            l1 = l1.next;
        } else {
            temp.next = l2;
            l2 = l2.next;
        }
        temp = temp.next;
    }
    temp.next = (l1 == null) ? l2 : l1;
    return res.next;

/* 执行用时 :1 ms, 在所有 Java 提交中击败了83.58%的用户
内存消耗 :38.2 MB, 在所有 Java 提交中击败了55.50%的用户 
*/
}
```
**This is a nice solution to have a head pointer, which means we haven't to modified current `temp.val` and register a new object on the `temp.next`. Also we needn't to care whether `l1` or `l2` is `null`**

> Reference  
> https://leetcode-cn.com/problems/merge-two-sorted-lists
