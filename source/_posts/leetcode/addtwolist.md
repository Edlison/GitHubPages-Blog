# 两数相加

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

**示例：**

输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        
        ListNode head = new ListNode(0);
        ListNode p = head;
        ListNode pre = p;
        int temp;


        while (l1.next != null && l2.next != null) {
            temp = l1.val + l2.val;
            if (p.val + temp < 10) {
                p.val += temp;
                p.next = new ListNode(0);
                p = p.next; l1 = l1.next; l2 = l2.next;
            } else {
                p.val = (p.val + temp) % 10;
                p.next = new ListNode(1);
                p = p.next; l1 = l1.next; l2 = l2.next;
            }
        }

        temp = l1.val + l2.val;
        if (p.val + temp < 10) {
            p.val += temp;
            p.next = new ListNode(0);
            pre = p;
            p = p.next;
        } else {
            p.val = (p.val + temp) % 10;
            p.next = new ListNode(1);
            p = p.next;
        }

        while (l1.next != null) {
            l1 = l1.next;
            if (p.val + l1.val < 10) {
                p.val += l1.val;
                p.next = new ListNode(0);
                pre = p;
                p = p.next;
            } else {
                p.val = (p.val + l1.val) % 10;
                p.next = new ListNode(1);
                p = p.next;
            }
        }

        while (l2.next != null) {
            l2 = l2.next;
            if (p.val + l2.val < 10) {
                p.val += l2.val;
                p.next = new ListNode(0);
                pre = p;
                p = p.next;
            } else {
                p.val = (p.val + l2.val) % 10;
                p.next = new ListNode(1);
                p = p.next;
            }
        }

        if (p.val == 0) {
            pre.next = null;
        }

        return head;
    }
}

/**
执行用时 :3 ms, 在所有 Java 提交中击败了39.78%的用户
内存消耗 :41.1 MB, 在所有 Java 提交中击败了92.22%的用户
*/
```

> Reference  
> https://leetcode-cn.com/problems/add-two-numbers
