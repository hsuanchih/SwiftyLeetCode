
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

![question_116.png](../images/question_116.png)
```
Input: root = [1,2,3,4,5,6,7]
Output: [1,#,2,3,#,4,5,6,7,#]
Explanation: Given the above perfect binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.
```

__Constraints:__
* The number of nodes in the tree is in the range `[0, pow(2, 12) - 1]`.
* `-1000 <= node.val <= 1000`

__Follow-up:__
* You may only use constant extra space.
* The recursive approach is fine. You may assume implicit stack space does not count as extra space for this problem.

### Solution
__O(n) Time, O((n + 1) / 2) Space - Iterative Level-Order Traversal:__
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
        guard let root else { return nil }
        var queue: [Node] = [root]
        while !queue.isEmpty {
            let count: Int = queue.count
            for i in 0 ..< count {
                let node: Node = queue.removeFirst()
                [node.left, node.right].forEach {
                    guard let next = $0 else { return }
                    queue.append(next)
                }
                if i < count - 1 {
                    node.next = queue.first
                }
            }
        }
        return root
    }
}
```
__O(n) Time, O(1) Space - Recursive Preorder Assignment:__
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
        guard let root else { return nil }
        root.left?.next = root.right
        root.right?.next = root.next?.left
        _ = connect(root.left)
        _ = connect(root.right)
        return root
    }
}
```
