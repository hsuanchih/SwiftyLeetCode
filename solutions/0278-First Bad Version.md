
### First Bad Version

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have `n` versions `[1, 2, ..., n]` and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API `bool isBadVersion(version)` which will return whether `version` is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

__Example:__
```
Given n = 5, and version = 4 is the first bad version.

call isBadVersion(3) -> false
call isBadVersion(5) -> true
call isBadVersion(4) -> true

Then 4 is the first bad version.
```

### Solution
__O(n) Time - Brute-Force:__
```Swift
/**
 * The knows API is defined in the parent class VersionControl.
 *     func isBadVersion(_ version: Int) -> Bool{}
 */

class Solution : VersionControl {
    func firstBadVersion(_ n: Int) -> Int {
        for version in 1...n {
            if isBadVersion(version) {
                return version
            }
        }
        fatalError()
    }
}
```
__O(log\[base 2\](n)) Time - Binary-Search:__
```Swift
/**
 * The knows API is defined in the parent class VersionControl.
 *     func isBadVersion(_ version: Int) -> Bool{}
 */

class Solution : VersionControl {
    func firstBadVersion(_ n: Int) -> Int {
        var start = 1, end = n
        while start+1 < end {
            let mid = start + (end-start)/2
            switch isBadVersion(mid) {
                case true:
                end = mid
                case false:
                start = mid
            }
        }
        return isBadVersion(start) ? start : end
    }
}
```