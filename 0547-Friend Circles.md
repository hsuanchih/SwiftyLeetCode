
### Friend Circles

There are __N__ students in a class. Some of them are friends, while some are not.</br> 
Their friendship is transitive in nature. For example, if A is a __direct__ friend of B,</br> 
and B is a direct friend of C, then A is an __indirect__ friend of C.</br> 
And we defined a friend circle is a group of students who are direct or indirect friends.

Given a __N\*N__ matrix __M__ representing the friend relationship between students in the class.</br> 
If M[i][j] = 1, then the ith and jth students are __direct__ friends with each other, otherwise not.</br> 
And you have to output the total number of friend circles among all the students.

__Example 1:__
```
Input: 
[[1,1,0],
 [1,1,0],
 [0,0,1]]
Output: 2
Explanation:The 0th and 1st students are direct friends, so they are in a friend circle. 
The 2nd student himself is in a friend circle. So return 2.
```
__Example 2:__
```
Input: 
[[1,1,0],
 [1,1,1],
 [0,1,1]]
Output: 1
Explanation:The 0th and 1st students are direct friends, the 1st and 2nd students are direct friends, 
so the 0th and 2nd students are indirect friends. All of them are in the same friend circle, so return 1.
```

__Note:__
1. N is in range `[1,200]`.
2. `M[i][i] = 1` for all students.
3. If `M[i][j] = 1`, then `M[j][i] = 1`.

### Solution
```Swift
class Solution {
    func findCircleNum(_ M: [[Int]]) -> Int {
        var visited : Set<Int> = [], result : Int = 0
        for person in 0..<M.count {
            if visited.contains(person) {
                continue
            }
            result+=1
            visited.insert(person)
            bfs(person, M, &visited)
        }
        return result
    }
    
    func bfs(_ person: Int, _ M: [[Int]], _ visited: inout Set<Int>) {
        for friend in 0..<M.count {
            if visited.contains(friend) || friend == person {
                continue
            }
            if M[person][friend] == 1 {
                visited.insert(friend)
                bfs(friend, M, &visited)
            }
        }
    }
}
```