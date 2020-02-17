
### Longest Valid Parentheses

Given a string containing just the characters `'('` and `')'`, find the length of the longest valid (well-formed) parentheses substring.

__Example 1:__
```
Input: "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()"
```
__Example 2:__
```
Input: ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()"
```

### Solution
__O(n):__
```Swift
func longestValidParentheses(_ s: String) -> Int {
        var stack : [Int] = [-1], result = 0
        let input = Array(s)
        for i in 0..<input.count {
            switch (input[i], stack.last) {
                case (")", .some(let last)) where last >= 0 && input[last] == "(":
                stack.removeLast()
                if let peek = stack.last {
                    result = max(result, i-peek)
                }
                default:
                stack.append(i)
            }
        }
        return result
    }
```