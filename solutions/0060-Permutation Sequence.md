
### Unique Paths

The set `[1,2,3,...,n]` contains a total of *n*! unique permutations.

By listing and labeling all of the permutations in order, we get the following sequence for *n* = 3:
```
"123"
"132"
"213"
"231"
"312"
"321"
```
Given *n* and *k*, return the *k*th permutation sequence.

__Note:__
* Given n will be between 1 and 9 inclusive.
* Given k will be between 1 and n! inclusive.

__Example 1:__
```
Input: n = 3, k = 3
Output: "213"
```
__Example 2:__
```
Input: n = 4, k = 9
Output: "2314"
```

### Solution
__O(n!):__
```Swift
class Solution {
    func getPermutation(_ n: Int, _ k: Int) -> String {
        var k = k, seen : Set<Int> = [], temp : [Int] = []
        return permutation(n, &k, &seen, &temp).reduce("") { "\($0)\($1)" }
    }
    
    func permutation(_ n: Int, _ k: inout Int, _ seen: inout Set<Int>, _ temp: inout [Int]) -> [Int] {
        if temp.count == n {
            k-=1
            if k == 0 {
                return temp
            }
            return []
        }
        let nums = Array(1...n)
        for index in stride(from: 0, to: nums.count, by: 1) {
            if !seen.contains(index) {
                seen.insert(index)
                temp.append(nums[index])
                let result = permutation(n, &k, &seen, &temp)
                if !result.isEmpty {
                    return result
                }
                temp.removeLast()
                seen.remove(index)
            }
        }
        return []
    }
}
```
__O(n*k):__
```Swift
class Solution {
    func getPermutation(_ n: Int, _ k: Int) -> String {
        return solve(n, k-1, Array(1 ... n))
    }
    
    func solve(_ n: Int, _ k: Int, _ input: [Int]) -> String {
        if n == 1 {
            return "\(input.first!)"
        }
        var seq : [Int] = input
        let divisor = factorial(n-1),
        num = seq.remove(at: k/divisor)
        return "\(num)" + solve(n-1, k%divisor, seq)
    }
    
    func factorial(_ n: Int) -> Int {
        if n == 0 {
            return 1
        }
        return (1...n).reduce(1) { $0 * $1 }
    }
}
```