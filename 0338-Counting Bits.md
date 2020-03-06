
### Counting Bits

Given a non negative integer number __num__.</br> 
For every numbers __i__ in the range __0 ≤ i ≤ num__</br> 
calculate the number of 1's in their binary representation and return them as an array.

__Example 1:__
```
Input: 2
Output: [0,1,1]
```
__Example 2:__
```
Input: 5
Output: [0,1,1,2,1,2]
```

__Follow up:__

* It is very easy to come up with a solution with run time __O(n\*sizeof(integer))__. But can you do it in linear time __O(n)__ /possibly in a single pass?
* Space complexity should be __O(n)__.
* Can you do it like a boss? Do it without using any builtin function like __builtin_popcount__ in c++ or in any other language.

### Solution
__O(num*32):__
```Swift
class Solution {
    func countBits(_ num: Int) -> [Int] {
        var result = Array(repeating: 0, count: num+1)
        for i in 0...num {
            for j in 0..<32 {
                result[i] += (i>>j) & 1
            }
        }
        return result
    }
}
```
__O(num):__
```Swift
class Solution {
    func countBits(_ num: Int) -> [Int] {
        var result = Array(repeating: 0, count: num+1)
        for i in 0...num {
            result[i] = result[i/2] + i%2
        }
        return result
    }
}
```