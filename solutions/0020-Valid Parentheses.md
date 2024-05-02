
### Valid Parentheses

Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:
1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.
 
__Example 1:__
```
Input: s = "()"
Output: true
```
__Example 2:__
```
Input: s = "()[]{}"
Output: true
```
__Example 3:__
```
Input: s = "(]"
Output: false
```

__Constraints:__
* `1 <= s.length <= pow(10, 4)`
* `s` consists of parentheses only `'()[]{}'`.

### Solution
__O(s) Time, O(s) Space:__
```Swift
class Solution {
    func isValid(_ s: String) -> Bool {
        let paren: [Character: Character] = [")": "(", "}": "{", "]": "["]
        var stack: [Character] = []

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
__O(s) Time, O(1) Space:__
```Swift
class Solution {
    func isValid(_ s: String) -> Bool {
        var stack: [Character] = []
        for char in s {
            switch char {
            case "(", "{", "[":
                stack.append(char)
            case ")" where stack.isEmpty || stack.last! != "(",
                 "}" where stack.isEmpty || stack.last! != "{",
                 "]" where stack.isEmpty || stack.last! != "[":
                return false
            case ")", "}", "]":
                _ = stack.removeLast()
            default:
                fatalError()
            }
        }
        return stack.isEmpty
    }
}
```