# Problem
Given the root of a Binary Search Tree (BST), return the minimum absolute difference between the values of any two different nodes in the tree.

Reminder: While calculating the differences, ensure it's absolute differences. 

# Logic
From the problem, we know that the tree is a binary search tree, which means it will follow the criteria of a BST; left node < parent < right node. We want to find the minimum absolute difference, which means we want to find the difference between the two closest node values. Here, it will make sense to use in-order-traversal since it traverses consecutively through every pair of closest values.
Therefore, to compute the minimum difference, we can

1. subtract the current node's value from its previous node
2. compare the result to the minimum value we currently have.
3. Update it if needed

In the end, we have the correct min_diff.

## Complexities
Runtime: O(n)
Space: O(1)

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
    public int getMinimumDifference(TreeNode root) {
        // arr[0] = prev, arr[1] = abs min diff
        int arr[] = {Integer.MAX_VALUE, Integer.MAX_VALUE};
        traverse(arr, root);
        return arr[1];
    }

    public static void traverse(int arr[], TreeNode r) {
        if (r == null) return;

        traverse(arr, r.left);

        int diff = Math.abs(arr[0] - r.val);
        arr[1] = Math.min(arr[1], diff);
        arr[0] = r.val;

        traverse(arr, r.right);

    }

}
```

# Alternative(s)
## Bruteforce - Array element comparisons
1. Go through in-order-traversal and store all elements inside an array.
2. Declare an var called min_diff. Compute each element pairs through a loop and update min_diff accordingly.
3. Exit loop, min_diff will be the absolute minimum diff.

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
    public int getMinimumDifference(TreeNode root) {
        ArrayList<Integer> sorted_list = new ArrayList<Integer>();
        traverse(sorted_list, root);

        int min_diff = Integer.MAX_VALUE;
        for (int i = 0; i < sorted_list.size()-1; i++) {
            int diff = Math.abs(sorted_list.get(i) - sorted_list.get(i+1));
            min_diff = Math.min(min_diff, diff);
        }

        return min_diff;
    }

    public static void traverse(ArrayList<Integer> arr, TreeNode r) {
        if (r == null) return;

        traverse(arr, r.left);
        arr.add(r.val);
        traverse(arr, r.right);
    }
}
```