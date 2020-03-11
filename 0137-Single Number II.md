
### Single Number II

Given a __non-empty__ array of integers, every element appears *three* times except for one,</br> 
which appears exactly once. Find that single one.

__Note:__

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

__Example 1:__
```
Input: [2,2,3,2]
Output: 3
```
__Example 2:__
```
Input: [0,1,0,1,0,1,99]
Output: 99
```

### Solution
__O(n^2) Time, O(1) Space:__
```Swift
class Solution {
    func singleNumber(_ nums: [Int]) -> Int {
        for i in 0..<nums.count {
            var count = 0
            for j in 0..<nums.count {
                if nums[i] == nums[j] {
                    if count == 2 {
                        break
                    }
                    count+=1
                }
                if j == nums.count-1 {
                    return nums[i]
                }
            }
        }
        fatalError()
    }
}
```
__O(n) Time, O(n/3) Space:__
```Swift
class Solution {
    func singleNumber(_ nums: [Int]) -> Int {
        var lookup : [Int: Int] = [:]
        for num in nums {
            lookup[num, default: 0] += 1
            if let count = lookup[num], count == 3 {
                lookup.removeValue(forKey: num)
            }
        }
        return lookup.first!.key
    }
}
```
__O(64\*n) Time, O(1) Space:__
```Swift
class Solution {
    func singleNumber(_ nums: [Int]) -> Int {
        var result = 0
        
        // Add each bit of every element in nums individually
        // If the sum of the i-th bit of all elements is divisible by 3
        // then the elements with i-th bit set will have appeared 3 times
        for shift in 0..<64 {
            var count = 0
            for num in nums {
                count += (num>>shift)&1
            }
            
            // Re-construct the single element that has its i-th bit set
            result |= (count%3)<<shift
        }
        return result
    }
}
```