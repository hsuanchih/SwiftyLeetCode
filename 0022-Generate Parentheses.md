
### Swap Nodes in Pairs

Given *n* pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given *n* = 3, a solution set is:
```
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```

### Solution
__O(2^n):__
```Swift
class Solution {
    func generateParenthesis(_ n: Int) -> [String] {
        var result : [String] = []
        generate(n, n, "", &result)
        return result
    }
    
    func generate(_ open: Int, _ close: Int, _ temp: String, _ result: inout [String]) {
        
        // If there are no more close brackets, then we have a possible solution
        // assuming that open <= close 
        if close == 0 {
            result.append(temp)
            return
        }
        
        // If we haven't exhausted all open brackets, use another open bracket
        if open > 0 {
            generate(open-1, close, temp.appending("("), &result)
        }
        
        // We can't have more close brackets than open brackets at any point
        // Use a close bracket only if there's more of them than opens used 
        if close > open {
            generate(open, close-1, temp.appending(")"), &result)
        }
    }
}
```