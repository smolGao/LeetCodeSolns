# Problem
*Difficulty*: Medium
Given the head of a linked list, remove the nth node from the end of the list and return its head.

**Example:**

![alt text](spiralmatrix.png)

# Logic
We'll be solving this problem by using two pointers.
To remove the nth element from the end of the list, the intuition tell us to read through whole list and rewind the node fater advance len - n - 1 times. However, we can do the same with slow and fast pointer.

First declare a dummy ListNode, and set slow and fast to it. Then, 

1. Move fast n node ahead.
2. Advance both slow and fast until fast == the end of list (null)
3. Rewind the slow pointer, which is just a node before the node to be removed.

## Complexities
Runtime: O(N)     
Space: O(1)   
N = length of the linkedlist

N is the number of nodes.

# Solutions
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode fast = dummy;
        ListNode slow = dummy;

        // Move the fast pointer n steps ahead
        for (int i = 0; i <= n; i++) {
            fast = fast.next;
        }

        // Move both pointers simultaneously until the fast pointer reaches the end
        while (fast != null) {
            fast = fast.next;
            slow = slow.next;
        }
        
        // Remove the nth node from the end
        slow.next = slow.next.next;
        
        return dummy.next; // Return the updated head
    }
}
```