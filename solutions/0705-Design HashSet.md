
### Design HashSet

Design a HashSet without using any built-in hash table libraries.

To be specific, your design should include these functions:
* `add(value)`: Insert a value into the HashSet. 
* `contains(value)`: Return whether the value exists in the HashSet or not.
* `remove(value)`: Remove a value in the HashSet. If the value does not exist in the HashSet, do nothing.

__Example:__
```
MyHashSet hashSet = new MyHashSet();
hashSet.add(1);         
hashSet.add(2);         
hashSet.contains(1);    // returns true
hashSet.contains(3);    // returns false (not found)
hashSet.add(2);          
hashSet.contains(2);    // returns true
hashSet.remove(2);          
hashSet.contains(2);    // returns false (already removed)
```

__Note:__
* All values will be in the range of `[0, 1000000]`.
* The number of operations will be in the range of `[1, 10000]`.
* Please do not use the built-in HashSet library.

### Solution
```Swift
class MyHashSet {
    private var lookup : [Bool]

    // O(1)
    /** Initialize your data structure here. */
    init() {
        lookup = Array(repeating: false, count: 1000000+1)
    }
    
    // O(1)
    func add(_ key: Int) {
        lookup[key] = true
    }
    
    // O(1)
    func remove(_ key: Int) {
        lookup[key] = false
    }
    
    // O(1)
    /** Returns true if this set contains the specified element */
    func contains(_ key: Int) -> Bool {
        return lookup[key]
    }
}

/**
 * Your MyHashSet object will be instantiated and called as such:
 * let obj = MyHashSet()
 * obj.add(key)
 * obj.remove(key)
 * let ret_3: Bool = obj.contains(key)
 */
```