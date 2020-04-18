
### Remove K Digits

Given a non-negative integer *num* represented as a string,</br> 
remove *k* digits from the number so that the new number is the smallest possible.

__Note:__
* The length of *num* is less than 10002 and will be ≥ *k*.
* The given *num* does not contain any leading zero.

__Example 1:__
```
Input: num = "1432219", k = 3
Output: "1219"
Explanation: Remove the three digits 4, 3, and 2 to form the new number 1219 which is the smallest.
```
__Example 2:__
```
Input: num = "10200", k = 1
Output: "200"
Explanation: Remove the leading 1 and the number is 200. Note that the output must not contain leading zeroes.
```
__Example 3:__
```
Input: num = "10", k = 2
Output: "0"
Explanation: Remove all the digits from the number and it is left with nothing which is 0.
```

### Solution
__O(num) Time, O(num) Space:__
```Swift
class Solution {
    func removeKdigits(_ num: String, _ k: Int) -> String {
        var k = k, stack : [Character] = []
        
        // Iterate through each character in num
        for char in num {
            
            // Continue popping from the stack while stack is not empty
            // and the element on top of the stack is greater in value
            // than the current element while keeping track of k
            while k > 0, let last = stack.last, last > char {
                stack.removeLast()
                k-=1
            }
            
            // Take care not to push leading 0's onto the stack when the
            // stack is empty
            if !stack.isEmpty || char != "0" {
                stack.append(char)
            }
        }
        
        // If the length of increasing subsequence is greater than k
        // we need to start popping from the stack to ensure that
        // k elements are removed from the end
        while k > 0, let _ = stack.last {
            stack.removeLast()
            k-=1
        }
        
        return stack.isEmpty ? "0" : String(stack)
    }
}
```