
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

__Example 1:__

![question_117.png](../images/question_117.png)
```
Input: root = [1,2,3,4,5,null,7]
Output: [1,#,2,3,#,4,5,7,#]
Explanation: Given the above binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.
```
__Example 2:__
```
Input: root = []
Output: []
```

__Constraints:__
* The number of nodes in the tree is in the range `[0, 6000]`.
* `-100 <= node.val <= 100`

__Follow up:__
* You may only use constant extra space.
* Recursive approach is fine, you may assume implicit stack space does not count as extra space for this problem.

### Solution
__O(root) Time, O((root + 1) / 2) Space - Iterative Level-Order Traversal:__
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
                if i < count - 1 {
                    node.next = queue.first
                }
                [node.left, node.right].forEach {
                    guard let next = $0 else { return }
                    queue.append(next)
                }
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
        guard let root else { return nil }
        switch (root.left, root.right) {
        case (.none, .none):
            return root
        case (.some(let left), .none):
            populateNext(root, left)
        case (.none, .some(let right)):
            populateNext(root, right)
        case (.some(let left), .some(let right)):
            left.next = right
            populateNext(root, right)
        }

        // Reverse pre-order here, visit right before visit left as the right sub-tree
        // next links need to be in-place in order to correctly connect the next pointers
        // in the left sub-tree
        _ = connect(root.right)
        _ = connect(root.left)
        return root
    }

    func populateNext(_ root: Node, _ node: Node) {
        // Tree is not perfect, so there may be parents nodes with no children
        // We need to traverse the next link until a node with at least one child
        // is found, and assign the current node's child's next to that node's child
        var root: Node? = root.next
        while let curr = root, curr.left == nil, curr.right == nil {
            root = curr.next
        }
        node.next = root?.left ?? root?.right
    }
}
```
