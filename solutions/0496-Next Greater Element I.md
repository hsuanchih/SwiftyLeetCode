
### Next Greater Element I

You are given two arrays __(without duplicates)__ `nums1` and `nums2` where `nums1`’s elements are subset of `nums2`.</br>
Find all the next greater numbers for `nums1`'s elements in the corresponding places of `nums2`.

The Next Greater Number of a number x in nums1 is the first greater number to its right in `nums2`.</br>
If it does not exist, output -1 for this number.

__Example 1:__
```
Input: nums1 = [4,1,2], nums2 = [1,3,4,2].
Output: [-1,3,-1]
Explanation:
    For number 4 in the first array, you cannot find the next greater number for it in the second array, so output -1.
    For number 1 in the first array, the next greater number for it in the second array is 3.
    For number 2 in the first array, there is no next greater number for it in the second array, so output -1.
```
__Example 2:__
```
Input: nums1 = [2,4], nums2 = [1,2,3,4].
Output: [3,-1]
Explanation:
    For number 2 in the first array, the next greater number for it in the second array is 3.
    For number 4 in the first array, there is no next greater number for it in the second array, so output -1.
```
__Note:__
1. All elements in `nums1` and `nums2` are unique.
2. The length of both `nums1` and `nums2` would not exceed 1000.

### Solution
__O(nums1*nums2) Time, O(1) Space - Brute-Force:__
```Swift
class Solution {
    func nextGreaterElement(_ nums1: [Int], _ nums2: [Int]) -> [Int] {
        var result = Array(repeating: -1, count: nums1.count)

        // For each element in nums1
        for i in 0..<nums1.count {

            // First find the index in nums1 with element corresponding 
            // to the same value in nums2 (target), then find the next value 
            // in nums2 that's greater than such value (nextGreater)
            var target, nextGreater : Int?
            for j in 0..<nums2.count {
                if nums2[j] == nums1[i] {
                    target = j
                    continue
                }
                if let target = target, nums2[j] > nums2[target] {
                    result[i] = nums2[j]
                    break
                }
            }
        }
        return result
    }
}
```
__O(nums1*nums2/2) Time, O(nums2) Space - Improved Brute-Force:__
```Swift
class Solution {
    func nextGreaterElement(_ nums1: [Int], _ nums2: [Int]) -> [Int] {
        var result = Array(repeating: -1, count: nums1.count)

        // Store values of nums2 with respect to their indices
        var lookup : [Int: Int] = [:]
        for (index, value) in nums2.enumerated() {
            lookup[value] = index
        }

        // Lookup in nums2 the index of value corresponding to nums1
        // Search in nums2 starting from such index to find the next greater value
        for (index, value) in nums1.enumerated() {
            if let target = lookup[value] {
                for j in target..<nums2.count {
                    if nums2[j] > value {
                        result[index] = nums2[j]
                        break
                    }
                }
            }
        }
        return result
    }
}
```
__O(nums1+nums2) Time, O(nums2) Space - Stack:__
```Swift
class Solution {
    func nextGreaterElement(_ nums1: [Int], _ nums2: [Int]) -> [Int] {
        var stack : [Int] = [], nextGreater : [Int: Int] = [:]

        for i in stride(from: nums2.count-1, through: 0, by: -1) {
            let num = nums2[i]

            // Remove all elements on the stack smaller than num
            while let last = stack.last, last < num {
                _ = stack.removeLast()
            }

            // If stack is not empty, the next element in nums2 greater 
            // than num is on the top of the stack, otherwise there are no
            // next elements greater than num (-1)
            // Insert the next greater element corresponding to num into the lookup
            if let last = stack.last {
                nextGreater[num] = last
            } else {
                nextGreater[num] = -1
            }

            // Push num onto the stack
            stack.append(num)
        }

        // For each element in nums1, lookup its next greater element in nums2
        return nums1.map { nextGreater[$0]! }
    }
}
```