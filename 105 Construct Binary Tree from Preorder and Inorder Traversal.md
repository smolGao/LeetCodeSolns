# Problem
Given two integer arrays preorder and inorder where preorder is the preorder traversal of a binary tree and inorder is the inorder traversal of the same tree, construct and return the binary tree.

# Logic
Let's go through the properties of traversals first:

**Preorder Traversal**: In this traversal, the nodes are visited in the order: root, left subtree, right subtree. This means the first element in the preorder array is always the root of the tree (or subtree).

**Inorder Traversal**: In this traversal, the nodes are visited in the order: left subtree, root, right subtree. This means the position of any element in the inorder array can help us identify the boundary between the left and right subtrees.

The trick here is that preorder array would give you the root of a tree while the inorder give you the information about subtrees.

Let's go through the steps:

1. **Identify the Root:** The first element of the preorder array is the root of the tree. Let's call this value `rootVal`.
2. **Locate the Root in Inorder Array:** Find the index of rootVal in the inorder array. This index splits the inorder array into left and right subtrees. Elements to the left of this index in inorder are part of the left subtree. Elements to the right of this index in inorder are part of the right subtree.
3. **Recursive Construction:** Recursively apply the same logic to construct the left and right subtrees.

Use the next element in preorder as the root for the left or right subtree. It's **important** to maintain the Index in a **reference-like style** as we need to use it to keep track of the current root element during recursive calls. Pass-by-value won't work because it doesn't reflect the correct root index (it ignores potentials trees under subtrees, which make the root element inaccurate). 

Use an index (or a reference-like structure) to keep track of the current root element in the preorder array as we move through it during recursive calls.


## Complexities
Runtime: O(N)  
Space: O(N)

N is the number of nodes.

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
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        int inorderLen = inorder.length;
        // To avoid iterating the array each time to spot root node element in preorder array
        // Create a hashmap to store indices of roots of each branch
        HashMap<Integer, Integer> inorderIndexMap = new HashMap<>();
        for (int i = 0; i < inorderLen; i++) {
            inorderIndexMap.put(inorder[i], i);
        }
        // An array to simulate pass-by-reference for the current index in the preorder array.
        // Start with the first element in preorder as the root.
        int rootIndex[] = {0};
        // Build the tree recursively
        return buildTree(preorder, inorder, inorderIndexMap, rootIndex, 0, inorderLen-1);
    }


    // This is a helper recursive function to build the tree
    // left and right represents the left/right subtree - partitions of inorder array - of the root node
    /** 
    * Helper recursive function to build the binary tree. 
    * 
    * @param preorder The preorder traversal array. 
    * @param inorder The inorder traversal array. 
    * @param inorderIndexMap HashMap storing the index of each value in inorder array. 
    * @param preorderIndex Array of size 1 to maintain current root index in preorder array. 
    * @param left The starting index of the current subtree in inorder array. 
    * @param right The ending index of the current subtree in inorder array. 
    * @return The root node of the binary tree (or subtree). 
    */
    
    public static TreeNode buildTree(int preorder[], int inorder[], HashMap<Integer, Integer> inorderIndexMap,
        int rootIndex[], int left, int right) {

        // Base case: If there are no elements to construct the subtree, return null.
        if (left > right) return null;

        // Select the current element in preorder array as the root value and increment the index.
        int rootVal = preorder[rootIndex[0]++];
        TreeNode root = new TreeNode(rootVal);

        // Build the left subtree with elements left of the root in inorder array.
        root.left = buildTree(preorder, inorder, inorderIndexMap, rootIndex, left, inorderIndexMap.get(rootVal) - 1);
        
        // Build the right subtree with elements right of the root in inorder array.
        root.right = buildTree(preorder, inorder, inorderIndexMap, rootIndex, inorderIndexMap.get(rootVal) + 1, right);

        return root;
    }
}
```