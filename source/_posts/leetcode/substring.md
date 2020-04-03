# Is Subsequence
Given a string s and a string t, check if s is subsequence of t.

You may assume that there is only lower case English letters in both s and t. t is potentially a very long (length ~= 500,000) string, and s is a short string (<=100).

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, "ace" is a subsequence of "abcde" while "aec" is not).

Example 1:  
s = "abc", t = "ahbgdc"

Return true.

Example 2:  
s = "axc", t = "ahbgdc"

Return false.

Follow up:  
If there are lots of incoming S, say S1, S2, ... , Sk where k >= 1B, and you want to check one by one to see if T has its subsequence. In this scenario, how would you change your code?

*Overtime*
```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        if (s.length() == 0) return true;
        
                for (int i = 0; i < t.length() - s.length(); i++) {
                    int res = 0;
                    int cur = i;
                    while (cur < t.length() && res < s.length()) {
                        if (t.charAt(cur) == s.charAt(res)) res++;
        
                        cur++;
        
                        if (res == s.length()) return true;
                    }
                }
        
                return false;
    }
}
```

```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        int res = -1;
        for (int i = 0; i < s.length(); i++) {
            res = t.indexOf(s.charAt(i), res + 1);
            if (res == -1) return false;
        }
        return true;
    }
}
/*
执行用时 :1 ms, 在所有 Java 提交中击败了90.09%的用户
内存消耗 :44.2 MB, 在所有 Java 提交中击败了100.00%的用户
*/
```

> Reference  
> https://leetcode-cn.com/problems/is-subsequence