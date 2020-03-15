
### Count Primes

Count the number of prime numbers less than a non-negative number, __n__.

__Example:__
```
Input: 10
Output: 4
Explanation: There are 4 prime numbers less than 10, they are 2, 3, 5, 7.
```

### Solution
__O(n^2):__
```Swift
class Solution {
    func countPrimes(_ n: Int) -> Int {
        var result = 0
        for num in 0..<n {
            if isPrime(num) {
                result+=1
            }
        }
        return result
    }
    
    func isPrime(_ n: Int) -> Bool {
        switch n {
            case Int.min...1:
            return false
            case 2:
            return true
            default:
            break
        }
        for num in 2...n {
            if n%num == 0 {
                return false
            }
        }
        return true
    }
}
```
__O(n*(n/2)):__
```Swift
class Solution {
    func countPrimes(_ n: Int) -> Int {
        var result = 0
        for num in 0..<n {
            if isPrime(num) {
                result+=1
            }
        }
        return result
    }
    
    func isPrime(_ n: Int) -> Bool {
        switch n {
            case Int.min...1:
            return false
            case 2:
            return true
            default:
            break
        }
        for num in 2...(n+1)/2 {
            if n%num == 0 {
                return false
            }
        }
        return true
    }
}
```
__O(n*sqrt(n)):__
```Swift
class Solution {
    func countPrimes(_ n: Int) -> Int {
        var result = 0
        for num in 0..<n {
            if isPrime(num) {
                result+=1
            }
        }
        return result
    }
    
    func isPrime(_ n: Int) -> Bool {
        switch n {
            case Int.min...1:
            return false
            case 2:
            return true
            default:
            break
        }
        var num = 2
        while num*num <= n {
            if n%num == 0 {
                return false
            }
            num+=1
        }
        return true
    }
}
```
__O(n*log(n)):__
```Swift
class Solution {
    func countPrimes(_ n: Int) -> Int {

        // Numbers less than 2 are not prime number
        guard n > 1 else { return 0 }
        var isPrime : [Bool] = Array(repeating: true, count: n)
        var num = 2, result = 0

        // Iterate through all numbers through n
        while num < n {

            // Move on if number is not a prime
            defer { num+=1 }
            guard isPrime[num] else { continue }

            // Increment result if number is prime
            result+=1

            // Set all multiples of num to false as they
            // cannot be be primes
            var j = num*num
            while j < n {
                isPrime[j] = false
                j+=num
            }
        }
        return result
    }
}
```