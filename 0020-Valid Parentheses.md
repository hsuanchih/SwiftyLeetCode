
### Valid Parentheses

Given a string containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:</br>
1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
Note that an empty string is also considered valid.

__Example 1:__
```
Input: "()"
Output: true
```
__Example 2:__
```
Input: "()[]{}"
Output: true
```
__Example 3:__
```
Input: "(]"
Output: false
```
__Example 4:__
```
Input: "([)]"
Output: false
```
__Example 5:__
```
Input: "{[]}"
Output: true
```

### Solution
__O(3^m*4^n):__
```Swift
class Solution {
    func isValid(_ s: String) -> Bool {
        let paren : [Character: Character] = [")": "(", "}": "{", "]": "["]
        var stack : [Character] = []

        for char in s {
            if let curr = paren[char] {
                
                // If curr is a close parenthesis, check that the top of the stack
                // is its open counterpart, otherwise there is a mismatch - return false
                if let last = stack.last, last == curr {
                    _ = stack.removeLast()
                } else {
                    return false
                }
            } else {
                
                // If curr is an open parenthesis, push it onto the stack
                stack.append(char)
            }
        }
        
        // Parentheses are balanced only if stack is empty
        return stack.isEmpty
    }
}
```
