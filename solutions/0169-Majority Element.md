
### Majority Element

Given an array `nums` of size `n`, return the majority element.

The majority element is the element that appears more than `⌊n / 2⌋` times. You may assume that the majority element always exists in the array.

__Example 1:__
```
Input: nums = [3,2,3]
Output: 3
```
__Example 2:__
```
Input: nums = [2,2,1,1,1,2,2]
Output: 2
```
 
__Constraints:__
* `n == nums.length`
* `1 <= n <= 5 * pow(10, 4)`
* `-pow(10, 9) <= nums[i] <= pow(10, 9)`

### Solution
__O(nums) Time, O(nums) Space - HashMap:__
```Swift
class Solution {
    func majorityElement(_ nums: [Int]) -> Int {
        var tally: [Int: Int] = [:]
        for num in nums {
            tally[num, default: 0] += 1
            if tally[num]! > nums.count / 2 {
                return num
            }
        }
        fatalError()
    }
}
```
__O(nums * log(nums)) Time, O(1) Space - Sorted:__
```Swift
class Solution {
    func majorityElement(_ nums: [Int]) -> Int {
        nums.sorted()[nums.count / 2]
    }
}
```
__O(n*2) Time, O(1) Space - Randomization:__
```Swift
class Solution {
    func majorityElement(_ nums: [Int]) -> Int {
        while true {
            let majority = nums[Int.random(in: 0..<nums.count)]
            var count = 0
            for num in nums {
                if num == majority {
                    count+=1
                }
                if count > nums.count/2 {
                    return majority
                }
            }
        }
        fatalError()
    }
}
```
__O(n*64) Time, O(1) Space - Bit Voting:__
```Swift
class Solution {
    func majorityElement(_ nums: [Int]) -> Int {
        var result = 0
        for shift in 0..<64 {
            var count = 0
            for num in nums {
                if num >> shift & 1 == 1 {
                    count+=1
                }
                if count > nums.count/2 {
                    result |= 1 << shift
                    break
                }
            }
        }
        return result
    }
}
```
__O(n) Time, O(1) Space - Boyer-Moore Voting:__
```Swift
class Solution {
    func majorityElement(_ nums: [Int]) -> Int {
        guard var majority = nums.first else { fatalError() }
        var count = 0
        for num in nums {
            if num == majority {
                count+=1
            } else {
                count-=1
            }
            if count <= 0 {
                majority = num
                count = 1
            }
        }
        return majority
    }
}
```