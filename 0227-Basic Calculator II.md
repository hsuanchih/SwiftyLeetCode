
### Basic Calculator II

Implement a basic calculator to evaluate a simple expression string.

The expression string contains only __non-negative__ integers, `+`, `-`, `*`, `/` operators and empty spaces` `.</br> 
The integer division should truncate toward zero.

__Example 1:__
```
Input: "3+2*2"
Output: 7
```
__Example 2:__
```
Input: " 3/2 "
Output: 1
```
__Example 3:__
```
Input: " 3+5 / 2 "
Output: 5
```

__Note:__
* You may assume that the given expression is always valid.
* __Do not__ use the `eval` built-in library function.

### Solution
__O(s) Time, O(s) Space:__
```Swift
extension Character {
    var isOperator : Bool {
        switch self {
            case "+", "-", "*", "/":
            return true
            default:
            return false
        }
    }
}

class Solution {
    
    var stack : [Int] = []
    
    func calculate(_ s: String) -> Int {
        let s = Array(s)
        var i = 0
        while i < s.count {
            switch s[i] {
                case let char where char.isNumber:
                let range = nextNumber(s, i)
                stack.append(Int(String(s[range]))!)
                i = range.endIndex
                case let char where char.isOperator:
                let range = nextNumber(s, i+1), num = Int(String(s[range]))!
                stack.append(process(char, num))
                i = range.endIndex
                default:
                i+=1
            }
        }
        return stack.reduce(0) { $0 + $1 }
    }
    
    func process(_ operator: Character, _ num: Int) -> Int {
        switch `operator` {
            case "+": return num
            case "-": return -num
            case "*": return stack.removeLast()*num
            case "/": return stack.removeLast()/num
            default: fatalError()
        }
    }
    
    func nextNumber(_ s: [Character], _ i: Int) -> Range<Int> {
        var i = i
        while i < s.count && !s[i].isNumber {
            i+=1
        }
        var j = i
        while j < s.count && s[j].isNumber {
            j+=1
        }
        return i..<j
    }
}
```