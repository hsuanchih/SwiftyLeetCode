
### Copy List with Random Pointer

A linked list of length `n` is given such that each node contains an additional random pointer, which could point to any node in the list, or `null`.

Construct a __deep copy__ of the list. The deep copy should consist of exactly `n` __brand new__ nodes, where each new node has its value set to the value of its corresponding original node. Both the `next` and `random` pointer of the new nodes should point to new nodes in the copied list such that the pointers in the original list and copied list represent the same list state. __None of the pointers in the new list should point to nodes in the original list__.

For example, if there are two nodes `X` and `Y` in the original list, where `X.random --> Y`, then for the corresponding two nodes `x` and `y` in the copied list, `x.random --> y`.

Return the head of the copied linked list.

The linked list is represented in the input/output as a list of `n` nodes. Each node is represented as a pair of `[val, random_index]` where:

* `val`: an integer representing `Node.val`
* `random_index`: the index of the node (range from `0` to `n-1`) that the `random` pointer points to, or `null` if it does not point to any node.

Your code will only be given the `head` of the original linked list.

__Example 1:__

![question_138-0.png](../images/question_138-0.png)
```
Input: head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
Output: [[7,null],[13,0],[11,4],[10,2],[1,0]]
```
__Example 2:__

![question_138-1.png](../images/question_138-1.png)
```
Input: head = [[1,1],[2,1]]
Output: [[1,1],[2,1]]
```
__Example 3:__

![question_138-2.png](../images/question_138-2.png)
```
Input: head = [[3,null],[3,0],[3,null]]
Output: [[3,null],[3,0],[3,null]]
```

__Constraints:__
* `0 <= n <= 1000`
* `-pow(10, 4) <= Node.val <= pow(10, 4)`
* `Node.random` is `null` or is pointing to some node in the linked list.

### Solution
__O(n) Time, O(n) Space - Recursive:__
```Swift
/**
 * Definition for a Node.
 * public class Node {
 *     public var val: Int
 *     public var next: Node?
 *     public var random: Node?
 *     public init(_ val: Int) {
 *         self.val = val
 *         self.next = nil
 *    	   self.random = nil
 *     }
 * }
 */

class Solution {
    func copyRandomList(_ head: Node?) -> Node? {
        guard let head else { return nil }
        var copy: [Node: Node] = [:]
        return cloneNode(head, &copy)
    }

    func cloneNode(_ node: Node, _ copy: inout [Node: Node]) -> Node {
        if let clone = copy[node] {
            return clone
        } else {
            let clone: Node = Node(node.val)
            copy[node] = clone
            if let next = node.next {
                clone.next = cloneNode(next, &copy)
            }
            if let random = node.random {
                clone.random = cloneNode(random, &copy)
            }
            return clone
        }
    }
}
```
