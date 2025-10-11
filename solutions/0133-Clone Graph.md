
### Clone Graph

Given a reference of a node in a connected undirected graph.

Return a deep copy (clone) of the graph.

Each node in the graph contains a value (`int`) and a list (`List[Node]`) of its neighbors.
```
class Node {
    public int val;
    public List<Node> neighbors;
}
```

__Test case format:__

For simplicity sake, each node's value is the same as the node's index (1-indexed).</br> 
For example, the first node with `val = 1`, the second node with `val = 2`, and so on.</br> 
The graph is represented in the test case using an adjacency list.

__Adjacency list__ is a collection of unordered lists used to represent a finite graph.</br> 
Each list describes the set of neighbors of a node in the graph.

The given node will always be the first node with `val = 1`.</br> 
You must return the copy of the given node as a reference to the cloned graph.

__Example 1:__

![question_133-0.png](../images/question_133-0.png)
```
Input: adjList = [[2,4],[1,3],[2,4],[1,3]]
Output: [[2,4],[1,3],[2,4],[1,3]]
Explanation: There are 4 nodes in the graph.
1st node (val = 1)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
2nd node (val = 2)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
3rd node (val = 3)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
4th node (val = 4)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
```
__Example 2:__

![question_133-1.png](../images/question_133-1.png)
```
Input: adjList = [[]]
Output: [[]]
Explanation: Note that the input contains one empty list. The graph consists of only one node with val = 1 and it does not have any neighbors.
```
__Example 3:__
```
Input: adjList = []
Output: []
Explanation: This an empty graph, it does not have any nodes.
```

__Constraints:__
* `1 <= Node.val <= 100`
* `Node.val` is unique for each node.
* Number of Nodes will not exceed 100.
* There is no repeated edges and no self-loops in the graph.
* The Graph is connected and all nodes can be visited starting from the given node.

### Solution
__O(V+E) Iterative:__
```Swift
/**
 * Definition for a Node.
 * public class Node {
 *     public var val: Int
 *     public var neighbors: [Node?]
 *     public init(_ val: Int) {
 *         self.val = val
 *         self.neighbors = []
 *     }
 * }
 */
class Solution {
    func cloneGraph(_ node: Node?) -> Node? {
        guard let node = node else { return nil }

        // Lookup table here serves 2 purposes:
        // 1. Tracking the visited nodes to prevent cycle, and
        // 2. Maintaining the assoication between the original & cloned nodes
        var queue: [Node] = [node], lookup: [Int: Node] = [node.val: Node(node.val)]

        while !queue.isEmpty {
            let size = queue.count
            for _ in 0..<size {

                // The clone is guaranteed to exist for curr
                let curr = queue.removeFirst(), clone = lookup[curr.val]!

                // Iterate through the node's neighbors
                for node in curr.neighbors {
                    var neighbor = lookup[node!.val]

                    // If the node has not been visited (ie, the clone does not exist)
                    // 1. Add the original node to the queue
                    // 2. Create a clone of it & add the clone to the lookup (ie, mark as seen)
                    if neighbor == nil {
                        queue.append(node!)
                        neighbor = Node(node!.val)
                        lookup[node!.val] = neighbor
                    }

                    // Assign the neighboring relationship amongst clones
                    clone.neighbors.append(neighbor!)
                }
            }
        }
        return lookup[node.val]
    }
}
```
__O(V+E) Recursive:__
```Swift
/**
 * Definition for a Node.
 * public class Node {
 *     public var val: Int
 *     public var neighbors: [Node?]
 *     public init(_ val: Int) {
 *         self.val = val
 *         self.neighbors = []
 *     }
 * }
 */

class Solution {
    func cloneGraph(_ node: Node?) -> Node? {
        guard let node else { return nil }
        var lookup: [Int: Node] = [:]
        return cloneNode(node, &lookup)
    }

    func cloneNode(_ node: Node, _ lookup: inout [Int: Node]) -> Node {
        if let clone = lookup[node.val] {
            return clone
        } else {
            let clone = Node(node.val)
            lookup[clone.val] = clone
            node.neighbors.forEach {
                if let neighbor = $0 {
                    clone.neighbors.append(cloneNode(neighbor, &lookup))
                }
            }
            return clone
        }
    }
}
```