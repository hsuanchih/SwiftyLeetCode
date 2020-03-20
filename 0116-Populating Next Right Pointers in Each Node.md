
### Populating Next Right Pointers in Each Node

You are given a __perfect binary tree__ where all leaves are on the same level, and every parent has two children.</br> 
The binary tree has the following definition:
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
```
Input: root = [1,2,3,4,5,6,7]
Output: [1,#,2,3,#,4,5,6,7,#]
Explanation: Given the above perfect binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.
```

__Constraints:__
* The number of nodes in the given tree is less than `4096`.
* `-1000 <= node.val <= 1000`

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
        node.left?.next = node.right
        node.right?.next = node.next?.left
        _ = connect(node.left)
        _ = connect(node.right)
        return node
    }
}
```