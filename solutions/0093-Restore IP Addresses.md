
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
        switch ranges.count {
            
            // We've accumulated 3 segments of the IP address
            // this will be our last
            case 3:
            // Validate the last segment of our IP address, 
            // if it is valid, we want to add this IP address
            // to result
            let range = index..<s.count
            if isValid(String(s[range])) {
                ranges.append(range)
                result.append(constructIPAddress(s, ranges))
                ranges.removeLast()
            }
            
            // We've not reached the last segment of the IP address
            // continue testing for each segment of the IP address
            default:
            
            // A valid IP address can be from 1 to 3 numbers long
            // validate each length, and for each of the valid segment,
            // use it to test the next segments
            for i in 1...3 where index+i <= s.count {
                let range = index..<index+i
                if isValid(String(s[range])) {
                    ranges.append(range)
                    restore(s, &ranges, &result)
                    ranges.removeLast()
                }
            }
        }
    }
    
    // Helper method to validate a given input
    func isValid(_ input: String) -> Bool {
        
        switch Int(input) {
            
            // If num == 0, the only valid representation is "0"
            // so "00" is invalid
            case .some(0):
            return input.count == 1
            
            // We cannot have a 0 prefix to a non-zero value
            // ie, "03" is invalid
            case .some(1...255):
            return input.first! != "0"
            
            // If the input cannot be converted to a number
            // or number > 255, the input is invalid 
            default:
            return false
        }
    }
    
    // Helper method to construct IP address given ranges
    func constructIPAddress(_ s: [Character], _ ranges: [Range<Int>]) -> String {
        var result = ""
        for i in 0..<ranges.count {
            result.append(String(s[ranges[i]]))
            if i == ranges.count - 1 {
                break
            }
            result.append(".")
        }
        return result
    }
}
```