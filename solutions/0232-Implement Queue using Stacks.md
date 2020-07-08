
### Implement Queue using Stacks

Implement the following operations of a queue using stacks.

* __push(x)__ -- Push element x to the back of queue.
* __pop()__ -- Removes the element from in front of queue.
* __peek()__ -- Get the front element.
* __empty()__ -- Return whether the queue is empty.

__Example:__
```
MyQueue queue = new MyQueue();

queue.push(1);
queue.push(2);  
queue.peek();  // returns 1
queue.pop();   // returns 1
queue.empty(); // returns false
```

__Notes:__
* You must use only standard operations of a stack -- which means only `push to top`,  `peek/pop from top`, `size`, and `is empty` operations are valid.
* Depending on your language, stack may not be supported natively. You may simulate a stack by using a list or deque (double-ended queue), as long as you use only standard operations of a stack.
* You may assume that all operations are valid (for example, no pop or peek operations will be called on an empty queue).

### Solution
```Swift
class MyQueue {

    /** Initialize your data structure here. */
    // O(n) Space
    var s1 : [Int], s2 : [Int]
    init() {
        s1 = []
        s2 = []
    }
    
    // O(n) Time
    /** Push element x to the back of queue. */
    func push(_ x: Int) {
        while !s1.isEmpty {
            s2.append(s1.removeLast())
        }
        s2.append(x)
        while !s2.isEmpty {
            s1.append(s2.removeLast())
        }
    }
    
    // O(1) Time
    /** Removes the element from in front of queue and returns that element. */
    func pop() -> Int {
        return s1.removeLast()
    }
    
    // O(1) Time
    /** Get the front element. */
    func peek() -> Int {
        return s1.last!
    }
    
    // O(1) Time
    /** Returns whether the queue is empty. */
    func empty() -> Bool {
        return s1.isEmpty
    }
}

/**
 * Your MyQueue object will be instantiated and called as such:
 * let obj = MyQueue()
 * obj.push(x)
 * let ret_2: Int = obj.pop()
 * let ret_3: Int = obj.peek()
 * let ret_4: Bool = obj.empty()
 */
```