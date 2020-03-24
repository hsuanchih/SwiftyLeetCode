
### Gray Code

The gray code is a binary numeral system where two successive values differ in only one bit.

Given a non-negative integer *n* representing the total number of bits in the code,</br> 
print the sequence of gray code. A gray code sequence must begin with 0.

__Example 1:__
```
Input: 2
Output: [0,1,3,2]
Explanation:
00 - 0
01 - 1
11 - 3
10 - 2

For a given n, a gray code sequence may not be uniquely defined.
For example, [0,2,3,1] is also a valid gray code sequence.

00 - 0
10 - 2
11 - 3
01 - 1
```
__Example 2:__
```
Input: 0
Output: [0]
Explanation: We define the gray code sequence to begin with 0.
             A gray code sequence of n has size = 2n, which for n = 0 the size is 20 = 1.
             Therefore, for n = 0 the gray code sequence is [0].
```

### Solution
__O(n*2^n) Time, O(2^n) Space - String Manipulation Iterative:__
```Swift
class Solution {
    func grayCode(_ n: Int) -> [Int] {
        guard n >= 1 else { return [0] }
        var curr : [String] = []
        
        for bits in 1...n {
            
            // Base case: when # of bits is 1, the gray code sequence is ["0","1"]
            if bits == 1 {
                curr = ["0","1"]
                continue
            }
            
            // Append "0" to gray code sequence from (bits-1)
            // Append "1" to the reverse of gray code sequence from (bits-1)
            // Concatenate both sequences together will give the gray code sequence for (bits)
            curr = (curr.map { "0".appending($0) }) + (curr.reversed().map { "1".appending($0) })
        }
        return curr.map { Int($0, radix: 2)! }
    }
}
```
__O(n*2^n/2) Time, O(2^n) Space - Iterative:__
```Swift
class Solution {
    func grayCode(_ n: Int) -> [Int] {
        guard n >= 1 else { return [0] }
        var curr : [Int] = []
        for bits in 1...n {
            if bits == 1 {
                curr = [0,1]
                continue
            }
            curr += curr.reversed().map { 1 << (bits-1) | $0 } 
        }
        return curr
    }
}
```
__O(n*2^n/2) Time, O(2^n) Space - Recursive:__
```Swift
class Solution {
    func grayCode(_ n: Int) -> [Int] {
        switch n {
            case 0:
            return [0]
            case 1:
            return [0,1]
            default:
            let prev = grayCode(n-1)
            return prev + prev.reversed().map { 1 << (n-1) | $0 }
        }
    }
}
```