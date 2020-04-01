
### Number of Segments in a String

Count the number of segments in a string, where a segment is defined to be a contiguous sequence of non-space characters.

Please note that the string does not contain any __non-printable__ characters.

__Example:__
```
Input: "Hello, my name is John"
Output: 5
```

### Solution
__O(s) Time, O(s) Space:__
```Swift
class Solution {
    func countSegments(_ s: String) -> Int {
        return s.split(separator: " ").count
    }
}
```