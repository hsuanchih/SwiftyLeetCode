
### Defanging an IP Address

Given a valid (IPv4) IP `address`, return a defanged version of that IP address.

A *defanged IP address* replaces every period `"."` with `"[.]"`.

__Example 1:__
```
Input: address = "1.1.1.1"
Output: "1[.]1[.]1[.]1"
```
__Example 2:__
```
Input: address = "255.100.50.0"
Output: "255[.]100[.]50[.]0"
```

__Constraints:__
* The given `address` is a valid IPv4 address.

### Solution
__O(n):__
```Swift
class Solution {
    func defangIPaddr(_ address: String) -> String {
        return Array(address).reduce("") {
             switch $1 {
                 case ".":
                 return $0 + "[.]"
                 default:
                 return $0 + String($1)
             }
        }
    }
}
```