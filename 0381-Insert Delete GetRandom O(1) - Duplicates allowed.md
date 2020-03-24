
### Insert Delete GetRandom O(1) - Duplicates allowed

Design a data structure that supports all following operations in average __O(1)__ time.

__Note: Duplicate elements are allowed.__
1. `insert(val):` Inserts an item val to the collection.
2. `remove(val):` Removes an item val from the collection if present.
3. `getRandom:` Returns a random element from current collection of elements. The probability of each element being returned is linearly related to the number of same value the collection contains.

__Example:__
```
// Init an empty collection.
RandomizedCollection collection = new RandomizedCollection();

// Inserts 1 to the collection. Returns true as the collection did not contain 1.
collection.insert(1);

// Inserts another 1 to the collection. Returns false as the collection contained 1. Collection now contains [1,1].
collection.insert(1);

// Inserts 2 to the collection, returns true. Collection now contains [1,1,2].
collection.insert(2);

// getRandom should return 1 with the probability 2/3, and returns 2 with the probability 1/3.
collection.getRandom();

// Removes 1 from the collection, returns true. Collection now contains [1,2].
collection.remove(1);

// getRandom should return 1 and 2 both equally likely.
collection.getRandom();
```

### Solution
```Swift
class RandomizedCollection {

    var lookup : [Int: Set<Int>]
    var array : [Int]
    /** Initialize your data structure here. */
    init() {
        lookup = [:]
        array = []
    }
    
    /** Inserts a value to the collection. Returns true if the collection did not already contain the specified element. */
    func insert(_ val: Int) -> Bool {
        defer{ array.append(val) }
        if var indices = lookup[val] {
            indices.insert(array.count)
            lookup[val] = indices
            return false
        } else {
            lookup[val] = [array.count]
            return true
        }
    }
    
    /** Removes a value from the collection. Returns true if the collection contained the specified element. */
    func remove(_ val: Int) -> Bool {
        guard var indices = lookup[val], let index = indices.first else { return false }
        let last = array.last!
        if last != val {
            array.swapAt(index, array.count-1)
            lookup[last]!.remove(array.count-1)
            lookup[last]!.insert(index)
            indices.remove(index)
        } else {
            indices.remove(array.count-1)
        }
        
        if indices.isEmpty {
            lookup.removeValue(forKey: val)
        } else {
            lookup[val] = indices
        }
        array.removeLast()
        
        return true
    }
    
    /** Get a random element from the collection. */
    func getRandom() -> Int {
        return array[Int.random(in: 0..<array.count)]
    }
}

/**
 * Your RandomizedCollection object will be instantiated and called as such:
 * let obj = RandomizedCollection()
 * let ret_1: Bool = obj.insert(val)
 * let ret_2: Bool = obj.remove(val)
 * let ret_3: Int = obj.getRandom()
 */
```