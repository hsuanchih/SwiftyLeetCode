
### Bulls and Cows

You are playing the following Bulls and Cows game with your friend:</br> 
You write down a number and ask your friend to guess what the number is.</br> 
Each time your friend makes a guess, you provide a hint that indicates</br> 
how many digits in said guess match your secret number exactly in both digit and position (called "bulls")</br> 
and how many digits match the secret number but locate in the wrong position (called "cows").</br> 
Your friend will use successive guesses and hints to eventually derive the secret number.

Write a function to return a hint according to the secret number and friend's guess,</br> 
use `A` to indicate the bulls and `B` to indicate the cows. 

Please note that both secret number and friend's guess may contain duplicate digits.

__Example 1:__
```
Input: secret = "1807", guess = "7810"

Output: "1A3B"

Explanation: 1 bull and 3 cows. The bull is 8, the cows are 0, 1 and 7.
```
__Example 2:__
```
Input: secret = "1123", guess = "0111"

Output: "1A1B"

Explanation: The 1st 1 in friend's guess is a bull, the 2nd or 3rd 1 is a cow.
```

__Note:__ You may assume that the secret number and your friend's guess only contain digits, and their lengths are always equal.

### Solution
__O(secret+guess):__
```Swift
class Solution {
    func getHint(_ secret: String, _ guess: String) -> String {
        var lookup : [Character: Int] = [:], bull = 0, cow = 0
        let secret = Array(secret), guess = Array(guess)

        for i in stride(from: 0, to: secret.count, by: 1) {
            switch secret[i] {

            // If secret & guess have same character in same position, it's a bull
            case guess[i]:
                bull+=1

            // Otherwise track the number of occurrences of each character in secret
            default:
                lookup[secret[i], default: 0]+=1
            }
        }

        for i in stride(from: 0, to: guess.count, by: 1) {
            switch guess[i] {

            // If guess & secret have the same character in same position, ignore this
            // from our cows calculation as we've taken care of it with bulls
            case secret[i]:
                break

            // Compute cows
            default:
                if let count = lookup[guess[i]], count >= 1 {
                    cow+=1
                    lookup[guess[i]] = count-1
                }
            }
        }
        return "\(bull)A\(cow)B"
    }
}
```