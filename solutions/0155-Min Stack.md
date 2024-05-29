
### Min Stack

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the `MinStack` class:
* `MinStack()` initializes the stack object.
* `void push(int val)` pushes the element `val` onto the stack.
* `void pop()` removes the element on the top of the stack.
* `int top()` gets the top element of the stack.
* `int getMin()` retrieves the minimum element in the stack.

You must implement a solution with `O(1)` time complexity for each function.

__Example 1:__
```
Input
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

Output
[null,null,null,null,-3,null,0,-2]

Explanation
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin(); // return -3
minStack.pop();
minStack.top();    // return 0
minStack.getMin(); // return -2
```

__Constraints:__
* `-pow(2, 31) <= val <= pow(2, 31) - 1`
* Methods `pop`, `top` and `getMin` operations will always be called on __non-empty__ stacks.
* At most `3 * pow(10, 4)` calls will be made to `push`, `pop`, `top`, and `getMin`.

### Solution
```Swift
extension MinStack {
    struct Element {
        let val: Int
        let min: Int
    }
}

class MinStack {
    private var stack: [Element]
    private var last: Element {
        if let last = stack.last {
            return last
        } else {
            fatalError()
        }
    }

    init() {
        stack = []
    }
    
    func push(_ val: Int) {
        let element: Element
        if let last = stack.last, last.min < val {
            element = Element(val: val, min: last.min)
        } else {
            element = Element(val: val, min: val)
        }
        stack.append(element)
    }
    
    func pop() {
        if stack.isEmpty {
            fatalError()
        } else {
            _ = stack.removeLast()
        }
    }
    
    func top() -> Int {
        last.val
    }
    
    func getMin() -> Int {
        last.min
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * let obj = MinStack()
 * obj.push(val)
 * obj.pop()
 * let ret_3: Int = obj.top()
 * let ret_4: Int = obj.getMin()
 */
```