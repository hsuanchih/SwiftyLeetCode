
### Single Number

Given a __non-empty__ array of integers, every element appears twice except for one. Find that single one.

__Note:__
Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

__Example 1:__
```
Input: [2,2,1]
Output: 1
```
__Example 2:__
```
Input: [4,1,2,1,2]
Output: 4
```

### Solution
__O(n) Time, O(n) Space:__
```Swift
class Solution {
    func singleNumber(_ nums: [Int]) -> Int {
        var set : Set<Int> = []
        nums.forEach {
            if set.contains($0) {
                set.remove($0)
            } else {
                set.insert($0)
            }
        }
        return set.first ?? -1
    }
}
```
__O(n) Time, O(1) Space:__
```Swift
class Solution {
    func singleNumber(_ nums: [Int]) -> Int {
        return nums.reduce(0) { $0 ^ $1 }
    }
}
```