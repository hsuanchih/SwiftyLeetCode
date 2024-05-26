
### Course Schedule

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you must take course `bi` first if you want to take course `ai`.

* For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return `true` if you can finish all courses. Otherwise, return `false`.

__Example 1:__
```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0. So it is possible.
```
__Example 2:__
```
Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.
```

__Constraints:__
* `1 <= numCourses <= 2000`
* `0 <= prerequisites.length <= 5000`
* `prerequisites[i].length == 2`
* `0 <= ai, bi < numCourses`
* All the pairs `prerequisites[i]` are __unique__.

### Solution
__Cycle Detection - TLE:__
```Swift
class Solution {
    func canFinish(_ numCourses: Int, _ prerequisites: [[Int]]) -> Bool {
        var adjList: [[Int]] = Array(repeating: [], count: numCourses)

        // Preprocess prerequisites into an adjacency list
        prerequisites.forEach {
            adjList[$0[0]].append($0[1])
        }

        for course in 0 ..< numCourses {
            // Seen is used to track the vertices visited along a DFS walk starting from each course
            var seen: Set<Int> = []
            if hasCycle(adjList, course, &seen) {
                return false
            }
        }
        return true
    }

    func hasCycle(_ adjList: [[Int]], _ course: Int, _ seen: inout Set<Int>) -> Bool {
        if seen.contains(course) {
            // If we end up visiting a vertex that has been visited before, a cycle exists
            return true
        } else {
            // Otherwise continue down DFS walk, marking vertices as visited along the way until:
            // * DFS completes at a tree edge & no cycle is detected, or
            // * A cycle is detected before the DFS completes
            seen.insert(course)
            for next in adjList[course] where hasCycle(adjList, next, &seen) {
                return true
            }
            seen.remove(course)
            return false
        }
    }
}
```
__Cycle Detection Improved - Omit Visited Vertices:__
```Swift
class Solution {
    func canFinish(_ numCourses: Int, _ prerequisites: [[Int]]) -> Bool {
        var adjList: [[Int]] = Array(repeating: [], count: numCourses)
        prerequisites.forEach {
            adjList[$0[0]].append($0[1])
        }

        var seen: Set<Int> = []
        for course in 0 ..< numCourses {
            var visitStack: Set<Int> = []
            if hasCycle(adjList, course, &seen, &visitStack) {
                return false
            }
        }
        return true
    }

    func hasCycle(_ adjList: [[Int]], _ course: Int, _ seen: inout Set<Int>, _ visitStack: inout Set<Int>) -> Bool {
        if visitStack.contains(course) {
            return true
        } else if !seen.contains(course) {
            seen.insert(course)
            visitStack.insert(course)
            for next in adjList[course] where hasCycle(adjList, next, &seen, &visitStack) {
                return true
            }
            visitStack.remove(course)
            return false
        } else {
            return false
        }
    }
}
```
__O(V+E):__
```Swift
class Solution {
    func canFinish(_ numCourses: Int, _ prerequisites: [[Int]]) -> Bool {
        let adjList: [[Int]] = adjacencyList(numCourses, prerequisites)
        var visited: Set<Int> = [], visitStack: Set<Int> = []
        for vertex in 0 ..< numCourses {
            if !dfs(vertex, adjList, &visited, &visitStack) {
                return false
            }
        }
        return true
    }
    
    func dfs(_ vertex: Int, _ adjList: [[Int]], _ visited: inout Set<Int>, _ visitStack: inout Set<Int>) -> Bool {
        guard !visitStack.contains(vertex) else { return false }
        visitStack.insert(vertex)
        if !visited.contains(vertex) {
            visited.insert(vertex)
            for next in adjList[vertex] {
                if !dfs(next, adjList, &visited, &visitStack) {
                    return false
                }
            }
        }
        visitStack.remove(vertex)
        return true
    }
    
    func adjacencyList(_ numCourses: Int, _ prerequisites: [[Int]]) -> [[Int]] {
        var result: [[Int]] = Array(repeating: [], count: numCourses)
        prerequisites.forEach {
            result[$0[1]].append($0[0])
        }
        return result
    }
}
```