
### Different Ways to Add Parentheses

Given a string of numbers and operators, return all possible results from computing all the different possible ways</br> 
to group numbers and operators. The valid operators are `+`, `-` and `*`.

__Example 1:__
```
Input: "2-1-1"
Output: [0, 2]
Explanation: 
((2-1)-1) = 0 
(2-(1-1)) = 2
```
__Example 2:__
```
Input: "2*3-4*5"
Output: [-34, -14, -10, -10, 10]
Explanation: 
(2*(3-(4*5))) = -34 
((2*3)-(4*5)) = -14 
((2*(3-4))*5) = -10 
(2*((3-4)*5)) = -10 
(((2*3)-4)*5) = 10
```

### Solution
```Swift
extension Array where Element == Character {
    func intValue(_ start: Int, _ end: Int) -> Int {
        return Int(String(self[start...end]))!
    }
}

class Solution {
    func diffWaysToCompute(_ input: String) -> [Int] {
        return solve(Array(input), 0, input.count-1)
    }
    
    // Exhaust all possible ways to process left & right operands of an operation
    func solve(_ input: [Character], _ start: Int, _ end: Int) -> [Int] {
        var result : [Int] = []

        // Iterate from start to end
        for i in start...end {
            
            // If character is an operator, we can recursively decompose everything to
            // the left & right of the operator again as sub operations
            if !input[i].isNumber {
                let left = solve(input, start, i-1), right = solve(input, i+1, end)

                // For all combination of results from each (left & right) sub operation, construct 
                // every possible outcome using the operator at index i
                for lNum in left {
                    for rNum in right {
                        result.append(compute(lNum, rNum, input[i]))
                    }
                }
            }
        }

        // If result is empty, then we have not found an operator at this sub level
        // thus input[start...end] must be a single number, return the number
        if result.isEmpty {
            result.append(input.intValue(start, end))
        }
        return result
    }
    
    // Helper method to compute an arithmetic operations given left & right operands and the operator
    func compute(_ left: Int, _ right: Int, _ operator: Character) -> Int {
        switch `operator` {
            case "+":
            return left+right
            case "-":
            return left-right
            case "*":
            return left*right
            default:
            fatalError("Invalid Operator")
        }
    }
}
```