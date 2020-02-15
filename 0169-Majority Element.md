
### Majority Element

Given an array of size *n*, find the majority element. The majority element is the element that appears __more than__ `⌊ n/2 ⌋` times.

You may assume that the array is non-empty and the majority element always exist in the array.

__Example 1:__
```
Input: [3,2,3]
Output: 3
```
__Example 2:__
```
Input: [2,2,1,1,1,2,2]
Output: 2
```

### Solution
__O(n) Time, O(n) Space:__
```Swift
class Solution {
    func majorityElement(_ nums: [Int]) -> Int {
        var map : [Int: Int] = [:]
        for num in nums {
            map[num, default: 0]+=1
            if map[num]! > nums.count/2 {
                return num
            }
        }
        fatalError()
    }
}
```
__O(nlogn) Time, O(1) Space:__
```Swift
class Solution {
    func majorityElement(_ nums: [Int]) -> Int {
        return nums.sorted()[nums.count/2]
    }
}
```