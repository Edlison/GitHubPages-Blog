# Merge k Sorted Lists

Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

**Example**:

Input:
[
  1->4->5,
  1->3->4,
  2->6
]
Output: 1->1->2->3->4->4->5->6

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        List<Integer> nodes = new ArrayList<>();
        
        for (int i = 0; i < lists.length; i++) {
            while (lists[i] != null) {
                nodes.add(lists[i].val);
                lists[i] = lists[i].next;
            }
        }

        if (nodes.size() == 0)
            return null;
        
        nodes.sort(Comparator.naturalOrder());
        ListNode res = new ListNode(nodes.remove(0));
        ListNode p = res;
        
        for (int i = 0; i < nodes.size(); i++) {
            p.next = new ListNode(nodes.get(i));
            p = p.next;
        }
        
        return res;
    }
}
```

> Reference  
> https://leetcode-cn.com/problems/merge-k-sorted-lists
