
### Decode String

Given an encoded string, return its decoded string.

The encoding rule is: `k[encoded_string]`, where the *encoded_string* inside the square brackets is being repeated exactly *k* times.</br> 
Note that *k* is guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, *k*.</br> 
For example, there won't be input like `3a` or `2[4]`.

__Examples:__
```
s = "3[a]2[bc]", return "aaabcbc".
s = "3[a2[c]]", return "accaccacc".
s = "2[abc]3[cd]ef", return "abcabccdcdcdef".
```

### Solution
__O(s+sum(k\*length(encoded_string))):__
```Swift
extension Array where Element == Character {
    var intValue : Int {
        return Int(String(self))!
    }
}

class Solution {
    func decodeString(_ s: String) -> String {
        var stack : [Character] = []
        
        // Iterate through s & push each character onto the stack
        // until a "]" is met
        for char in s {
            switch char {
                case "]":
                
                // Keep popping from the stack until a "[" is found
                // construct letters from the segment between "[" - "]"
                // pair
                var temp : [Character] = []
                while let last = stack.last, last != "[" {
                    temp.append(stack.removeLast())
                }
                
                // Pop "[" from the stack
                stack.removeLast()
                
                // Keep popping from the stack as long as the character
                // is a number
                var num : [Character] = []
                while let last = stack.last, last.isNumber {
                    num.append(stack.removeLast())
                }
                
                // num reversed is the number of times temp should be repeated
                for _ in 0..<num.reversed().intValue {
                    
                    // Since temp stores the characters of s in reverse order,
                    // push characters in temp onto the stack in reverse order
                    // Do this n times
                    for i in stride(from: temp.count-1, through: 0, by: -1) {
                        stack.append(temp[i])
                    }
                }
                default:
                stack.append(char)
            }
        }
        return String(stack)
    }
}
```