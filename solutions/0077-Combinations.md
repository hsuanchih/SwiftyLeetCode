
### Combinations

Given two integers *n* and *k*, return all possible combinations of *k* numbers out of 1 ... *n*.

__Example:__
```
Input: n = 4, k = 2
Output:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

### Solution
__O(C(n,k)):__
```Swift
class Solution {
    func combine(_ n: Int, _ k: Int) -> [[Int]] {
        var temp : [Int] = [], result : [[Int]] = []
        generate(n, k, 1, &temp, &result)
        return result
    }
    
    func generate(_ n: Int, _ k: Int, _ index: Int, _ temp: inout [Int], _ result: inout [[Int]]) {
        if temp.count == k {
            result.append(temp)
            return
        }
        for i in stride(from: index, through: n, by: 1) {
            temp.append(i)
            generate(n, k, i+1, &temp, &result)
            temp.removeLast()
        }
    }
}
```