
### Excel Sheet Column Title

Given a positive integer, return its corresponding column title as appear in an Excel sheet.

__For example:__
```
    1 -> A
    2 -> B
    3 -> C
    ...
    26 -> Z
    27 -> AA
    28 -> AB
    ...
```
__Example 1:__
```
Input: 1
Output: "A"
```
__Example 2:__
```
Input: 28
Output: "AB"
```
__Example 3:__
```
Input: 701
Output: "ZY"
```

### Solution
__Recursive:__
```Swift
class Solution {
    func convertToTitle(_ n: Int) -> String {
        if n == 0 {
            return ""
        }
        var prefix = convertToTitle((n-1)/26)
        prefix.append(Character(UnicodeScalar(Character("A").asciiValue!+UInt8((n-1)%26))))
        return prefix
    }
}
```
__Iterative:__
```Swift
class Solution {
    func convertToTitle(_ n: Int) -> String {
        var n = n, result = ""
        while n > 0 {
            let num = (n-1)%26
            result.append(Character(UnicodeScalar(Character("A").asciiValue!+UInt8((n-1)%26))))
            n = (n-1)/26
        }
        return String(result.reversed())
    }
}
```