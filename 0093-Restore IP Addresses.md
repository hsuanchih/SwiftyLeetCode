
### Restore IP Addresses

Given a string containing only digits, restore it by returning all possible valid IP address combinations.

__Example:__
```
Input: "25525511135"
Output: ["255.255.11.135", "255.255.111.35"]
```

### Solution
```Swift
class Solution {
    func restoreIpAddresses(_ s: String) -> [String] {
        var ranges : [Range<Int>] = [], result : [String] = []
        restore(Array(s), &ranges, &result)
        return result
    }
    
    func restore(_ s: [Character], _ ranges: inout [Range<Int>], _ result: inout [String]) {
        let index = ranges.last?.upperBound ?? 0
        if index > s.count {
            return
        }
        switch ranges.count {
            case 3:
            let last = String(s[index..<s.count])
            if isValid(last) {
                var temp = ranges.reduce("") {
                    let string = String(s[$1])
                    if $0.isEmpty {
                        return string
                    } else {
                        return $0 + ".\(string)"
                    }
                }
                temp.append(".\(last)")
                result.append(temp)
            }
            default:
            for i in 1..<4 {
                if index+i > s.count {
                    break
                }
                let range = index..<index+i
                if isValid(String(s[range])) {
                    ranges.append(range)
                    restore(s, &ranges, &result)
                    ranges.removeLast()
                }
            }
        }
    }
    
    func isValid(_ input: String) -> Bool {
        guard let num = Int(input) else {
            return false
        }
        if num == 0 && input.count > 1 {
            return false
        }
        if input[input.startIndex] == "0" && num != 0 {
            return false
        }
        return num >= 0 && num <= 255
    }
}
```