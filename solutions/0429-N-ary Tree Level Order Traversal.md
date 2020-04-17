
### N-ary Tree Level Order Traversal

Given an n-ary tree, return the *level order* traversal of its nodes' values.

*Nary-Tree input serialization is represented in their level order traversal, each group of children is separated by the null value (See examples).*

__Example 1:__

![example 1](images/question_429-0.png)

```
Input: root = [1,null,3,2,4,null,5,6]
Output: [[1],[3,2,4],[5,6]]
```
__Example 2:__

![example 2](images/question_429-1.png)

```
Input: root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
Output: [[1],[2,3,4,5],[6,7,8,9,10],[11,12,13],[14]]
```

__Constraints:__
* The height of the n-ary tree is less than or equal to `1000`
* The total number of nodes is between `[0, 10^4]`

### Solution
__O(n) Time, O(n/2) Space - Iterative:__
```Swift
/**
 * Definition for a Node.
 * public class Node {
 *     public var val: Int
 *     public var children: [Node]
 *     public init(_ val: Int) {
 *         self.val = val
 *         self.children = []
 *     }
 * }
 */

class Solution {
    func levelOrder(_ root: Node?) -> [[Int]] {
        guard let root = root else { return [] }
        var queue = [root], result : [[Int]] = []
        while !queue.isEmpty {
            let size = queue.count
            var temp : [Int] = []
            for _ in 0..<size {
                let curr = queue.removeFirst()
                curr.children.forEach {
                    queue.append($0)
                }
                temp.append(curr.val)
            }
            result.append(temp)
        }
        return result
    }
}
```
__O(n) Time, O(1) Space - Recursive Pre-Order:__
```Swift
/**
 * Definition for a Node.
 * public class Node {
 *     public var val: Int
 *     public var children: [Node]
 *     public init(_ val: Int) {
 *         self.val = val
 *         self.children = []
 *     }
 * }
 */

class Solution {
    func levelOrder(_ root: Node?) -> [[Int]] {
        var result : [[Int]] = []
        preOrder(root, 0, &result)
        return result
    }
    
    func preOrder(_ node: Node?, _ level: Int, _ result: inout [[Int]]) {
        guard let node = node else { return }
        if level == result.count {
            result.append([])
        }
        result[level].append(node.val)
        node.children.forEach {
            preOrder($0, level+1, &result)
        }
    }
}
```