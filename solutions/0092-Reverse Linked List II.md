
### Reverse Linked List II

Reverse a linked list from position *m* to *n*. Do it in one-pass.

__Note:__ 1 ≤ m ≤ n ≤ length of list.

__Example:__
```
Input: 1->2->3->4->5->NULL, m = 2, n = 4
Output: 1->4->3->2->5->NULL
```

### Solution
__Recursive O(n):__
```Swift
class Solution {
    func reverseBetween(_ head: ListNode?, _ m: Int, _ n: Int) -> ListNode? {
        // m & n in this problem uses 1-based indexing, 
        // thus m == 1 is our base case indicating that we've reached 
        // the intended node to start the reverse process
        if m == 1 {
            return reverse(head, n).0
        }
        // We assume that n >= m as valid input, in which case the # of nodes we want
        // to reverse is (n-m+1), thus the strategy is to decrement m & n together while
        // we move further into the recursion to identify the first node to reverse
        head?.next = reverseBetween(head?.next, m-1, n-1)
        return head
    }
    
    // Helper method to reverse n nodes starting at head
    // The return type returns: 
    // (head of reversed linked list after the current node, the node following the last node of the reversed linked list)
    func reverse(_ head: ListNode?, _ n: Int) -> (ListNode?, ListNode?) {
        // n == 1 is our base case, indicating that we've reached the last 
        // node to be reversed, return the (last node, node after the last node)
        if n == 1 {
            return (head, head?.next)
        }
        let reversed = reverse(head?.next, n-1)
        head?.next?.next = head
        head?.next = reversed.1
        return reversed
    }
}
```