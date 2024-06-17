
### Binary Search Tree Iterator

Implement the `BSTIterator` class that represents an iterator over the __in-order traversal__ of a binary search tree (BST):
* `BSTIterator(TreeNode root)` Initializes an object of the `BSTIterator` class. The `root` of the BST is given as part of the constructor. The pointer should be initialized to a non-existent number smaller than any element in the BST.
* `boolean hasNext()` Returns `true` if there exists a number in the traversal to the right of the pointer, otherwise returns `false`.
* `int next()` Moves the pointer to the right, then returns the number at the pointer.

Notice that by initializing the pointer to a non-existent smallest number, the first call to `next()` will return the smallest element in the BST.

You may assume that `next()` calls will always be valid. That is, there will be at least a next number in the in-order traversal when `next()` is called.

__Example 1:__

![question_173.png](../images/question_173.png)
```
Input
["BSTIterator", "next", "next", "hasNext", "next", "hasNext", "next", "hasNext", "next", "hasNext"]
[[[7, 3, 15, null, null, 9, 20]], [], [], [], [], [], [], [], [], []]
Output
[null, 3, 7, true, 9, true, 15, true, 20, false]

Explanation
BSTIterator bSTIterator = new BSTIterator([7, 3, 15, null, null, 9, 20]);
bSTIterator.next();    // return 3
bSTIterator.next();    // return 7
bSTIterator.hasNext(); // return True
bSTIterator.next();    // return 9
bSTIterator.hasNext(); // return True
bSTIterator.next();    // return 15
bSTIterator.hasNext(); // return True
bSTIterator.next();    // return 20
bSTIterator.hasNext(); // return False
```

__Constraints:__
* The number of nodes in the tree is in the range `[1, pow(10, 5)]`.
* `0 <= Node.val <= pow(10, 6)`
* At most `pow(10, 5)` calls will be made to `hasNext`, and `next`.
 

__Follow up:__
* Could you implement `next()` and `hasNext()` to run in average `O(1)` time and use `O(h)` memory, where `h` is the height of the tree?

### Solution
__O(tree) Space:__
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

class BSTIterator {
    private var stack: [Int]

    init(_ root: TreeNode?) {
        stack = []
        reverseInOrder(root)
    }
    
    func next() -> Int {
        stack.removeLast()
    }
    
    func hasNext() -> Bool {
        !stack.isEmpty
    }

    func reverseInOrder(_ node: TreeNode?) {
        guard let node else { return }
        reverseInOrder(node.right)
        stack.append(node.val)
        reverseInOrder(node.left)
    }
}

/**
 * Your BSTIterator object will be instantiated and called as such:
 * let obj = BSTIterator(root)
 * let ret_1: Int = obj.next()
 * let ret_2: Bool = obj.hasNext()
 */
```
__O(height) Space:__
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

class BSTIterator {
    private var stack: [TreeNode]

    init(_ root: TreeNode?) {
        stack = []
        pushLeftChildren(root)
    }
    
    func next() -> Int {
        let next: TreeNode = stack.removeLast()
        defer { pushLeftChildren(next.right) }
        return next.val
    }
    
    func hasNext() -> Bool {
        !stack.isEmpty
    }

    func pushLeftChildren(_ node: TreeNode?) {
        var node: TreeNode? = node
        while let current = node {
            stack.append(current)
            node = current.left
        }
    }
}

/**
 * Your BSTIterator object will be instantiated and called as such:
 * let obj = BSTIterator(root)
 * let ret_1: Int = obj.next()
 * let ret_2: Bool = obj.hasNext()
 */
```
