
### LRU Cache

Design and implement a data structure for Least Recently Used (LRU) cache. It should support the following operations:</br> 
`get` and `put`.

`get(key)` - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return `-1`.
`put(key, value)` - Set or insert the value if the key is not already present. When the cache reached its capacity,</br> 
it should invalidate the least recently used item before inserting a new item.

The cache is initialized with a __positive__ capacity.

__Follow up:__
Could you do both operations in __O(1)__ time complexity?

__Example:__
```
LRUCache cache = new LRUCache( 2 /* capacity */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // returns 1
cache.put(3, 3);    // evicts key 2
cache.get(2);       // returns -1 (not found)
cache.put(4, 4);    // evicts key 1
cache.get(1);       // returns -1 (not found)
cache.get(3);       // returns 3
cache.get(4);       // returns 4
```

### Solution
__Recursive O(n):__
```Swift
class LRUCache {
    
    // A Doubly-LinkedList node implementation
    // Note:
    // Key is necessary here only to preserve O(1) run time on eviction when new item is inserted with LRU at full capacity.
    // When we remove the node at the end of the LinkedList, we need to remove the node also from the lookup, which requires
    // referencing via the key.
    // Alternatively we can remove the node from the lookup without requiring the key, but that will make removal an O(n) operation.
    private class ListNode<Key, Value> {
        var prev : ListNode<Key, Value>?, next : ListNode<Key, Value>?
        var key : Key?
        var val : Value
        init(key: Key? = nil, val: Value) {
            self.key = key
            self.val = val
        }
    }
    
    private let head : ListNode<Int, Int> = ListNode<Int, Int>(val: -1), tail : ListNode<Int, Int> = ListNode<Int, Int>(val: -1)
    
    private var lookup : [Int: ListNode<Int, Int>] = [:]
    private let capacity : Int

    // O(1)
    init(_ capacity: Int) {
        self.capacity = capacity
        head.next = tail
        tail.next = head
    }
    
    // O(1)
    func get(_ key: Int) -> Int {
        guard let node = lookup[key] else { return -1 }
        
        // Accessing an existing item should move the item to the front
        // of the LRU Cache, call put to remove the item & add it to the front
        put(key, node.val)
        return node.val
    }
    
    // O(1)
    func put(_ key: Int, _ value: Int) {
        
        // If key already exists in the lookup, a put should move
        // its value to the front of the LRU, remove it from the list &
        // the lookup, we will add it fresh
        if let _ = lookup[key] {
            remove(key)
        }
        
        // If we've reached the LRU's capacity, remove the least recently
        // accessed item from the list so that we can add a new item
        if lookup.count == capacity, let key = tail.prev?.key {
            remove(key)
        }
        
        // Create a new ListNode, we will add the node to the list & the lookup
        let node = ListNode(key: key, val: value)
        insert(node)
        lookup[key] = node
    }
    
    private func remove(_ key: Int) {
        guard let node = lookup[key] else { return }
        lookup.removeValue(forKey: key)
        remove(node)
    }
    
    private func insert(_ node: ListNode<Int, Int>) {
        let front = head.next
        head.next = node
        node.prev = head
        node.next = front
        front?.prev = node
    }
    
    private func remove(_ node: ListNode<Int, Int>) {
        node.next?.prev = node.prev
        node.prev?.next = node.next
        node.prev = nil
        node.next = nil
    }
}
```