
### Reverse String

Write a function that reverses a string. The input string is given as an array of characters `char[]`.

Do not allocate extra space for another array, you must do this by __modifying the input array in-place__ with O(1) extra memory.

You may assume all the characters consist of printable ascii characters.

__Example 1:__
```
Input: ["h","e","l","l","o"]
Output: ["o","l","l","e","h"]
```
__Example 2:__
```
Input: ["H","a","n","n","a","h"]
Output: ["h","a","n","n","a","H"]
```

### Solution
__O(s):__
```Swift
class Solution {
    func reverseString(_ s: inout [Character]) {
        var start = 0, end = s.count-1
        while start < end {
            s.swapAt(start, end)
            start+=1
            end-=1
        }
    }
}
```