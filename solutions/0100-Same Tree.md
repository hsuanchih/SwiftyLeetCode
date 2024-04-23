
### Same Tree

Given the roots of two binary trees `p` and `q`, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

__Example 1:__

![question_100-0.jpg](../images/question_100-0.jpg)
```
Input: p = [1,2,3], q = [1,2,3]
Output: true
```
__Example 2:__

![question_100-1.jpg](../images/question_100-1.jpg)
```
Input: p = [1,2], q = [1,null,2]
Output: false
```
__Example 3:__

![question_100-2.jpg](../images/question_100-2.jpg)
```
Input: p = [1,2,1], q = [1,1,2]
Output: false
```

__Constraints:__
* The number of nodes in both trees is in the range `[0, 100]`.
* `-pow(10, 4) <= Node.val <= pow(10, 4)`

### Solution
__Recursive:__
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
    func isSameTree(_ p: TreeNode?, _ q: TreeNode?) -> Bool {
        switch (p, q) {
        case (.none, .none):
            return true 
        case (.some(let p), .some(let q)):
            return p.val == q.val && isSameTree(p.left, q.left) && isSameTree(p.right, q.right)
        case (.some, .none), (.none, .some):
            return false
        }
    }
}
```
__Iterative:__
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
    func isSameTree(_ p: TreeNode?, _ q: TreeNode?) -> Bool {
        switch (p, q) {
        case (.none, .none):
            return true
        case (.none, .some), (.some, .none):
            return false
        case (.some(let p), .some(let q)) where p.val == q.val:
            var pQ: [TreeNode] = [p]
            var qQ: [TreeNode] = [q]
            while !pQ.isEmpty && !qQ.isEmpty && pQ.count == qQ.count {
                let count: Int = pQ.count
                for _ in 0 ..< count {
                    let p: TreeNode = pQ.removeFirst()
                    let q: TreeNode = qQ.removeFirst()
                    if let pLeft = p.left, let qLeft = q.left, pLeft.val == qLeft.val {
                        pQ.append(pLeft)
                        qQ.append(qLeft)
                    } else if p.left?.val != q.left?.val {
                        return false
                    }

                    if let pRight = p.right, let qRight = q.right, pRight.val == qRight.val {
                        pQ.append(pRight)
                        qQ.append(qRight)
                    } else if p.right?.val != q.right?.val {
                        return false
                    }
                }
            }
            return true
        default:
            return false
        }
    }
}
```