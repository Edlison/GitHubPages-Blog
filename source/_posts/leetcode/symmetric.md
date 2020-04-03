# Symmetric Tree

Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree [1,2,2,3,4,4,3] is symmetric:  
  
   1  
   / \  
  2   2  
 / \ / \  
3  4 4  3  
 

But the following [1,2,2,null,3,null,3] is not:

   1  
   / \  
  2   2  
   \   \  
   3    3  
 

Note:
Bonus points if you could solve it both recursively and iteratively.

```java
public class Symmetric {
    public static void main(String[] args) {

    }

    public boolean isSymmetric(TreeNode root) {

        if (root == null) return true;

        return checker(root.left, root.right);
    }

    public boolean checker(TreeNode left, TreeNode right) {
        if (left == null && right != null || left != null && right == null) return false;
        if (left == null && right == null) return true;

        if (left.val != right.val) return false;

        return checker(left.left, right.right) && checker(left.right, right.left);
    }
}

/*
执行用时 :0 ms, 在所有 Java 提交中击败了100.00%的用户
内存消耗 :37.4 MB, 在所有 Java 提交中击败了41.58%的用户
*/
```
The base case is that `if (root == null)`.  
First divide the problem into smallest piece that we judge the node's value equals or not.  
Then we conquer the problem that the node `left.left` equals `right.right` and `left.right` equals `right.left`.  

> Reference  
> https://leetcode-cn.com/problems/symmetric-tree/submissions