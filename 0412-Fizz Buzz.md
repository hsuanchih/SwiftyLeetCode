
### Fizz Buzz

Write a program that outputs the string representation of numbers from 1 to *n*.

But for multiples of three it should output “Fizz” instead of the number and for the multiples of five output “Buzz”.</br> 
For numbers which are multiples of both three and five output “FizzBuzz”.

__Example:__
```
n = 15,

Return:
[
    "1",
    "2",
    "Fizz",
    "4",
    "Buzz",
    "Fizz",
    "7",
    "8",
    "Fizz",
    "Buzz",
    "11",
    "Fizz",
    "13",
    "14",
    "FizzBuzz"
]
```

### Solution
__Recursive Top-Down O(3*n):__
```Swift
class Solution {
    enum Buzzer : String, CaseIterable {
        case FizzBuzz, Buzz, Fizz
        var divisor : Int {
            switch self {
                case .FizzBuzz: return 15
                case .Buzz: return 5
                case .Fizz: return 3
            }
        }
    }
    
    func fizzBuzz(_ n: Int) -> [String] {
        if n == 1 {
            return [String(n)]
        }
        var prev = fizzBuzz(n-1)
        for buzzer in Buzzer.allCases {
            if n%buzzer.divisor == 0 {
                prev.append(buzzer.rawValue)
                return prev
            }
        }
        prev.append(String(n))
        return prev
    }
}
```
__Iterative Bottom-Up O(3*n):__
```Swift
class Solution {
    enum Buzzer : String, CaseIterable {
        case FizzBuzz, Buzz, Fizz
        var divisor : Int {
            switch self {
                case .FizzBuzz: return 15
                case .Buzz: return 5
                case .Fizz: return 3
            }
        }
    }
    
    func fizzBuzz(_ n: Int) -> [String] {
        var result : [String] = []
        for num in 1...n {
            var curr = ""
            for buzzer in Buzzer.allCases {
                if num%buzzer.divisor == 0 {
                    curr = buzzer.rawValue
                    break
                }
            }
            result.append(curr.isEmpty ? String(num) : curr)
        }
        return result
    }
}
```