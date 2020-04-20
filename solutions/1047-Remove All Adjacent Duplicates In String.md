
### Remove All Adjacent Duplicates In String

Given a string `S` of lowercase letters, a *duplicate removal* consists of choosing two adjacent and equal letters, and removing them.

We repeatedly make duplicate removals on S until we no longer can.

Return the final string after all such duplicate removals have been made.  It is guaranteed the answer is unique.

__Example 1:__
```
Input: "abbaca"
Output: "ca"
Explanation: 
For example, in "abbaca" we could remove "bb" since the letters are adjacent and equal, and this is the only possible move.  The result of this move is that the string is "aaca", of which only "aa" is possible, so the final string is "ca".
```

__Note:__
1. `1 <= S.length <= 20000`
2. `S` consists only of English lowercase letters.

### Solution
__O(S) Time, O(S) Space:__
```Swift
class Solution {
    func removeDuplicates(_ S: String) -> String {

        // User stack to keep track of non-repeating characters in S
        var stack : [Character] = []
        
        // Iterate through S
        for char in S {

            // If the top of stack matches the current character, we see a duplicate in S (pop from stack)
            // Otherwise push current character onto the stack
            if let last = stack.last, last == char {
                _ = stack.removeLast()
            } else {
                stack.append(char)
            }
        }

        // Generate result from stack
        return String(stack)
    }
}
```