
### Min Stack

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

* push(x) -- Push element x onto stack.
* pop() -- Removes the element on top of the stack.
* top() -- Get the top element.
* getMin() -- Retrieve the minimum element in the stack.

__Example:__
```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> Returns -3.
minStack.pop();
minStack.top();      --> Returns 0.
minStack.getMin();   --> Returns -2.
```

### Solution
```Swift
class MinStack {
    
    private struct Element<T> {
        let val : T
        let min : T
    }
    private var stack : [Element<Int>]

    /** initialize your data structure here. */
    init() {
        stack = []
    }
    
    // O(1)
    func push(_ x: Int) {
        var element : Element<Int>!
        defer {
            stack.append(element)
        }
        if let last = stack.last, last.min < x {
            element = Element(val: x, min: last.min)
        } else {
            element = Element(val: x, min: x)
        }
    }
    
    // O(1)
    func pop() {
        guard !stack.isEmpty else { return }
        _ = stack.removeLast()
    }
    
    // O(1)
    func top() -> Int {
        return stack.last!.val
    }
    
    // O(1)
    func getMin() -> Int {
        return stack.last!.min
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * let obj = MinStack()
 * obj.push(x)
 * obj.pop()
 * let ret_3: Int = obj.top()
 * let ret_4: Int = obj.getMin()
 */
```