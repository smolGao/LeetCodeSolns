# Problem
Given the root of a binary search tree, and an integer k, return the kth smallest value (1-indexed) of all the values of the nodes in the tree.

This problem is similar to Problem 530 in term of traversal.

# Logic
Since in-order-traversal would traverse the BST in sorted order, we can utilize it to find the kth smallest elements.

Either loop or recursive versions would work. For the best runtime, I chose to traverse the tree in a loop with the stack.

We'll terminate the loop when we first encounter the k-th elements. 


## Complexities
Runtime: O(N)  
Space: O(H), where H is the height of the tree

# Solutions
```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        Stack<TreeNode> s = new Stack<TreeNode>(); 
        
        int count = 0;
        
        TreeNode curr = root;
 
        // Traverse the tree
        while (curr != null || s.size() > 0)
        {
 
            // Reach the leftmost TreeNode of the current TreeNode
            while (curr !=  null)
            {
                s.push(curr);
                curr = curr.left;
            }
 
            // Current must be null at this point            
            curr = s.pop();

            // Update count and check if kth index
            count++;
            if (count == k) return curr.val;
  
            // We have visited the node and its left subtree.
            // Now, it's right subtree's turn
            curr = curr.right;
        }


        return -1;
    }

}