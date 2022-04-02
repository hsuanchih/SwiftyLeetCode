
### Keys and Rooms

There are `n` rooms labeled from `0` to `n - 1` and all the rooms are locked except for room `0`. Your goal is to visit all the rooms. However, you cannot enter a locked room without having its key.

When you visit a room, you may find a set of __distinct keys__ in it. Each key has a number on it, denoting which room it unlocks, and you can take all of them with you to unlock the other rooms.

Given an array `rooms` where `rooms[i]` is the set of keys that you can obtain if you visited room `i`, return `true` if you can visit __all__ the rooms, or `false` otherwise.

__Example 1:__
```
Input: rooms = [[1],[2],[3],[]]

Output: true

Explanation: 
We visit room 0 and pick up key 1.
We then visit room 1 and pick up key 2.
We then visit room 2 and pick up key 3.
We then visit room 3.
Since we were able to visit every room, we return true.
```

__Example 2:__
```
Input: rooms = [[1,3],[3,0,1],[2],[0]]

Output: false

Explanation: We can not enter room number 2 since the only key that unlocks it is in that room.
```

__Constraints:__
* `n == rooms.length`
* `2 <= n <= 1000`
* `0 <= rooms[i].length <= 1000`
* `1 <= sum(rooms[i].length) <= 3000`
* `0 <= rooms[i][j] < n`
* All the values of `rooms[i]` are __unique__.

### Solution

__O(n*n) (V+E), BFS:__
```swift
class Solution {
    func canVisitAllRooms(_ rooms: [[Int]]) -> Bool {
        guard !rooms.isEmpty else { return false }
        var visited: Set<Int> = [0]
        visit(room: 0, rooms: rooms, visited: &visited)
        return visited.count == rooms.count
    }
    
    func visit(room: Int, rooms: [[Int]], visited: inout Set<Int>) {
        rooms[room].forEach {
            if !visited.contains($0) {
                visited.insert($0)
                visit(room: $0, rooms: rooms, visited: &visited)
            }
        }
    }
}
```