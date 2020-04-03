# Binary Tree Inorder Traversal
Given a binary tree, return the inorder traversal of its nodes' values.

**Example:**

Input: [1,null,2,3]  
   1  
    \  
     2  
    /  
   3

Output: [1,3,2]  
Follow up: Recursive solution is trivial, could you do it iteratively?

*Old*
```c
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        stack<TreeNode*> stack;
        vector<int> vector;

        while (root != nullptr || !stack.empty()) {
            while (root != nullptr) {
                stack.push(root);
                root = root->left;
            }
            if (!stack.empty()) {
                root = stack.top();
                stack.pop();
                vector.push_back(root->val);
                root = root->right;
            }
        }
        
        return vector;
    }
}
```

> Reference  
> https://leetcode-cn.com/problems/binary-tree-inorder-traversal