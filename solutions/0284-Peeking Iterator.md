
### Peeking Iterator

Given an Iterator class interface with methods: `next()` and `hasNext()`, design and implement a PeekingIterator that support the `peek()` operation -- it essentially `peek()` at the element that will be returned by the next call to `next()`.

__Example:__
```
Assume that the iterator is initialized to the beginning of the list: [1,2,3].

Call next() gets you 1, the first element in the list.
Now you call peek() and it returns 2, the next element. Calling next() after that still return 2. 
You call next() the final time and it returns 3, the last element. 
Calling hasNext() after that should return false.
```
__Follow up:__
How would you extend your design to be generic and work with all types, not just integer?

### Solution
```Swift
// Swift IndexingIterator refernence:
// https://developer.apple.com/documentation/swift/indexingiterator

class PeekingIterator {
    
    private var elements : IndexingIterator<Array<Int>>
    private var curr : Int?
    
    init(_ arr: IndexingIterator<Array<Int>>) {
        elements = arr
        curr = elements.next()
    }
    
    func next() -> Int {
        let temp = curr
        curr = elements.next()
        return temp!
    }
    
    func peek() -> Int {
        return curr!
    }
    
    func hasNext() -> Bool {
        return curr != nil
    }
}

/**
 * Your PeekingIterator object will be instantiated and called as such:
 * let obj = PeekingIterator(arr)
 * let ret_1: Int = obj.next()
 * let ret_2: Int = obj.peek()
 * let ret_3: Bool = obj.hasNext()
 */
```