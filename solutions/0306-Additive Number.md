
### Additive Number

Additive number is a string whose digits can form additive sequence.

A valid additive sequence should contain __at least__ three numbers.</br> 
Except for the first two numbers, each subsequent number in the sequence must be the sum of the preceding two.

Given a string containing only digits `'0'-'9'`, write a function to determine if it's an additive number.

__Note:__ Numbers in the additive sequence __cannot__ have leading zeros, so sequence `1, 2, 03` or `1, 02, 3` is invalid.

__Example 1:__
```
Input: "112358"
Output: true
Explanation: The digits can form an additive sequence: 1, 1, 2, 3, 5, 8. 
             1 + 1 = 2, 1 + 2 = 3, 2 + 3 = 5, 3 + 5 = 8
```
__Example 2:__
```
Input: "199100199"
Output: true
Explanation: The additive sequence is: 1, 99, 100, 199. 
             1 + 99 = 100, 99 + 100 = 199
```

__Constraints:__
* num consists only of digits `'0'-'9'`.
* `1 <= num.length <= 35`

__Follow up:__
How would you handle overflow for very large input integers?

### Solution
```Swift
class Solution {
    func isAdditiveNumber(_ num: String) -> Bool {
        return isAdditive(Array(num), 0, 0, 0, 0)
    }
    
    func isAdditive(_ num: [Character], _ first: Int, _ second: Int, _ index: Int, _ seq: Int) -> Bool {
        
        // If we've reached the end of the input,
        // check if we have at least 3 numbers
        if index == num.count {
            return seq > 2
        }
        
        // If the current number starts with "0", the only valid
        // number here is 0, as we cannot have numbers with leading 0s 
        for i in index..<(num[index] == "0" ? index+1 : num.count) {
            
            // Check that the conversion from string to integer succeeds
            // This might not be the case for very long strings where
            // the number will exceed Int.max
            guard let curr = Int(String(num[index...i])) else {
                break
            }
            
            // Check the current sequence number
            // 0 : We compute the first number
            // 1 : We compute the second number
            // 2+: We validate the previous 2 numbers,
            //     and continue with the next sequence
            switch seq {
                case 0:
                if isAdditive(num, curr, second, i+1, seq+1) {
                    return true
                }
                case 1:
                if isAdditive(num, first, curr, i+1, seq+1) {
                    return true
                }
                default:
                if first+second == curr && isAdditive(num, second, curr, i+1, seq+1) {
                    return true
                }
            }
        }
        return false
    }
}
```