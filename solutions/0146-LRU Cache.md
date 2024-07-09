
### LRU Cache

Design a data structure that follows the constraints of a __Least Recently Used (LRU) cache__.

Implement the `LRUCache` class:
* `LRUCache(int capacity)` Initialize the LRU cache with positive size `capacity`.
* `int get(int key)` Return the value of the key if the `key` exists, otherwise return `-1`.
* `void put(int key, int value)` Update the `value` of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, evict the least recently used key.

The functions `get` and `put` must each run in `O(1)` average time complexity.

__Example 1:__
```
Input
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
Output
[null, null, null, 1, null, -1, null, -1, 3, 4]

Explanation
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4
```

__Constraints:__
* `1 <= capacity <= 3000`
* `0 <= key <= pow(10, 4)`
* `0 <= value <= pow(10, 5)`
* At most `2 * pow(10, 5)` calls will be made to `get` and `put`.

### Solution
__Key-Value Store + Doubly Linked List:__
```Swift
class LRUCache {
    private let capacity: Int
    private let head: Node
    private let tail: Node
    private var lookup: [Int: Node]

    init(_ capacity: Int) {
        self.capacity = capacity
        head = Node(key: nil, value: -1)
        tail = Node(key: nil, value: -1)
        head.next = tail
        tail.prev = head
        lookup = [:]
    }
    
    // O(1)
    func get(_ key: Int) -> Int {
        if let node = lookup[key] {
            // Accessing an existing item should move the item to the front
            // of the LRU Cache, call put to remove the item & add it to the front
            put(key, node.value)
            return node.value
        } else {
            return -1
        }
    }
    
    // O(1)
    func put(_ key: Int, _ value: Int) {
        // If key already exists in the lookup, a put should move
        // its value to the front of the LRU, remove it from the list &
        // the lookup, we will add it fresh
        if let node = lookup[key] {
            remove(node)
        }

        // Create a new ListNode, we will add the node to the list & the lookup
        insert(Node(key: key, value: value))

        // If we've exceeded the LRU's capacity, remove the least recently
        // accessed item from the list
        if lookup.count > capacity, let last = tail.prev {
            remove(last)
        }
    }

    private func insert(_ node: Node) {
        if let key = node.key {
            lookup[key] = node
            node.next = head.next
            node.prev = head
            node.next?.prev = node
            head.next = node
        } else {
            fatalError()
        }
    }

    private func remove(_ node: Node) {
        if let key = node.key {
            lookup.removeValue(forKey: key)
            node.prev?.next = node.next
            node.next?.prev = node.prev
        } else {
            fatalError()
        }
    }
}

extension LRUCache {
    // A Doubly-LinkedList node implementation
    // Note:
    // Key is necessary here only to preserve O(1) run time on eviction when new item is inserted with LRU at full capacity.
    // When we remove the node at the end of the LinkedList, we need to remove the node also from the lookup, which requires
    // referencing via the key.
    // Alternatively we can remove the node from the lookup without requiring the key, but that will make removal an O(n) operation.
    typealias Node = ListNode<Int?, Int>
    class ListNode<Key, Value> {
        var prev : ListNode<Key, Value>?
        var next : ListNode<Key, Value>?
        let key: Key
        let value: Value

        init(key: Key, value: Value) {
            (self.key, self.value) = (key, value)
        }
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * let obj = LRUCache(capacity)
 * let ret_1: Int = obj.get(key)
 * obj.put(key, value)
 */
```