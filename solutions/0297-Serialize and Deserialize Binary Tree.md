
### Serialize and Deserialize Binary Tree

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

__Example:__
```
You may serialize the following tree:

    1
   / \
  2   3
     / \
    4   5

as "[1,2,3,null,null,4,5]"
```

__Clarification:__ The above format is the same as how LeetCode serializes a binary tree. You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

__Note:__ Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.

### Solution
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

class Codec {
    func serialize(_ root: TreeNode?) -> String {
        return preOrder(root)
    }
    
    func preOrder(_ node: TreeNode?) -> String {
        guard let node = node else { return "@" }
        return "\(node.val),\(preOrder(node.left)),\(preOrder(node.right))"
    }
    
    func deserialize(_ data: String) -> TreeNode? {
        var data = data.split(separator: ",")
        return preOrder(&data)
    }
    
    func preOrder(_ data: inout [Substring]) -> TreeNode? {
        guard !data.isEmpty else { return nil }
        switch data.removeFirst() {
            case "@":
            return nil
            case let val:
            let node = TreeNode(Int(val)!)
            node.left = preOrder(&data)
            node.right = preOrder(&data)
            return node
        }
    }
}

// Your Codec object will be instantiated and called as such:
// var codec = Codec()
// codec.deserialize(codec.serialize(root))
```