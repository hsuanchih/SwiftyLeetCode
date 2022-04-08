
### Delete and Earn

You are given an integer array `nums`. You want to maximize the number of points you get by performing the following operation any number of times:

* Pick any `nums[i]` and delete it to earn `nums[i]` points. Afterwards, you must delete __every__ element equal to `nums[i] - 1` and __every__ element equal to `nums[i] + 1`.

Return the __maximum number of points__ you can earn by applying the above operation some number of times.

__Example 1:__
```
Input: nums = [3,4,2]

Output: 6

Explanation: You can perform the following operations:
- Delete 4 to earn 4 points. Consequently, 3 is also deleted. nums = [2].
- Delete 2 to earn 2 points. nums = [].
You earn a total of 6 points.
```

__Example 2:__
```
Input: nums = [2,2,3,3,3,4]

Output: 9

Explanation: You can perform the following operations:
- Delete a 3 to earn 3 points. All 2's and 4's are also deleted. nums = [3,3].
- Delete a 3 again to earn 3 points. nums = [3].
- Delete a 3 once more to earn 3 points. nums = [].
You earn a total of 9 points.
```

__Constraints:__
* `1 <= nums.length <= 2 * 10^4`
* `1 <= nums[i] <= 10^4`

### Solution
__O(2^nums) Time - Exhaustive Search, TLE:__
```swift
class Solution {
    func deleteAndEarn(_ nums: [Int]) -> Int {
        guard !nums.isEmpty else { return 0 }
        // The deleted set of numbers
        var deleted: Set<Int> = []
        return maxEarning(index: 0, nums: nums, deleted: &deleted)
    }
    
    func maxEarning(index: Int, nums: [Int], deleted: inout Set<Int>) -> Int {
        guard index < nums.count else { return 0 }
        let num = nums[index]

        // If the deleted set already includes num, num should be added to the
        // total points.
        if deleted.contains(num) {
            return num + maxEarning(index: index+1, nums: nums, deleted: &deleted)
        }

        // If num can be deleted (ie, we haven't deleted a number where number-1 & number+1 is num),
        // we can either choose to delete the number or not delete the number
        // Otherwise, we cannot delete the num
        if ([num - 1, num + 1].allSatisfy { !deleted.contains($0) }) {
            // Exercise option 1: We choose to delete num
            deleted.insert(num)
            let includeNum = num + maxEarning(index: index+1, nums: nums, deleted: &deleted)
            deleted.remove(num)

            // Exercise option 2: We choose not to delete num
            let excludeNum = maxEarning(index: index+1, nums: nums, deleted: &deleted)

            // And maximize the points we can get with either option
            return max(includeNum, excludeNum)
        } else {
            // We cannot delete num because num does not satify the constaints for deletion
            return maxEarning(index: index+1, nums: nums, deleted: &deleted)
        }
    }
}
```

__O(nums + nums.max()) Time - Memoization:__
```swift
class Solution {
    func deleteAndEarn(_ nums: [Int]) -> Int {
        // Identify the max value in nums & total number of points that can be earned by deleting
        // a specific number in nums
        let (maxValue, cumulativePoints): (Int, [Int: Int]) = nums.reduce(into: (0, [Int: Int]())) {
            $0.0 = max($1, $0.0)
            $0.1[$1, default: 0] += $1
        }

        // This records the maximum number of points that can be earned by deleting numbers from 0 up to i
        var memo: [Int] = Array(repeating: 0, count: maxValue + 1)

        (0 ... maxValue).forEach {
            // For each number that exists between 0 to max value in nums
            switch $0 {
            case 0:
                // We do not earn any points up to number 0
                memo[0] = 0
            case 1:
                // We can only earn up to cumulativePoints[1] up to number 1, since we either
                // * include 0, and earn 0 points
                // * or include 1, and earn cumulativePoints[1]
                // but we cannot do both
                memo[1] = cumulativePoints[1] ?? 0
            case let num:
                // We can either:
                // * include num, in this case we may only include max points we can earn up to num-2
                // * exclude num, in this case we may includes max points we can earn up to num-1
                // In either case, we want to maximize the points we can earn up to num
                memo[num] = max((memo[num-2] ?? 0) + (cumulativePoints[num] ?? 0), memo[num-1] ?? 0)
            }
        }
        return memo.last ?? 0
    }
}
```