
### Binary Search Tree Iterator

Implement an iterator over a binary search tree (BST). Your iterator will be initialized with the root node of a BST.

Calling `next()` will return the next smallest number in the BST.

__Example:__

![Example](images/question_173.png)
```
BSTIterator iterator = new BSTIterator(root);
iterator.next();    // return 3
iterator.next();    // return 7
iterator.hasNext(); // return true
iterator.next();    // return 9
iterator.hasNext(); // return true
iterator.next();    // return 15
iterator.hasNext(); // return true
iterator.next();    // return 20
iterator.hasNext(); // return false
```

__Note:__
* `next()` and `hasNext()` should run in average O(1) time and uses O(*h*) memory, where *h* is the height of the tree.
* You may assume that `next()` call will always be valid, that is, there will be at least a next smallest number in the BST when `next()` is called.

### Solution
__O(n) Space:__
```Swift
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public var val: Int
 *     public var left: TreeNode?
 *     public var right: TreeNode?
 *     public init(_ val: Int) {
 *         self.val = val
 *         self.left = nil
 *         self.right = nil
 *     }
 * }
 */

class BSTIterator {
    
    // Store values of BST nodes from largest to smallest
    private var stack : [Int] = []

    // O(n)
    init(_ root: TreeNode?) {
        self.reverseInOrder(root, &stack)
    }
    
    // O(1)
    /** @return the next smallest number */
    func next() -> Int {
        return stack.removeLast()
    }
    
    // O(1)
    /** @return whether we have a next smallest number */
    func hasNext() -> Bool {
        return !stack.isEmpty
    }
    
    // Use reverse in-order traversal to unwind the binary tree onto the stack
    private func reverseInOrder(_ node: TreeNode?, _ stack: inout [Int]) {
        guard let node = node else { return }
        reverseInOrder(node.right, &stack)
        stack.append(node.val)
        reverseInOrder(node.left, &stack)
    }
}

/**
 * Your BSTIterator object will be instantiated and called as such:
 * let obj = BSTIterator(root)
 * let ret_1: Int = obj.next()
 * let ret_2: Bool = obj.hasNext()
 */
```
__O(h) Space:__
```Swift
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public var val: Int
 *     public var left: TreeNode?
 *     public var right: TreeNode?
 *     public init(_ val: Int) {
 *         self.val = val
 *         self.left = nil
 *         self.right = nil
 *     }
 * }
 */

class BSTIterator {

    // Use a stack to track the next greater nodes, not all nodes
    // of the BST will be unwind at once, we only unwind next nodes
    // as needed
    private var stack : [TreeNode] = []

    // O(h)
    init(_ root: TreeNode?) {
        self.reverseInOrder(root)
    }
    
    // O(1)
    /** @return the next smallest number */
    func next() -> Int {
        let node = stack.removeLast()
        defer {
            // If a node has a right subtree, nodes under this subtree
            // need to be processed before the node's parent can be processed
            if let right = node.right {
                reverseInOrder(right)
            }
        }
        return node.val
    }
    
    // O(1) 
    // Each node gets pushed and popped exactly once in next() when iterating over all N nodes.
    // Averages out to 2N * O(1) over N calls to next(), thus O(1) on average
    /** @return whether we have a next smallest number */
    func hasNext() -> Bool {
        return !stack.isEmpty
    }
    
    // Simulate in-order traversal iteratively
    // Push lefty-nodes from the left subtree of a specific node onto the stack
    private func reverseInOrder(_ node: TreeNode?) {
        var node = node
        while let curr = node {
            stack.append(curr)
            node = curr.left
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
