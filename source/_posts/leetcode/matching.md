# Wildcard Matching
Given an input string (s) and a pattern (p), implement wildcard pattern matching with support for '?' and '*'.

'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).
The matching should cover the entire input string (not partial).

Note:

s could be empty and contains only lowercase letters a-z.
p could be empty and contains only lowercase letters a-z, and characters like ? or *.
Example 1:

Input:
s = "aa"
p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
Example 2:

Input:
s = "aa"
p = "*"
Output: true
Explanation: '*' matches any sequence.
Example 3:

Input:
s = "cb"
p = "?a"
Output: false
Explanation: '?' matches 'c', but the second letter is 'a', which does not match 'b'.
Example 4:

Input:
s = "adceb"
p = "*a*b"
Output: true
Explanation: The first '*' matches the empty sequence, while the second '*' matches the substring "dce".
Example 5:

Input:
s = "acdcb"
p = "a*c?b"
Output: false

```java
class Solution {
  public boolean isMatch(String s, String p) {
    int sLen = s.length(), pLen = p.length();
    int sIdx = 0, pIdx = 0;
    int starIdx = -1, sTmpIdx = -1;

    while (sIdx < sLen) {

      if (pIdx < pLen && (p.charAt(pIdx) == '?' || p.charAt(pIdx) == s.charAt(sIdx))){
        ++sIdx;
        ++pIdx;
      }

      else if (pIdx < pLen && p.charAt(pIdx) == '*') {

        starIdx = pIdx;
        sTmpIdx = sIdx;
        ++pIdx;
      }

      else if (starIdx == -1) {
        return false;
      }

      else {

        pIdx = starIdx + 1;
        sIdx = sTmpIdx + 1;
        sTmpIdx = sIdx;
      }
    }

    for(int i = pIdx; i < pLen; i++)
      if (p.charAt(i) != '*') return false;
    return true;
  }
}
```

> Reference  
> https://leetcode-cn.com/problems/wildcard-matching/