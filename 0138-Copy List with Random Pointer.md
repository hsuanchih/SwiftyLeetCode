
### Copy List with Random Pointer

A linked list is given such that each node contains an additional random pointer which could point to any node in the list or `null`.

Return a __deep copy__ of the list.

The Linked List is represented in the input/output as a list of n nodes. Each node is represented as a pair of `[val, random_index]` where:
* `val`: an integer representing `Node.val`
* `random_index`: the index of the node (range from `0` to `n-1`) where random pointer points to, or `null` if it does not point to any node.
 

__Example 1:__

```
Input: head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
Output: [[7,null],[13,0],[11,4],[10,2],[1,0]]
```
__Example 2:__

```
Input: head = [[1,1],[2,1]]
Output: [[1,1],[2,1]]
```
__Example 3:__

```
Input: head = [[3,null],[3,0],[3,null]]
Output: [[3,null],[3,0],[3,null]]
```
__Example 4:__
```
Input: head = []
Output: []
Explanation: Given linked list is empty (null pointer), so return null.
```

__Constraints:__
* `-10000 <= Node.val <= 10000`
* `Node.random` is null or pointing to a node in the linked list.
* Number of Nodes will not exceed 1000.

### Solution
__O(n) Time, O(1) Space - Recursive:__
```Swift
class Solution {
    func copyRandomList(_ head: Node?) -> Node? {
        guard let head = head else { return nil }
        var lookup : [Node: Node] = [:]
        return cloneNode(head, &lookup)
    }
    
    func cloneNode(_ node: Node, _ lookup: inout [Node: Node]) -> Node {
        guard let clone = lookup[node] else {
            let clone = Node(node.val)

            // Important: Need to insert the clone into the lookup before
            // calling cloneNode on next & random to prevent clone cycle
            lookup[node] = clone

            if let next = node.next {
                clone.next = cloneNode(next, &lookup)
            }
            if let random = node.random {
                clone.random = cloneNode(random, &lookup)
            }
            return clone
        }
        return clone
    }
}
```