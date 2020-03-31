
### Keyboard Row

Given a List of words, return the words that can be typed using letters of __alphabet__ on only one row's of American keyboard like the image below.

![Example](images/question_500.png)

__Example:__
```
Input: ["Hello", "Alaska", "Dad", "Peace"]
Output: ["Alaska", "Dad"]
```

__Note:__
1. You may use one character in the keyboard more than once.
2. You may assume the input string will only contain letters of alphabet.

### Solution
__O(m*n) Time, O(1) Space:__
```Swift
class Solution {
    func nextGreaterElement(_ nums1: [Int], _ nums2: [Int]) -> [Int] {
        var nums1 = nums1

        // For each element in nums1
        for index1 in stride(from: 0, to: nums1.count, by: 1) {

            // First find the corresponding value in nums2,
            // then find the next value in nums2 that's greater than such value
            var found = false
            for index2 in stride(from: 0, to: nums2.count, by: 1) {
                if found && nums2[index2] > nums1[index1] {
                    nums1[index1] = nums2[index2]
                    break
                }
                if nums2[index2] == nums1[index1] {
                    found = true
                }

                // If no candidate is found, set -1
                if index2 == nums2.count-1 {
                    nums1[index1] = -1
                }
            }
        }
        return nums1
    }
}
```
__O(m*n/2) Time, O(n) Space:__
```Swift
class Solution {
    func nextGreaterElement(_ nums1: [Int], _ nums2: [Int]) -> [Int] {

        // Store values of nums2 with respect to their indices
        var lookup : [Int: Int] = [:]
        for (index, value) in nums2.enumerated() {
            lookup[value] = index
        }

        // Lookup in nums2 the index of value corresponding to nums1
        // Search in nums2 starting from such index to find the next greater value
        var nums1 = nums1
        for i in stride(from: 0, to: nums1.count, by: 1) {
            let curr = nums1[i]
            var j = lookup[curr]!
            while j < nums2.count {
                if nums2[j] > curr {
                    nums1[i] = nums2[j]
                    break
                }
                j+=1
            }

            // If no candidate is found, set -1
            if nums1[i] == curr {
                nums1[i] = -1
            }
        }
        return nums1
    }
}
```
__O(m+n) Time, O(n) Space:__
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