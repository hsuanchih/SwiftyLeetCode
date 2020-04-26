
### Merge k Sorted Lists

Merge *k* sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

__Example:__
```
Input:
[
  1->4->5,
  1->3->4,
  2->6
]
Output: 1->1->2->3->4->4->5->6
```

### Solution
__O(k^2*n) Time, O(1) Space - Brute-Force:__
```Swift
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public var val: Int
 *     public var next: ListNode?
 *     public init(_ val: Int) {
 *         self.val = val
 *         self.next = nil
 *     }
 * }
 */
class Solution {
    func mergeKLists(_ lists: [ListNode?]) -> ListNode? {
        var lists = lists.compactMap { $0 }, result = ListNode(0), curr : ListNode? = result
        while !lists.isEmpty {
            var minIndex = 0
            for index in stride(from: 1, to: lists.count, by: 1) {
                if lists[index].val < lists[minIndex].val {
                    minIndex = index
                }
            }
            let node = lists[minIndex]
            if let next = node.next {
                lists[minIndex] = next
            } else {
                lists.remove(at: minIndex)
            }
            curr?.next = node
            curr = curr?.next
        }
        return result.next
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
__O((n\*k)\*log\[base 2\](n\*k) + (n\*k)\*log\[base 2\](n\*k)) Time, O(n\*k) Space:__
```Swift
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public var val: Int
 *     public var next: ListNode?
 *     public init(_ val: Int) {
 *         self.val = val
 *         self.next = nil
 *     }
 * }
 */
class Solution {
    func mergeKLists(_ lists: [ListNode?]) -> ListNode? {
        var minHeap = PriorityQueue<ListNode>(comparator: { $0.val < $1.val })
        for list in lists {
            var iter = list
            while let curr = iter {
                iter = curr.next
                minHeap.enqueue(curr)
                curr.next = nil
            }
        }
        let result = ListNode(0)
        var iter : ListNode? = result
        while !minHeap.isEmpty {
            iter?.next = minHeap.dequeue()
            iter = iter?.next
        }
        return result.next
    }
}
```
__O((n\*k)\*2\*log(k)) Time, O(k) Space:__
```Swift
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public var val: Int
 *     public var next: ListNode?
 *     public init(_ val: Int) {
 *         self.val = val
 *         self.next = nil
 *     }
 * }
 */
class Solution {
    func mergeKLists(_ lists: [ListNode?]) -> ListNode? {
        var minHeap = PriorityQueue<ListNode>(comparator: { $0.val < $1.val })
        for list in lists {
            if let node = list {
                minHeap.enqueue(node)
            }
        }
        let result = ListNode(0)
        var iter : ListNode? = result
        while !minHeap.isEmpty {
            if let node = minHeap.dequeue() {
                iter?.next = node
                if let next = node.next {
                    minHeap.enqueue(next)
                }
                iter = iter?.next
            }
        }
        return result.next
    }
}
```