
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
        switch (open, close) {
            case (0, 0):
            result.append(temp)
            case (0...Int.max, 0...Int.max):
            if close > open {
                generate(open, close-1, temp+")", &result)
            }
            generate(open-1, close, temp+"(", &result)
            default:
            break
        }
    }
}
```