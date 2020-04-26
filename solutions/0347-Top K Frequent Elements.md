
### Top K Frequent Elements

Given a non-empty array of integers, return the *__k__* most frequent elements.

__Example 1:__
```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```
__Example 2:__
```
Input: nums = [1], k = 1
Output: [1]
```

__Note:__
* You may assume *k* is always valid, 1 ≤ *k* ≤ number of unique elements.
* Your algorithm's time complexity __must be__ better than O(*n* log *n*), where *n* is the array's size.

### Solution
__O(u\*log(u)+nums+k) where u is the number of unique elements in nums (upperbound O(nums\*log\[\base 2](nums))):__
```Swift
class Solution {
    func topKFrequent(_ nums: [Int], _ k: Int) -> [Int] {
        
        // tally tracks the number of occurences of each element in nums
        let tally = nums.reduce(into: [Int: Int]()) { $0[$1, default: 0]+=1 }
        var i = 0, result : [Int] = Array(repeating: 0, count: k)
        
        // Sort tally by value (ie, frequency of occurrence) and iterate through
        // tally to populate result with the corresponding key (element in nums)
        for (key, _) in (tally.sorted { $0.value > $1.value }) {
            if i == k {
                return result
            }
            result[i] = key
            i+=1
        }
        return result
    }
}
```
__Using PriorityQueue:__
```Swift
public protocol QueueOperations {
    associatedtype Element
    var isEmpty : Bool { get }
    var count : Int { get }
    func peek() -> Element?
    mutating func enqueue(_ element: Element)
    mutating func dequeue() -> Element?
}

public struct PriorityQueue<Element> {
    private var elements : [Element]
    private let comparator : (Element, Element) -> Bool
    
    public init(elements: [Element] = [], comparator: @escaping (Element, Element) -> Bool) {
        self.elements = elements
        self.comparator = comparator
        buildHeap()
    }
    
    private mutating func buildHeap() {
        stride(from: elements.count/2-1, to: -1, by: -1).forEach { heapifyDown($0) }
    }
    
    private mutating func heapifyUp(_ index: Int) {
        guard let parent = parent(of: index), comparator(elements[index], elements[parent]) else { return }
        elements.swapAt(index, parent)
        heapifyUp(parent)
    }
    
    private mutating func heapifyDown(_ index: Int) {
        let leftChild = self.leftChild(of: index), rightChild = self.rightChild(of: index)
        var nextIndex = index
        switch (compare(index, leftChild), compare(index, rightChild)) {
        case (false, false):
            nextIndex = compare(leftChild, rightChild) ? leftChild! : rightChild!
        case (true, false):
            nextIndex = rightChild!
        case (false, true):
            nextIndex = leftChild!
        case (true, true):
            return
        }
        elements.swapAt(index, nextIndex)
        heapifyDown(nextIndex)
    }
    
    private func parent(of index: Int) -> Int? {
        return index == 0 ? nil : (index-1)/2
    }
    
    private func leftChild(of index: Int) -> Int? {
        let child = index*2+1
        return child < elements.count ? child : nil
    }
    
    private func rightChild(of index: Int) -> Int? {
        let child = (index+1)*2
        return child < elements.count ? child : nil
    }
    
    private func compare(_ lhs: Int?, _ rhs: Int?) -> Bool {
        switch (lhs, rhs) {
        case let (.some(lhs), .some(rhs)):
            return comparator(elements[lhs], elements[rhs])
        case (.some(_), .none):
            return true
        case (.none, .some(_)):
            return false
        case (.none, .none):
            return true
        }
    }
}

extension PriorityQueue : QueueOperations {
    public typealias Element = Element
    
    public var isEmpty : Bool {
        return elements.isEmpty
    }
    
    public var count : Int {
        return elements.count
    }
    
    public func peek() -> Element? {
        return elements.first
    }
    
    public mutating func enqueue(_ element: Element) {
        elements.append(element)
        heapifyUp(count-1)
    }
    
    public mutating func dequeue() -> Element? {
        guard !isEmpty else { return nil }
        elements.swapAt(0, count - 1)
        let element = elements.removeLast()
        if !isEmpty {
            heapifyDown(0)
        }
        return element
    }
}
```
__O(nums\*log\[base 2\](k)) Time, O(nums) Space - PriorityQueue:__
```Swift
class Solution {
    func topKFrequent(_ nums: [Int], _ k: Int) -> [Int] {
        let tally = nums.reduce(into: [Int: Int]()) { $0[$1, default: 0]+=1 }
        var minHeap = PriorityQueue<(Int, Int)> { $0.1 < $1.1 }
        for (num, count) in tally {
            minHeap.enqueue((num, count))
            if minHeap.count > k {
                _ = minHeap.dequeue()
            }
        }
        var result : [Int] = []
        while !minHeap.isEmpty {
            result.append(minHeap.dequeue()!.0)
        }
        return result
    }
}
```