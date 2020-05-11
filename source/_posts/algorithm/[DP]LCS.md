# 最长公共子序列 (Longest Common Subsequence)

给定两个字符串 text1 和 text2，返回这两个字符串的最长公共子序列的长度。

一个字符串的 子序列 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。
例如，"ace" 是 "abcde" 的子序列，但 "aec" 不是 "abcde" 的子序列。两个字符串的「公共子序列」是这两个字符串所共同拥有的子序列。

若这两个字符串没有公共子序列，则返回 0。

示例:

输入：text1 = "abcde", text2 = "ace"   
输出：3    
解释：最长公共子序列是 "ace"，它的长度为 3。  

```java
public class DynamicProgramming {

	public static void main(String[] args) {
		String A = "xzyzzyx";
		String B = "zxyyzxz";
		String res = "";
		
		int[][] dp = new int[A.length() + 1][B.length() + 1];
		
		for (int i = 0; i <= A.length(); i++) {
			for (int j = 0; j <= B.length(); j++) {
				if (i == 0 || j == 0) {
					dp[i][j] = 0;
					continue;
				}
				
				if (A.charAt(i - 1) == B.charAt(j - 1)) {
					dp[i][j] = dp[i - 1][j - 1] + 1;
					continue;
				}
				
				if (A.charAt(i - 1) != B.charAt(j - 1)) {
					dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
					continue;
				}
			}
		}
		
		int row = A.length();
		int col = B.length();
		
		while (row > 0 && col > 0) {
			if (dp[row - 1][col - 1] < dp[row][col]) {
				res = res + A.charAt(row - 1);
				row--; col--;
			} else {
				row--;
			}
		}
		
		System.out.println("DP matrix is below: ");
		for (int i = 0; i <= A.length(); i++) {
			for (int j = 0; j <= B.length(); j++) {
				System.out.print(dp[i][j] + " ");
			}
			System.out.println();
		}
		
		System.out.print("One of th LCS is: ");
		for (int i = res.length() - 1; i >= 0; i--) {
			System.out.print(res.charAt(i));
		}
	}
}

/* result

DP matrix is below: 
0 0 0 0 0 0 0 0 
0 0 1 1 1 1 1 1 
0 1 1 1 1 2 2 2 
0 1 1 2 2 2 2 2 
0 1 1 2 2 3 3 3 
0 1 1 2 2 3 3 4 
0 1 1 2 3 3 3 4 
0 1 2 2 3 3 4 4 

One of th LCS is: xyzx

*/

```

典型的动态规划问题

首先声明一个以字符串A为行B为列的二维数组。
- 当`i=0`或`j=0`时`dp[i][j]=0`
- 当`ai=bi`时`dp[i][j]=dp[i-1][j-1]`(对应位置相等的话公共子串在`row-1`,`col-1`的情况下加1.)
- 当`ai!=bi`时`dp[i][j]=max{dp[i-1][j], dp[i][j-1]}`(不想等则取字符串A,B上一个相邻字符最长公共子串.)

最后显示一个最长子串时必须**从后向前**判断，顺序判断出的可能不是最长的子串。