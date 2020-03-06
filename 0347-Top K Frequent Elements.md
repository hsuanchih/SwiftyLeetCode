
### Top K Frequent Elements

Given a non-empty array of integers, return the *__k__* most frequent elements.

__Example 1:__
```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```
__Example 2:__
```
Input: nums = [1], k = 1
Output: [1]
```

__Note:__
* You may assume *k* is always valid, 1 ≤ *k* ≤ number of unique elements.
* Your algorithm's time complexity __must be__ better than O(*n* log *n*), where *n* is the array's size.

### Solution
__O(u\*log(u)+nums+k) where u is the number of unique elements in nums (upperbound O(nums*log(nums))):__
```Swift
class Solution {
    func topKFrequent(_ nums: [Int], _ k: Int) -> [Int] {
        
        // tally tracks the number of occurences of each element in nums
        let tally = nums.reduce(into: [Int: Int]()) { $0[$1, default: 0]+=1 }
        var i = 0, result : [Int] = Array(repeating: 0, count: k)
        
        // Sort tally by value (ie, frequency of occurrence) and iterate through
        // tally to populate result with the corresponding key (element in nums)
        for (key, _) in (tally.sorted { $0.value > $1.value }) {
            if i == k {
                return result
            }
            result[i] = key
            i+=1
        }
        return result
    }
}
```