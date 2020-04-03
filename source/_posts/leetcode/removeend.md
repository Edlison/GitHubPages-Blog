# Remove Nth Node From End of List

Given a linked list, remove the n-th node from the end of list and return its head.

**Example**:

Given linked list: 1->2->3->4->5, and n = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5.

**Note**:

Given n will always be valid.

**Solution One**
```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode p = head;
        ListNode temp = head;
        int len = 1;

        if (p == null)
            return null;

        while (p.next != null) {
            len++;
            p = p.next;
        }

        int loc = len - n;
        
        if (loc == 0) {
            return head.next;
        }

        for (int i = 1; i < loc; i++) {
            temp = temp.next;
        }

        temp.next = temp.next.next;

        return head;
    }
}
```
Pass one time and get the length of LinkedList, so just count `length - n - 1` times we can find the pre pointer. All we need to do is point pre pointer.next equal the next of which pointer we should delete.

**Solution Two**
```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        List<ListNode> stack = new LinkedList<>();
        ListNode temp = head;

        while (temp != null) {
            stack.add(temp);
            temp = temp.next;
        }

        ListNode[] list = stack.toArray(new ListNode[stack.size()]);

        if ((list.length - n - 1) < 0) return head.next;

        list[list.length - n - 1].next = list[list.length - n].next;

        return head;
    }
}
```
Just let the List structure give a help, we can transfer a `LinkedList` to an `Array`. So all we need to do is let `list[list.length - n - 1].next` equals `list[list.length - n].next`. 

Base case: if delete the head node we just need to return `head.next`.

> Reference  
> https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list
