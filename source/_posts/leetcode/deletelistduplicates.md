# 删除排序链表中的重复元素

给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。

**示例 1:**

输入: 1->1->2
输出: 1->2
示例 2:

输入: 1->1->2->3->3
输出: 1->2->3

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
       ListNode temp = head;

        if (head == null) {
            return null;
        }

        while (temp.next != null) {
            if (temp.val == temp.next.val) {
                temp.next = temp.next.next;
                continue;
            }
            temp = temp.next;
        }

        return head;
    }
}

/**
执行用时 :1 ms, 在所有 Java 提交中击败了94.59%的用户
内存消耗 :39.3 MB, 在所有 Java 提交中击败了5.00%的用户
*/
```
Base Case: `head==null`  
Because this is a sorted linked list, we just need to figure out whether the next node's values equals current node's value.  

> Reference  
> https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list
