# Longest Substring Without Repeating Characters

Given a string, find the length of the longest substring without repeating characters.

**Example 1:**

Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3. 

**Example 2:**

Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.

**Example 3:**

Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3. 
             Note that the answer must be a substring, "pwke" is a subsequence and not a substring.

**Solution 1**
```java
    private static int lengthOfLongestSubstring(String s) {
        Set<Character> substring = new HashSet<>();
        int res = 0;

        if (s == null) {
            return res;
        }

        for (int i = 0; i < s.length(); i++) {
            int temp = 0;
            int j = i;
            while (j < s.length()) {
                if (!substring.contains(s.charAt(j))) {
                    substring.add(s.charAt(j++));
                    temp++;
                } else {
                    substring.clear();
                    break;
                }
            }
            res = Math.max(temp, res);
        }

        return res;

        /**
        执行用时 :127 ms, 在所有 Java 提交中击败了10.50%的用户
        内存消耗 :42.3 MB, 在所有 Java 提交中击败了5.02%的用户
        */
    }
```
Using too much time, so I read the leetcode document.  
Here is a *SlideWindow* method.

**Solution 2**
```java
private static int plus(String s) {
        int res = 0;
        Set<Character> set = new HashSet<>();
        int i = 0, j = i; 
        
        while (j < s.length()) {
            if (!set.contains(s.charAt(j))) {
                set.add(s.charAt(j++));
                res = Math.max(res, set.size());
            } else {
                set.remove(s.charAt(i++));
            }
        }

        return res;

        /**
        执行用时 :10 ms, 在所有 Java 提交中击败了64.95%的用户
        内存消耗 :41.6 MB, 在所有 Java 提交中击败了5.02%的用户
        */
```
We can adjust the *SlideWindow* by increase `i` - head pointer and `j` - rear pointer.  
This can sharply optimize the complexity of time.

> Reference  
> https://leetcode-cn.com/problems/longest-substring-without-repeating-characters