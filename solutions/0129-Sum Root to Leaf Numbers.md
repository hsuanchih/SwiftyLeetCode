
### Sum Root to Leaf Numbers

You are given the `root` of a binary tree containing digits from `0` to `9` only.

Each root-to-leaf path in the tree represents a number.
* For example, the root-to-leaf path `1 -> 2 -> 3` represents the number `123`.

Return the total sum of all root-to-leaf numbers. Test cases are generated so that the answer will fit in a 32-bit integer.

A __leaf__ node is a node with no children.

__Example 1:__

![question_129-0.jpg](../images/question_129-0.jpg)
```
Input: root = [1,2,3]
Output: 25
Explanation:
The root-to-leaf path 1->2 represents the number 12.
The root-to-leaf path 1->3 represents the number 13.
Therefore, sum = 12 + 13 = 25.
```
__Example 2:__

![question_129-1.jpg](../images/question_129-1.jpg)
```
Input: root = [4,9,0,5,1]
Output: 1026
Explanation:
The root-to-leaf path 4->9->5 represents the number 495.
The root-to-leaf path 4->9->1 represents the number 491.
The root-to-leaf path 4->0 represents the number 40.
Therefore, sum = 495 + 491 + 40 = 1026.
```

__Constraints:__
* The number of nodes in the tree is in the range `[1, 1000]`.
* `0 <= Node.val <= 9`
* The depth of the tree will not exceed `10`.

### Solution
__O(n) Recursive:__
```Swift
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public var val: Int
 *     public var left: TreeNode?
 *     public var right: TreeNode?
 *     public init() { self.val = 0; self.left = nil; self.right = nil; }
 *     public init(_ val: Int) { self.val = val; self.left = nil; self.right = nil; }
 *     public init(_ val: Int, _ left: TreeNode?, _ right: TreeNode?) {
 *         self.val = val
 *         self.left = left
 *         self.right = right
 *     }
 * }
 */
class Solution {
    func sumNumbers(_ root: TreeNode?) -> Int {
        guard let root else { return 0 }
        var result: Int = 0
        sum(root, 0, &result)
        return result
    }

    func sum(_ node: TreeNode, _ pathSum: Int, _ result: inout Int) {
        let pathSum: Int = pathSum * 10 + node.val
        if node.left == nil, node.right == nil {
            result += pathSum
        } else {
            [node.left, node.right].forEach {
                guard let next = $0 else { return }
                sum(next, pathSum, &result)
            }
        }
    }
}
```
__O(n) Iterative:__
```Swift
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public var val: Int
 *     public var left: TreeNode?
 *     public var right: TreeNode?
 *     public init() { self.val = 0; self.left = nil; self.right = nil; }
 *     public init(_ val: Int) { self.val = val; self.left = nil; self.right = nil; }
 *     public init(_ val: Int, _ left: TreeNode?, _ right: TreeNode?) {
 *         self.val = val
 *         self.left = left
 *         self.right = right
 *     }
 * }
 */
 extension TreeNode: Hashable {
    public func hash(into hasher: inout Hasher) {
        hasher.combine(ObjectIdentifier(self))
    }

    public static func ==(lhs: TreeNode, rhs: TreeNode) -> Bool {
        lhs === rhs
    }
}

class Solution {
    func sumNumbers(_ root: TreeNode?) -> Int {
        guard let root else { return 0 }
        var result: Int = 0
        var queue: [TreeNode] = [root]
        var sumLookupByNode: [TreeNode: Int] = [:]
        while !queue.isEmpty {
            let count: Int = queue.count
            for _ in 0 ..< count {
                let node: TreeNode = queue.removeLast()
                let sum: Int = (sumLookupByNode[node] ?? 0) * 10 + node.val
                if node.left == nil, node.right == nil {
                    result += sum
                } else {
                    [node.left, node.right].forEach {
                        guard let next = $0 else { return }
                        queue.append(next)
                        sumLookupByNode[next] = sum
                    }
                }
                sumLookupByNode.removeValue(forKey: node)
            }
        }
        return result
    }
}
```