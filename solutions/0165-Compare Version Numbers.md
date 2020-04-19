
### Compare Version Numbers

Compare two version numbers *version1* and *version2*.
If `version1 > version2` return `1`; if `version1 < version2` return `-1`;otherwise return `0`.

You may assume that the version strings are non-empty and contain only digits and the `.` character.

The `.` character does not represent a decimal point and is used to separate number sequences.

For instance, `2.5` is not "two and a half" or "half way to version three",</br> 
it is the fifth second-level revision of the second first-level revision.

You may assume the default revision number for each level of a version number to be `0`.</br> 
For example, version number `3.4` has a revision number of `3` and `4` for its first and second level revision number.</br> 
Its third and fourth level revision number are both `0`. 

__Example 1:__
```
Input: version1 = "0.1", version2 = "1.1"
Output: -1
```
__Example 2:__
```
Input: version1 = "1.0.1", version2 = "1"
Output: 1
```
__Example 3:__
```
Input: version1 = "7.5.2.4", version2 = "7.5.3"
Output: -1
```
__Example 4:__
```
Input: version1 = "1.01", version2 = "1.001"
Output: 0
Explanation: Ignoring leading zeroes, both “01” and “001" represent the same number “1”
```
__Example 5:__
```
Input: version1 = "1.0", version2 = "1.0.0"
Output: 0
Explanation: The first version number does not have a third level revision number, which means its third level revision number is default to "0"
```

__Note:__
1. Version strings are composed of numeric strings separated by dots . and this numeric strings __may__ have leading zeroes.
2. Version strings do not start or end with dots, and they will not be two consecutive dots.

### Solution
__O(n):__
```Swift
extension Substring {
    func compared(to substring: Substring) -> Int {
        switch Int(self)!-Int(substring)! {
            case 0:
            return 0
            case Int.min..<0:
            return -1
            default:
            return 1
        }
    }
}

class Solution {
    func compareVersion(_ version1: String, _ version2: String) -> Int {
        let version1 = version1.split(separator: "."), version2 = version2.split(separator: "."), length = max(version1.count, version2.count)
        for index in stride(from: 0, to: length, by: 1) {
            var result : Int = 0
            switch (version1.count-1-index, version2.count-1-index) {
                case (Int.min..<0, _):
                result = Substring("0").compared(to: version2[index])
                case (_, Int.min..<0):
                result = version1[index].compared(to: Substring("0"))
                default:
                result = version1[index].compared(to: version2[index])
            }
            if result != 0 {
                return result
            }
        }
        return 0
    }
}
```