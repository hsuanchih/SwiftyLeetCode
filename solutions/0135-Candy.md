
### Candy

There are `n` children standing in a line. Each child is assigned a rating value given in the integer array `ratings`.

You are giving candies to these children subjected to the following requirements:

* Each child must have at least one candy.
* Children with a higher rating get more candies than their neighbors.

Return the _minimum number of candies you need to have to distribute the candies to the children_.

__Example 1:__
```
Input: ratings = [1,0,2]
Output: 5
Explanation: You can allocate to the first, second and third child with 2, 1, 2 candies respectively.
```
__Example 2:__
```
Input: ratings = [1,2,2]
Output: 4
Explanation: You can allocate to the first, second and third child with 1, 2, 1 candies respectively.
The third child gets 1 candy because it satisfies the above two conditions.
```

__Constraints:__
* `n == ratings.length`
* `1 <= n <= 2 * pow(10, 4)`
* `0 <= ratings[i] <= 2 * pow(10, 4)`

### Solution
__O(pow(ratings, 2)) Time, O(ratings) Space - Recursive Brute-Force:__
```Swift
class Solution {
    enum Direction {
        case left, right
    }

    func candy(_ ratings: [Int]) -> Int {
        var candy: [Int] = Array(repeating: 1, count: ratings.count)
        for index in 0 ..< ratings.count {
            [Direction.left, .right].forEach {
                updateCandiesForNeighbor(ratings, index, $0, &candy)
            }
        }
        return candy.reduce(into: 0, +=)
    }

    func updateCandiesForNeighbor(_ ratings: [Int], _ index: Int, _ direction: Direction, _ candy: inout [Int]) {
        switch (index, direction) {
        case (let index, .left) where index > 0 && ratings[index - 1] > ratings[index] && candy[index - 1] <= candy[index]:
            candy[index - 1] += 1
            updateCandiesForNeighbor(ratings, index - 1, .left, &candy)
        case (let index, .right) where index < ratings.count - 1 && ratings[index + 1] > ratings[index] && candy[index + 1] <= candy[index]:
            candy[index + 1] += 1
            updateCandiesForNeighbor(ratings, index + 1, .right, &candy)
        default:
            break
        }
    }
}
```
__O(pow(ratings, 2)) Time, O(ratings) Space - Iterative Brute-Force:__
```Swift
class Solution {
    func candy(_ ratings: [Int]) -> Int {
        var candy: [Int] = Array(repeating: 1, count: ratings.count)
        for target in 0 ..< ratings.count {
            for i in stride(from: target - 1, through: 0, by: -1) {
                if ratings[i] > ratings[i + 1] && candy[i] <= candy[i + 1] {
                    candy[i] += 1
                } else {
                    break
                }
            }
            for j in stride(from: target + 1, to: ratings.count, by: 1) {
                if ratings[j] > ratings[j - 1] && candy[j] <= candy[j - 1] {
                    candy[j] += 1
                } else {
                    break
                }
            }
        }
        return candy.reduce(into: 0, +=)
    }
}
```
__O(ratings) Time, O(ratings) Space - Iterative Greedy:__
```Swift
class Solution {
    func candy(_ ratings: [Int]) -> Int {
        guard !ratings.isEmpty else { return 0 }
        var candy: [Int] = Array(repeating: 1, count: ratings.count)
        for target in stride(from: 1, to: ratings.count, by: 1) where ratings[target - 1] < ratings[target] && candy[target] <= candy[target - 1] {
            candy[target] = candy[target - 1] + 1
        }
        for target in stride(from: ratings.count - 2, through: 0, by: -1) where ratings[target + 1] < ratings[target] && candy[target] <= candy[target + 1] {
            candy[target] = candy[target + 1] + 1
        }
        return candy.reduce(into: 0, +=)
    }
}
```