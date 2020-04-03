# Longest Common Prefix
Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string "".

**Example 1:**

Input: ["flower","flow","flight"]
Output: "fl"

**Example 2:**

Input: ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.

**Note:**

All given inputs are in lowercase letters a-z.

*Unsolved*
```
Input:
["aa","a"]
Output:
"aa"
Answer:
"a"
```
```java
class Solution {
    public static String longestCommonPrefix(String[] strs) {
        if (strs == null)
            return "";

        String res = strs[0];
        char[] resChar = res.toCharArray();

        for (int i = 0; i < strs.length; i++) {
            char[] tempChar = strs[i].toCharArray();
            for (int j = 0; j < tempChar.length & j < resChar.length; j++) {
                if (resChar[j] != tempChar[j]) {
                    res = strs[i].substring(0, j);
                    resChar = res.toCharArray();
                    break;
                }
            }
        }

        return res;
    }
}
```

**LeetCode Solution**
```java
public String longestCommonPrefix(String[] strs) {
   if (strs.length == 0) return "";
   String prefix = strs[0];
   for (int i = 1; i < strs.length; i++)
       while (strs[i].indexOf(prefix) != 0) {
           prefix = prefix.substring(0, prefix.length() - 1);
           if (prefix.isEmpty()) return "";
       }        
   return prefix;
}
```
Nice idea to use substring to find out what index is it. If `strs` doesn't have a substring the index can not be 0. All we need to do is cut the `prefix`'s length.

> Reference  
> https://leetcode-cn.com/problems/longest-common-prefix
