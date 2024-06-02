
### Validate Binary Search Tree

Given the `root` of a binary tree, determine if it is a valid binary search tree (BST).

A __valid BST__ is defined as follows:
* The left subtree of a node contains only nodes with keys __less than__ the node's key.
* The right subtree of a node contains only nodes with keys __greater than__ the node's key.
* Both the left and right subtrees must also be binary search trees.
 
__Example 1:__

![question_98-0.jpg](../images/question_98-0.jpg)
```
Input: root = [2,1,3]
Output: true
```
__Example 2:__

![question_98-1.jpg](../images/question_98-1.jpg)
```
Input: root = [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
```

__Constraints:__
* The number of nodes in the tree is in the range `[1, pow(10, 4)]`.
* `-pow(2, 31) <= Node.val <= pow(2, 31) - 1`

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
    func isValidBST(_ root: TreeNode?) -> Bool {
        isValidBST(root, .min, .max)
    }

    func isValidBST(_ node: TreeNode?, _ min: Int, _ max: Int) -> Bool {
        guard let node else { return true }
        return node.val > min && node.val < max && isValidBST(node.left, min, node.val) && isValidBST(node.right, node.val, max)
    }
}
```
__Recursive Alternative:__
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
    func isValidBST(_ root: TreeNode?) -> Bool {
        isValidBST(root, .min, .max)
    }

    func isValidBST(_ node: TreeNode?, _ min: Int, _ max: Int) -> Bool {
        guard let node else { return true }
        return node.val > min && node.val < max && isValidBST(node.left, min, node.val) && isValidBST(node.right, node.val, max)
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
    func isValidBST(_ root: TreeNode?) -> Bool {
        var min: Int = .min
        var curr: TreeNode? = root
        var stack: [TreeNode] = []

        while curr != nil || !stack.isEmpty {
            while let node = curr {
                stack.append(node)
                curr = node.left
            }

            let node: TreeNode = stack.removeLast()

            if node.val <= min {
                return false
            } else {
                min = node.val
                curr = node.right
            }
        }
        return true
    }
```