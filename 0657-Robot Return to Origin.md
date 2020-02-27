
### Robot Return to Origin

There is a robot starting at position (0, 0), the origin, on a 2D plane.</br> 
Given a sequence of its moves, judge if this robot __ends up at (0, 0)__ after it completes its moves.

The move sequence is represented by a string, and the character moves[i] represents its ith move.</br> 
Valid moves are R (right), L (left), U (up), and D (down).</br> 
If the robot returns to the origin after it finishes all of its moves, return true. Otherwise, return false.

__Note:__ The way that the robot is "facing" is irrelevant.</br> 
"R" will always make the robot move to the right once, "L" will always make it move left, etc.</br> 
Also, assume that the magnitude of the robot's movement is the same for each move.

__Example 1:__
```
Input: "UD"
Output: true 
Explanation: The robot moves up once, and then down once. All moves have the same magnitude, so it ended up at the origin where it started. Therefore, we return true.
```
__Example 2:__
```
Input: "LL"
Output: false
Explanation: The robot moves left twice. It ends up two "moves" to the left of the origin. We return false because it is not at the origin at the end of its moves.
```

### Solution
__O(moves):__
```Swift
class Solution {
    func judgeCircle(_ moves: String) -> Bool {
        var x = 0, y = 0
        for move in moves {
            switch move {
            case "U": y+=1
            case "D": y-=1
            case "L": x-=1
            case "R": x+=1
            default: break
            }
        }
        return x == 0 && y == 0
    }
}
```