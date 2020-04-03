# Valid Parentheses
Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Note that an empty string is also considered valid.

**Example 1:**

Input: "()"
Output: true

**Example 2:**

Input: "()[]{}"
Output: true

**Example 3:**

Input: "(]"
Output: false

**Example 4:**

Input: "([)]"
Output: false

**Example 5:**

Input: "{[]}"
Output: true

```java
class Solution {
    public boolean isValid(String s) {
        char[] charArray = s.toCharArray();
        Stack<Integer> stack = new Stack<>();
        if (s == null || s.length() == 0) {
            return true;
        }

        for (int i = 0; i < charArray.length; i++) {
            if (trans(charArray[i]) > 0) {
                stack.push(trans(charArray[i]));
            } else if (trans(charArray[i]) < 0 ) {
                if (!stack.isEmpty() && stack.pop() + trans(charArray[i]) == 0) {
                    continue;
                } else {
                    return false;
                }
            } else {
                return false;
            }
        }

        if (stack.size() == 0) {
            return true;
        } else {
            return false;
        }
    }

    private int trans(char c) {
        switch (c) {
            case '(': return 1;
            case ')': return -1;
            case '[': return 2;
            case ']': return -2;
            case '{': return 3;
            case '}': return -3;
            default: return 0;
        }
    }

/**
执行用时 :2 ms, 在所有 Java 提交中击败了91.77%的用户
内存消耗 :37.2 MB, 在所有 Java 提交中击败了5.05%的用户
*/
}
```
Source code is easy to see.

> Reference  
> https://leetcode-cn.com/problems/valid-parentheses