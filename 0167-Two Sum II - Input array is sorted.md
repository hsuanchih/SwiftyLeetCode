
### Two Sum II - Input array is sorted

Given an array of integers that is already __sorted in ascending order__, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2.

__Note:__
* Your returned answers (both index1 and index2) are not zero-based.
* You may assume that each input would have exactly one solution and you may not use the same element twice.
__Example:__
```
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore index1 = 1, index2 = 2.
```

### Solution
__O(n):__
```Swift
class Solution {
    func twoSum(_ numbers: [Int], _ target: Int) -> [Int] {
        var start = 0, end = numbers.count-1
        while start < end {
            switch numbers[start]+numbers[end] {
                case target:
                return [start+1, end+1]
                case Int.min..<target:
                start+=1
                default:
                end-=1
            } 
        }
        fatalError()
    }
}
```