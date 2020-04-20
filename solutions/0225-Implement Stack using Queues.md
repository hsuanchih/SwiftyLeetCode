
### Implement Stack using Queues

Implement the following operations of a stack using queues.

* `push(x)` -- Push element x onto stack.
* `pop()` -- Removes the element on top of the stack.
* `top()` -- Get the top element.
* `empty()` -- Return whether the stack is empty.

__Example:__
```
MyStack stack = new MyStack();

stack.push(1);
stack.push(2);  
stack.top();   // returns 2
stack.pop();   // returns 2
stack.empty(); // returns false
```

__Notes:__
* You must use *only* standard operations of a queue -- which means only `push to back`, `peek/pop from front`, `size`, and `is empty` operations are valid.
* Depending on your language, queue may not be supported natively. You may simulate a queue by using a list or deque (double-ended queue), as long as you use only standard operations of a queue.
* You may assume that all operations are valid (for example, no pop or top operations will be called on an empty stack).

### Solution
```Swift
class Queue<Element> {
    private class ListNode<Element> {
        var next : ListNode<Element>?
        var val : Element
        init(_ val: Element) {
            self.val = val
        }
    }
    private var head : ListNode<Element>?, tail : ListNode<Element>?
    
    public init() { }
    public func enQueue(_ element: Element) {
        defer { count+=1 }
        let node = ListNode(element)
        guard let _ = tail else {
            head = node
            tail = node
            return
        }
        tail!.next = node
        tail = node
    }
    public func deQueue() -> Element {
        defer { count-=1 }
        guard let first = first else {
            fatalError()
        }
        head = head?.next
        return first
    }
    public var first : Element? {
        return head?.val
    }
    public var isEmpty : Bool {
        return first == nil
    }
}

class MyStack {
    
    private var q1 : Queue<Int>, q2 : Queue<Int>
    private var _top_ : Int?

    /** Initialize your data structure here. */
    init() {
        q1 = Queue<Int>()
        q2 = Queue<Int>()
    }
    
    // O(1)
    /** Push element x onto stack. */
    func push(_ x: Int) {
        q1.enQueue(x)
        _top_ = x
    }
    
    // O(n)
    /** Removes the element on top of the stack and returns that element. */
    func pop() -> Int {
        defer {
            q1 = q2
            q2 = Queue<Int>()
        }
        _top_ = nil
        var val : Int?
        while let first = q1.first {
            val = q1.deQueue()
            if let first = q1.first {
                _top_ = val
                q2.enQueue(val!)
            }
        }
        return val!
    }
    
    // O(1)
    /** Get the top element. */
    func top() -> Int {
        return _top_!
    }
    
    // O(1)
    /** Returns whether the stack is empty. */
    func empty() -> Bool {
        return _top_ == nil
    }
}

/**
 * Your MyStack object will be instantiated and called as such:
 * let obj = MyStack()
 * obj.push(x)
 * let ret_2: Int = obj.pop()
 * let ret_3: Int = obj.top()
 * let ret_4: Bool = obj.empty()
 */
```