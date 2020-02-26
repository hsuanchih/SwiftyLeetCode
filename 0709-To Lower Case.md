
### To Lower Case

Implement function ToLowerCase() that has a string parameter str, and returns the same string in lowercase.

__Example 1:__
```
Input: "Hello"
Output: "hello"
```
__Example 2:__
```
Input: "here"
Output: "here"
```
__Example 3:__
```
Input: "LOVELY"
Output: "lovely"
```

### Solution
__O(n):__
```Swift
class Solution {
    func toLowerCase(_ str: String) -> String {
        return str.lowercased()
    }
}
```