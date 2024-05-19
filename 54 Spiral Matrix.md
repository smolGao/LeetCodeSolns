# Problem
Given an m x n matrix, return all elements of the matrix in spiral order.

**Example:**

![alt text](spiralmatrix.png)

# Logic
From the example, we notice a pattern: the element traverse goes from `top row -> right column -> bottom row -> left row`. That forms a cycle. 

Therefore, to solve that problem, we want to iterate the matrix in that pattern until the top collides with the bottom boundary and the left collides with the right boundary.

So, we initialize    

list = empty list   
top = 0, index of top boundary/row   
right = n, index of right boundary/column, where n is the length of a row   
bottom = m, index of bottom boundary/row, where m is the number of rows   
left = 0, index of left boundary/column

Then, in a while loop until top <= bottom and left <= right, we want to continues run through existing unvisited elements.

Inside the loop,
we have a loop to run through each segement of the pattern:
1. for `top` row, from `left to right`, add each elements to the list, increment `top`
2. for `right` column, from `top to bottom`, add each elements to the list, decrement `right` boundary.
3. for `bottom` row, from `right to left`, add each elements to the list, decrement `bottom`
4. for `left` column, from `bottom to top`, add each elements to the list, increment `left`

**Challenge for you:** There are edge cases where elements `outside of boundary` might get added to the list for inaccurate bottom row/left column traversal.   
Why is that?    
If you can think through it, take a pen and paper to trace it through. 


## Complexities
Runtime: O(N x M),     
Space: O(N x M)   
N = length of a row, M = total rows.

N is the number of nodes.

# Solutions
```
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> res = new ArrayList<Integer>();

        int top = 0;
        int left = 0;
        int bottom = matrix.length-1;;
        int right = matrix[0].length-1;

        while (top <= bottom && left <= right) {
            // Traverse top row
            for (int i = left; i <= right; i++) {
                res.add(matrix[top][i]);
            }
            top++;

            // Traverse right column
            for (int i = top; i <= bottom; i++) {
                res.add(matrix[i][right]);
            }
            right--;

            // Traverse bottom row
            if (top <= bottom) {
                for (int i = right; i >= left; i--) {
                    res.add(matrix[bottom][i]);
                }
                bottom--;
            }
            

            // Traverse left column
            if (left <= right)
            for (int i = bottom; i >= top; i--) {
                res.add(matrix[i][left]);
            }
            left++;
            
        }
        return res;        
    }
}
```