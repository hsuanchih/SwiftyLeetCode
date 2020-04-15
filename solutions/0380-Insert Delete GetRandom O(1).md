
### Insert Delete GetRandom O(1)

Design a data structure that supports all following operations in *average* __O(1)__ time.

1. `insert(val)`: Inserts an item val to the set if not already present.
2. `remove(val)`: Removes an item val from the set if present.
3. `getRandom`: Returns a random element from current set of elements. Each element must have the __same probability__ of being returned.

__Example:__
```
// Init an empty set.
RandomizedSet randomSet = new RandomizedSet();

// Inserts 1 to the set. Returns true as 1 was inserted successfully.
randomSet.insert(1);

// Returns false as 2 does not exist in the set.
randomSet.remove(2);

// Inserts 2 to the set, returns true. Set now contains [1,2].
randomSet.insert(2);

// getRandom should return either 1 or 2 randomly.
randomSet.getRandom();

// Removes 1 from the set, returns true. Set now contains [2].
randomSet.remove(1);

// 2 was already in the set, so return false.
randomSet.insert(2);

// Since 2 is the only number in the set, getRandom always return 2.
randomSet.getRandom();
```

### Solution
```Swift
class RandomizedSet {

    // Array stores the elements currently in the random set
    private var array : [Int]
    
    // Lookup stores the index of each element in the array
    private var lookup : [Int: Int]
    
    /** Initialize your data structure here. */
    init() {
        lookup = [:]
        array = []
    }
    
    // O(1)
    /** Inserts a value to the set. Returns true if the set did not already contain the specified element. */
    func insert(_ val: Int) -> Bool {
        
        // If the element is already in the array, return false
        if let index = lookup[val] {
            return false
        }
        
        // Insert element at the end of the array & update lookup accordingly
        lookup[val] = array.count
        array.append(val)
        return true
    }
    
    // O(1)
    /** Removes a value from the set. Returns true if the set contained the specified element. */
    func remove(_ val: Int) -> Bool {
        
        // If element is not in the array, it cannot be removed - return false
        guard let index = lookup[val] else { return false }
        
        // Element to be removed is at index
        // Swap the element to be removed to the end of the array
        // and the last element of the array to index
        // then remove the last element of the array
        array.swapAt(index, array.count-1)
        array.removeLast()
        
        // Update lookup
        lookup.removeValue(forKey: val)
        if index < array.count {
            lookup[array[index]] = index
        }
        
        return true
    }
    
    // O(1)
    /** Get a random element from the set. */
    func getRandom() -> Int {
        return array[Int.random(in: 0..<array.count)]
    }
}

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * let obj = RandomizedSet()
 * let ret_1: Bool = obj.insert(val)
 * let ret_2: Bool = obj.remove(val)
 * let ret_3: Int = obj.getRandom()
 */
```