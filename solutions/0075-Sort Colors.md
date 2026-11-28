
### Sort Colors

Given an array with *n* objects colored red, white or blue, sort them __in-place__</br>
so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

__Note:__ You are not suppose to use the library's sort function for this problem.

__Example:__
```
Input: [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```
__Follow up:__

* A rather straight forward solution is a two-pass algorithm using counting sort. First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.
* Could you come up with a one-pass algorithm using only constant space?

### Solution
__O(nums) Time, O(1) Space - Counting Sort, 2 Pass:__
```Swift
class Solution {
    func sortColors(_ nums: inout [Int]) {
        var lookup: [Int] = Array(repeating: 0, count: 3)
        for num in nums {
            lookup[num] += 1
        }

        var i: Int = 0
        for index in 0 ..< lookup.count {
            for _ in 0 ..< lookup[index] {
                nums[i] = index
                i += 1
            }
        }
    }
}
```
__O(nums) Time, O(1) Space - 2 Pointers, 1 Pass:__
```Swift
class Solution {
    func sortColors(_ nums: inout [Int]) {
        var zero = 0, two = nums.count-1, index = 0
        while index < nums.count {
            if index > zero && nums[index] == 0 {
                nums.swapAt(index, zero)
                zero+=1
                continue
            }
            if index < two && nums[index] == 2 {
                nums.swapAt(index, two)
                two-=1
                continue
            }
            index+=1
        }
    }
}
```