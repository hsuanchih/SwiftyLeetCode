
### Find Median from Data Stream

Median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle value.

For example,
`[2,3,4]`, the median is `3`

`[2,3]`, the median is `(2 + 3) / 2 = 2.5`

Design a data structure that supports the following two operations:

* `void addNum(int num)` - Add a integer number from the data stream to the data structure.
* `double findMedian()` - Return the median of all elements so far.

__Example:__
```
addNum(1)
addNum(2)
findMedian() -> 1.5
addNum(3) 
findMedian() -> 2
```

__Follow up:__
1. If all integer numbers from the stream are between 0 and 100, how would you optimize it?
2. If 99% of all integer numbers from the stream are between 0 and 100, how would you optimize it?

### Solution
__Priority Queue:__
```Swift
public protocol QueueOperations {
    associatedtype Element
    var isEmpty : Bool { get }
    var count : Int { get }
    func peek() -> Element?
    mutating func enqueue(_ element: Element)
    mutating func dequeue() -> Element?
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
```

```Swift
class MedianFinder {
    
    // Use maxHeap to track the smaller set of numbers,
    // and minHeap to track the larger set of numbers in the data stream
    private var maxHeap : PriorityQueue<Int>
    private var minHeap : PriorityQueue<Int>
    
    /** initialize your data structure here. */
    init() {
        maxHeap = PriorityQueue<Int>(comparator: >)
        minHeap = PriorityQueue<Int>(comparator: <)
    }
    
    func addNum(_ num: Int) {
        
        // O(log(n)) time
        // Insert num into the set it belongs
        if let max = maxHeap.peek(), num > max {
            minHeap.enqueue(num)
        } else {
            maxHeap.enqueue(num)
        }
        
        // O(2*log(n)) time
        // Balance the trees to ensure that maxHeap has no more than 1 elements
        // than the minHeap
        if minHeap.count > maxHeap.count {
            maxHeap.enqueue(minHeap.dequeue()!)
        } else if maxHeap.count - minHeap.count == 2 {
            minHeap.enqueue(maxHeap.dequeue()!)
        }
    }
    
    func findMedian() -> Double {

        // O(1) time
        // If the total number of elements is even, the median
        // is the average of the largest element from the smaller set
        // and the smallest element from the larger set.
        // Otherwise the median is the largest element from the smaller set.
        if (maxHeap.count+minHeap.count)%2 == 0 {
            return (Double(maxHeap.peek()!) + Double(minHeap.peek()!))/2
        } else {
            return Double(maxHeap.peek()!)
        }
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * let obj = MedianFinder()
 * obj.addNum(num)
 * let ret_2: Double = obj.findMedian()
 */
```