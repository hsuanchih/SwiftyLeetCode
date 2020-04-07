
### Maximum Depth of N-ary Tree

Given a n-ary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

*Nary-Tree input serialization is represented in their level order traversal, each group of children is separated by the null value (See examples).*

__Example 1:__

![Example 1](images\question_559-0.png)

```
Input: root = [1,null,3,2,4,null,5,6]
Output: 3
```
__Example 2:__

![Example 2](images\question_559-1.png)

```
Input: root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
Output: 5
```

__Constraints:__
* The depth of the n-ary tree is less than or equal to `1000`.
* The total number of nodes is between `[0, 10^4]`.

### Solution
__O(n) Time, O(1) Space - Recursive:__
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
    func maxDepth(_ root: Node?) -> Int {
        guard let node = root else { return 0 }
        return 1 + node.children.reduce(0) { max($0, maxDepth($1)) }
    }
}
```
__O(n) Time, O(n) Space - Iterative:__
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
    func maxDepth(_ root: Node?) -> Int {
        guard let root = root else { return 0 }
        var queue = [root], depth = 0
        while !queue.isEmpty {
            let length = queue.count
            for _ in 0..<length {
                let curr = queue.removeFirst()
                curr.children.lazy.compactMap { $0 }.forEach {
                    queue.append($0)
                }
            }
            depth+=1
        }
        return depth
    }
}
```