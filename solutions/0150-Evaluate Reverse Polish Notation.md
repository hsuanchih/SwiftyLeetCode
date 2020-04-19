
### Evaluate Reverse Polish Notation

Evaluate the value of an arithmetic expression in Reverse Polish Notation.

Valid operators are `+`, `-`, `*`, `/`. Each operand may be an integer or another expression.

__Note:__
* Division between two integers should truncate toward zero.
* The given RPN expression is always valid. That means the expression would always evaluate to a result and there won't be any divide by zero operation.

__Example 1:__
```
Input: ["2", "1", "+", "3", "*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9
```
__Example 2:__
```
Input: ["4", "13", "5", "/", "+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6
```
__Example 3:__
```
Input: ["10", "6", "9", "3", "+", "-11", "*", "/", "*", "17", "+", "5", "+"]
Output: 22
Explanation: 
  ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
```

### Solution
__O(tokens) Time:__
```Swift
class Solution {
    
    // Define valid operators
    let operators = Set(["+", "-", "*", "/"])
    
    func evalRPN(_ tokens: [String]) -> Int {
        var stack : [Int] = []
        for string in tokens {

            // If token is an operator, pop the top two values from the stack as right & left operands 
            // and run computation using the operator; push the result of computation onto the stack
            // Otherwise convert token into its Int representation & push the value onto the stack
            if isOperator(string) {
                let right = stack.removeLast(), left = stack.removeLast()
                stack.append(compute(left, right, string))
            } else {
                stack.append(Int(string)!)
            }
        }
        return stack.last!
    }
    
    // Helper method to check if a token is an operator
    func isOperator(_ string: String) -> Bool {
        return operators.contains(string)
    }
    
    // Helper method to perform arithmetic operation given left & right operands and the operator
    func compute(_ left: Int, _ right: Int, _ operator: String) -> Int {
        switch `operator` {
            case "+": return left+right
            case "-": return left-right
            case "*": return left*right
            case "/": return left/right
            default: fatalError()
        }
    }
}
```