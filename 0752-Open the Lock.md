
### Open the Lock

You have a lock in front of you with 4 circular wheels. Each wheel has 10 slots:</br> 
`'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'`.</br> 
The wheels can rotate freely and wrap around:</br> 
for example we can turn `'9'` to be `'0'`, or `'0'` to be `'9'`.</br> 
Each move consists of turning one wheel one slot.

The lock initially starts at `'0000'`, a string representing the state of the 4 wheels.

You are given a list of `deadends` dead ends, meaning if the lock displays any of these codes,</br> 
the wheels of the lock will stop turning and you will be unable to open it.

Given a `target` representing the value of the wheels that will unlock the lock,</br> 
return the minimum total number of turns required to open the lock, or `-1` if it is impossible.

__Example 1:__
```
Input: deadends = ["0201","0101","0102","1212","2002"], target = "0202"
Output: 6
Explanation:
A sequence of valid moves would be "0000" -> "1000" -> "1100" -> "1200" -> "1201" -> "1202" -> "0202".
Note that a sequence like "0000" -> "0001" -> "0002" -> "0102" -> "0202" would be invalid,
because the wheels of the lock become stuck after the display becomes the dead end "0102".
```
__Example 2:__
```
Input: deadends = ["8888"], target = "0009"
Output: 1
Explanation:
We can turn the last wheel in reverse to move from "0000" -> "0009".
```
__Example 3:__
```
Input: deadends = ["8887","8889","8878","8898","8788","8988","7888","9888"], target = "8888"
Output: -1
Explanation:
We can't reach the target without getting stuck.
```
__Example 4:__
```
Input: deadends = ["0000"], target = "8888"
Output: -1
```
__Note:__
1. The length of `deadends` will be in the range `[1, 500]`.
2. `target` will not be in the list `deadends`.
3. Every string in `deadends` and the string `target` will be a string of 4 digits from the 10,000 possibilities `'0000'` to `'9999'`.

### Solution
__O(V+E):__
```Swift
extension Int {

    // Converts a Character to Int
    init(_ character: Character) {
        self = Int(String(character))!
    }
}

extension Solution.Lock {

    // Converts a string into a Lock
    static func from(_ string: String) -> Solution.Lock {
        return Array<Character>(string).map(Int.init)
    }
    
    // Generate the next possible Lock states given the Lock's current state
    // Each wheel can be turned up 1 (+1%10) or down 1 (+9%10)
    var nextTurns : [Solution.Lock] {
        var lock = self, result : [Solution.Lock] = []
        for i in 0..<lock.count {
            let curr = lock[i]
            [1, 9].forEach {
                lock[i] = (curr+$0)%10
                result.append(lock)
            }
            lock[i] = curr
        }
        return result
    }
}

class Solution {
    
    // Lock abstraction: Lock is represented by an array of 4 Int elements
    typealias Lock = [Int]
    
    func openLock(_ deadends: [String], _ target: String) -> Int {
        let deadends = Set(deadends.map(Lock.from)), 
        target = Lock.from(target), 
        start = Lock([0,0,0,0])

        // If deadends contains target or start, we know there is no solution
        if deadends.contains(target) || deadends.contains(start) {
            return -1
        }

        // Use BFS to find the shortest path from start to target
        var visited : Set<Lock> = [start], queue : [Lock] = [start], level = 0
        while !queue.isEmpty {
            let size = queue.count
            for _ in 0..<size {
                let curr = queue.removeFirst()
                if curr == target {
                    return level
                }
                curr.nextTurns.forEach {
                    if !deadends.contains($0) && !visited.contains($0) {
                        queue.append($0)
                        visited.insert($0)
                    }
                }
            }
            level+=1
        }
        return -1
    }
}
```