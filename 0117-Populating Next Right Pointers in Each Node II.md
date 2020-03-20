
### Populating Next Right Pointers in Each Node II

Given a binary tree
```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```
Populate each next pointer to point to its next right node.</br> 
If there is no next right node, the next pointer should be set to `NULL`.

Initially, all next pointers are set to `NULL`.

__Follow up:__
* You may only use constant extra space.
* Recursive approach is fine, you may assume implicit stack space does not count as extra space for this problem.

__Example 1:__

![example](images/question_117.png)
```
Input: root = [1,2,3,4,5,null,7]
Output: [1,#,2,3,#,4,5,7,#]
Explanation: Given the above binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.
```

__Constraints:__
* The number of nodes in the given tree is less than `6000`.
* `-100 <= node.val <= 100`

### Solution
__O(n) Time, O((n+1)/2) Space - Iterative Level-Order Traversal:__
```Swift
/**
 * Definition for a Node.
 * public class Node {
 *     public var val: Int
 *     public var left: Node?
 *     public var right: Node?
 *     public var next: Node?
 *     public init(_ val: Int) {
 *         self.val = val
 *         self.left = nil
 *         self.right = nil
 *         self.next = nil
 *     }
 * }
 */
class Solution {
    func connect(_ root: Node?) -> Node? {
        guard let root = root else { return nil }
        var queue : [Node] = [root]
        while !queue.isEmpty {
            let size = queue.count
            for i in 0..<size {
                let curr = queue.removeFirst()
                if let left = curr.left {
                    queue.append(left)
                }
                if let right = curr.right {
                    queue.append(right)
                }
                guard i < size-1 else { break }
                curr.next = queue.first
            }
        }
        return root
    }
}
```
__O(n) Time, O(1) Space - Recursive Pre-Order Assignment:__
```Swift
/**
 * Definition for a Node.
 * public class Node {
 *     public var val: Int
 *     public var left: Node?
 *     public var right: Node?
 *     public var next: Node?
 *     public init(_ val: Int) {
 *         self.val = val
 *         self.left = nil
 *         self.right = nil
 *         self.next = nil
 *     }
 * }
 */
class Solution {
    func connect(_ root: Node?) -> Node? {
        guard let node = root else { return root }
        var curr : Node?
        switch (node.left, node.right) {
            case (.none, .none):
            return node
            case let (.some(left), .some(right)):
            left.next = right
            curr = right
            default:
            curr = node.left ?? node.right
        }
        
        // Tree is not perfect, so there may be parents nodes with no children
        // We need to traverse the next link until a node with at least one child
        // is found, and assign the current node's child's next to that node's child
        var next = node.next
        while let more = next {
            switch (more.left, more.right) {
                case (.none, .none):
                next = more.next
                default:
                curr!.next = more.left ?? more.right
                next = nil
            }
        }

        // Reverse pre-order here, visit right before visit left as the right sub-tree
        // next links need to be in-place in order to correctly connect the next pointers
        // in the left sub-tree
        _ = connect(node.right)
        _ = connect(node.left)

        return node
    }
}
```
