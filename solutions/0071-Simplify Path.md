
### Simplify Path

Given an __absolute path__ for a file (Unix-style), simplify it. Or in other words, convert it to the __canonical path__.

In a UNIX-style file system, a period `.` refers to the current directory.</br> 
Furthermore, a double period `..` moves the directory up a level.</br>
For more information, see: [Absolute path vs relative path in Linux/Unix](https://www.linuxnix.com/abslute-path-vs-relative-path-in-linuxunix/)

Note that the returned canonical path must always begin with a slash `/`, and there must be only a single slash `/` between two directory names. The last directory name (if it exists) __must not__ end with a trailing `/`. Also, the canonical path must be the __shortest__ string representing the absolute path.

__Example 1:__
```
Input: "/home/"
Output: "/home"
Explanation: Note that there is no trailing slash after the last directory name.
```
__Example 2:__
```
Input: "/../"
Output: "/"
Explanation: Going one level up from the root directory is a no-op, as the root level is the highest level you can go.
```
__Example 3:__
```
Input: "/home//foo/"
Output: "/home/foo"
Explanation: In the canonical path, multiple consecutive slashes are replaced by a single one.
```
__Example 4:__
```
Input: "/a/./b/../../c/"
Output: "/c"
```
__Example 5:__
```
Input: "/a/../../b/../c//.//"
Output: "/c"
```
__Example 6:__
```
Input: "/a//b////c/d//././/.."
Output: "/a/b/c"
```

### Solution
__O(path) Time, O(path) Space:__
```Swift
class Solution {
    func simplifyPath(_ path: String) -> String {
        let path = path.split(separator: "/")
        var stack : [String] = []
        for component in path {
            switch component {
                case "..":
                if let temp = stack.last {
                    stack.removeLast()
                }
                case ".":
                break
                default:
                stack.append(String(component))
            }
        }
        return stack.isEmpty ? "/" : stack.reduce("") { $0+"/\($1)" }
    }
}
```