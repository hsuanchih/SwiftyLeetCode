
### Largest Divisible Subset

Given a set of distinct positive integers `nums`, return the largest subset `answer` such that every pair `(answer[i], answer[j])` of elements in this subset satisfies:

* `answer[i] % answer[j] == 0`, or
* `answer[j] % answer[i] == 0`

If there are multiple solutions, return any of them.

__Example 1:__
```
Input: nums = [1,2,3]
Output: [1,2]
Explanation: [1,3] is also accepted.
```
__Example 2:__
```
Input: nums = [1,2,4,8]
Output: [1,2,4,8]
```

__Constraints:__
* `1 <= nums.length <= 1000`
* `1 <= nums[i] <= 2 * pow(10, 9)`
* All the integers in `nums` are unique.

### Solution
__Brute-Force:__
```Swift
class Solution {
    func largestDivisibleSubset(_ nums: [Int]) -> [Int] {
        var subsets: [[Int]] = []
        return divisibleSubset(nums.sorted(), 0, &subsets)
    }

    func divisibleSubset(_ nums: [Int], _ index: Int, _ subsets: inout [[Int]]) -> [Int] {
        if index == nums.count {
            return subsets.reduce([Int]()) { $1.count > $0.count ? $1 : $0 }
        } else {
            for subset in subsets where subset.allSatisfy { nums[index] % $0 == 0 } {
                subsets.append(subset + [nums[index]])
            }
            subsets.append([nums[index]])
            return divisibleSubset(nums, index + 1, &subsets)
        }
    }
}
```
__Brute-Force Improved:__
```Swift
class Solution {
    func largestDivisibleSubset(_ nums: [Int]) -> [Int] {
        var subsets: [[Int]] = []
        return divisibleSubset(nums.sorted(), 0, &subsets)
    }

    func divisibleSubset(_ nums: [Int], _ index: Int, _ subsets: inout [[Int]]) -> [Int] {
        if index == nums.count {
            return subsets.reduce([Int]()) { $1.count > $0.count ? $1 : $0 }
        } else {
            for subset in subsets where subset.last.flatMap { nums[index] % $0 == 0 } == true {
                subsets.append(subset + [nums[index]])
            }
            subsets.append([nums[index]])
            return divisibleSubset(nums, index + 1, &subsets)
        }
    }
}
```
__O(pow(nums, 2)) Time, O(32 * nums) Space - Recursive + Memoization:__
```Swift
class Solution {
    func largestDivisibleSubset(_ nums: [Int]) -> [Int] {
        let nums: [Int] = nums.sorted()
        var divisibleSubsetIncludingIndex: [[Int]?] = Array(repeating: nil, count: nums.count)
        var result: [Int] = []
        for startingIndex in 0 ..< nums.count {
            let largestDivisibleSubset: [Int] = divisibleSubset(nums, startingIndex, &divisibleSubsetIncludingIndex)
            if largestDivisibleSubset.count > result.count {
                result = largestDivisibleSubset
            }
        }
        return result
    }

    func divisibleSubset(_ nums: [Int], _ index: Int, _ divisibleSubsetIncludingIndex: inout [[Int]?]) -> [Int] {
        if index == nums.count {
            return []
        } else if let result = divisibleSubsetIncludingIndex[index] {
            return result
        } else {
            var subset: [Int] = []
            for j in index + 1 ..< nums.count where nums[j] % nums[index] == 0 {
                let subsetIncludingJ: [Int] = divisibleSubset(nums, j, &divisibleSubsetIncludingIndex)
                if subsetIncludingJ.count > subset.count {
                    subset = subsetIncludingJ
                }
            }
            divisibleSubsetIncludingIndex[index] = [nums[index]] + subset
            return divisibleSubsetIncludingIndex[index]!
        }
    }
}
```