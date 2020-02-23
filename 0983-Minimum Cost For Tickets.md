
### Minimum Cost For Tickets

In a country popular for train travel, you have planned some train travelling one year in advance.</br>
The days of the year that you will travel is given as an array `days`.  Each day is an integer from `1` to `365`.

Train tickets are sold in 3 different ways:

a 1-day pass is sold for `costs[0]` dollars;
a 7-day pass is sold for `costs[1]` dollars;
a 30-day pass is sold for `costs[2]` dollars.
The passes allow that many days of consecutive travel.</br>
For example, if we get a 7-day pass on day 2, then we can travel for 7 days: day 2, 3, 4, 5, 6, 7, and 8.

Return the minimum number of dollars you need to travel every day in the given list of `days`.

__Example 1:__
```
Input: days = [1,4,6,7,8,20], costs = [2,7,15]
Output: 11
Explanation: 
For example, here is one way to buy passes that lets you travel your travel plan:
On day 1, you bought a 1-day pass for costs[0] = $2, which covered day 1.
On day 3, you bought a 7-day pass for costs[1] = $7, which covered days 3, 4, ..., 9.
On day 20, you bought a 1-day pass for costs[0] = $2, which covered day 20.
In total you spent $11 and covered all the days of your travel.
```
__Example 2:__
```
Input: days = [1,2,3,4,5,6,7,8,9,10,30,31], costs = [2,7,15]
Output: 17
Explanation: 
For example, here is one way to buy passes that lets you travel your travel plan:
On day 1, you bought a 30-day pass for costs[2] = $15 which covered days 1, 2, ..., 30.
On day 31, you bought a 1-day pass for costs[0] = $2 which covered day 31.
In total you spent $17 and covered all the days of your travel.
```

__Note:__
1. `1 <= days.length <= 365`
2. `1 <= days[i] <= 365`
3. `days` is in strictly increasing order.
4. `costs.length == 3`
5. `1 <= costs[i] <= 1000`

### Solution
__O(Range of Travel Period):__
```Swift
class Solution {
    func mincostTickets(_ days: [Int], _ costs: [Int]) -> Int {

        // days is in increaing order, so the last day of travel determines the number of days
        // we need to iterate through
        let lastDay = days.last!

        // The days we need to take the train
        let days = Set(days)

        // Convert costs into Dictionary<Duration of Pass, Cost of Pass>
        let costs = zip([1, 7, 30], costs).reduce(into: [Int: Int]()) { $0[$1.0] = $1.1 }

        // Keeps track of the minimum cost required to travel on that day
        var memo : [Int] = Array(repeating: 0, count: lastDay+1)

        // Iterate from Day 1 through to the last day of travel
        for day in 1...lastDay {

            // If no travel on the specfic day, no potential cost incurs, so the minimum cost
            // is same as the day before
            if !days.contains(day) {
                memo[day] = memo[day-1]
                continue
            }

            // Need to travel on this day, calculate the minimum cost that would incur using each
            // kind of pass
            memo[day] = Int.max
            for (key, value) in costs {
                memo[day] = min(memo[max(0, day-key)]+value, memo[day])
            }
        }

        return memo.last!
    }
}
```
