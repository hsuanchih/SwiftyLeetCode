
### All Nodes Distance K in Binary Tree

We are given a binary tree (with root node `root`), a `target` node, and an integer value `K`.

Return a list of the values of all nodes that have a distance `K` from the `target` node.  The answer can be returned in any order.

__Example 1:__


```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], target = 5, K = 2

Output: [7,4,1]

Explanation: 
The nodes that are a distance 2 from the target node (with value 5)
have values 7, 4, and 1.

Note that the inputs "root" and "target" are actually TreeNodes.
The descriptions of the inputs above are just serializations of these objects.
```

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
extension TreeNode : Equatable {
    public static func ==(lhs: TreeNode, rhs: TreeNode) -> Bool {
        return ObjectIdentifier(lhs) == ObjectIdentifier(rhs)
    }
}
extension TreeNode : Hashable {
    public func hash(into hasher: inout Hasher) {
        hasher.combine(ObjectIdentifier(self).hashValue)
    }
}
class Solution {
    func distanceK(_ root: TreeNode?, _ target: TreeNode?, _ K: Int) -> [Int] {
        guard let root = root, let target = target else { return [] }
        var adjList : [TreeNode: Set<TreeNode>] = [:]
        preOrder(root, nil, &adjList)
        var visited : Set<TreeNode> = [target], queue : [TreeNode] = [target]
        var level = 0

        // Walk the adjacency list using BFS, and return the result for level K
        while !queue.isEmpty {
            let size = queue.count
            var temp : [Int] = []
            for _ in 0..<size {
                let node = queue.removeFirst()
                adjList[node]?.forEach {
                    if !visited.contains($0) {
                        visited.insert($0)
                        queue.append($0)
                    }
                }
                temp.append(node.val)
            }
            if level == K {
                return temp
            }
            level+=1
        }
        return []
    }
    
    // Pre-Order method to construct adjacency list
    func preOrder(_ node: TreeNode?, _ parent: TreeNode?, _ adjList: inout [TreeNode: Set<TreeNode>]) {
        guard let node = node else { return }
        adjList[node] = Set<TreeNode>()
        if let parent = parent {
            adjList[node]!.insert(parent)
        }
        if let left = node.left {
            adjList[node]!.insert(left)
            preOrder(left, node, &adjList)
        }
        if let right = node.right {
            adjList[node]!.insert(right)
            preOrder(right, node, &adjList)
        }
    }
}
```