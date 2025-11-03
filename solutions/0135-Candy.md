
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
        var candies: [Int] = Array(repeating: 1, count: ratings.count)
        for i in 0 ..< ratings.count {
            for j in stride(from: i, to: 0, by: -1) where ratings[j - 1] > ratings[j] && candies[j - 1] <= candies[j] {
                candies[j - 1] += 1
            }
            for k in stride(from: i, to: ratings.count - 1, by: 1) where ratings[k + 1] > ratings[k] && candies[k + 1] <= candies[k] {
                candies[k + 1] += 1
            }
        }
        return candies.reduce(into: 0, +=)
    }
}
```
__O(ratings) Time, O(ratings) Space - Iterative Greedy:__
```Swift
class Solution {
    func candy(_ ratings: [Int]) -> Int {
        var candies: [Int] = Array(repeating: 1, count: ratings.count)
        for i in stride(from: 0, to: ratings.count - 1, by: 1) where ratings[i + 1] > ratings[i] && candies[i + 1] <= candies[i] {
            candies[i + 1] = candies[i] + 1
        }
        for i in stride(from: ratings.count - 1, to: 0, by: -1) where ratings[i - 1] > ratings[i] && candies[i - 1] <= candies[i] {
            candies[i - 1] = candies[i] + 1
        }
        return candies.reduce(into: 0, +=)
    }
}
```