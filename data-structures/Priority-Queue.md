# Priority Queue

```swift
import Foundation

/**
 The `QueueOperations` ADT supports the following operations:
    - Checking if a queue is empty
    - Enqueuing to the queue
    - Dequeuing from the queue
 */
public protocol QueueOperations {
    associatedtype Element
    var isEmpty: Bool { get }
    mutating func enqueue(_ element: Element)
    mutating func dequeue() -> Element?
}

/**
 The `PriorityQueue` data type implements a priority queue, and internally uses an array & elements swaps to emulate heapify and to adhere to the invariants of a binary heap. Allowing for:
    - Linear time heap construction from scratch
    - O(log(n)) time element enqueue
    - O(log(n)) time element dequeue
 */
public struct PriorityQueue<Element> {
    private var elements: [Element]
    private let comparator: (Element, Element) -> Bool

    public init(elements: [Element] = [], comparator: @escaping (Element, Element) -> Bool) {
        self.elements = elements
        self.comparator = comparator
        buildHeap()
    }
}

extension PriorityQueue: QueueOperations {
    public var isEmpty: Bool {
        elements.isEmpty
    }

    public mutating func enqueue(_ element: Element) {
        elements.append(element)
        heapifyUp(elements.count - 1)
    }

    public mutating func dequeue() -> Element? {
        defer { heapifyDown(0) }

        if isEmpty {
            return nil
        } else {
            elements.swapAt(0, elements.count - 1)
            return elements.removeLast()
        }
    }
}

extension PriorityQueue {
    private mutating func buildHeap() {
        stride(from: elements.count / 2 - 1, through: 0, by: -1).forEach { heapifyDown($0) }
    }

    private mutating func heapifyUp(_ index: Int) {
        guard index >= 0, index < elements.count, let parent = parent(of: index), comparator(elements[index], elements[parent]) else { return }
        elements.swapAt(index, parent)
        heapifyUp(parent)
    }

    private mutating func heapifyDown(_ index: Int) {
        guard index >= 0, index < elements.count else { return }
        let nextIndex: Int?
        switch (leftChild(of: index), rightChild(of: index)) {
        case (.some(let leftChild), .some(let rightChild)):
            switch (comparator(elements[index], elements[leftChild]), comparator(elements[index], elements[rightChild])) {
            case (false, false):
                nextIndex = comparator(elements[leftChild], elements[rightChild]) ? leftChild : rightChild
            case (true, false):
                nextIndex = rightChild
            case (false, true):
                nextIndex = leftChild
            case (true, true):
                nextIndex = nil
            }
        case (.some(let leftChild), .none):
            nextIndex = comparator(elements[leftChild], elements[index]) ? leftChild : nil
        case (.none, .some(let rightChild)):
            nextIndex = comparator(elements[rightChild], elements[index]) ? rightChild : nil
        case (.none, .none):
            nextIndex = nil
        }

        guard let nextIndex else { return }
        elements.swapAt(index, nextIndex)
        heapifyDown(nextIndex)
    }

    private func parent(of index: Int) -> Int? {
        index == 0 ? nil : (index - 1) / 2
    }

    private func leftChild(of index: Int) -> Int? {
        let child = index * 2 + 1
        return child < elements.count ? child : nil
    }

    private func rightChild(of index: Int) -> Int? {
        let child = (index + 1) * 2
        return child < elements.count ? child : nil
    }
}
```