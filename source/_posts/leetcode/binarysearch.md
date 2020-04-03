# 不同的二叉搜索树 II
给定一个整数 n，生成所有由 1 ... n 为节点所组成的二叉搜索树。

示例:

输入: 3  
输出:  
[  
  [1,null,3,2],  
  [3,2,null,1],  
  [3,1,null,null,2],  
  [2,1,3],  
  [1,null,2,null,3]  
]  
解释:  
以上的输出对应以下 5 种不同结构的二叉搜索树：  

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
   
*Unsolved*
```java
class Solution {
    public List<TreeNode> generateTrees(int n) {

        if (n == 0) return new ArrayList<>();
        return helper(1, n);

    }

    private List<TreeNode> helper(int start, int end) {
        List<TreeNode> res = new ArrayList<>();
        if (start > end) {
            res.add(null);
            return res;
        }
        for (int val = start; val <= end; val++) {
            List<TreeNode> left = helper(start, val - 1);
            List<TreeNode> right = helper(val + 1, end);
            for (TreeNode l : left) {
                for (TreeNode r : right) {
                    TreeNode root = new TreeNode(val);
                    root.left = l;
                    root.right = r;
                    res.add(root);
                }
            }
        }
        return res;
    }
}
```

> Reference  
> https://leetcode-cn.com/problems/unique-binary-search-trees-ii