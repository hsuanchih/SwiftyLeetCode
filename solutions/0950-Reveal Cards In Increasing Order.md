
### Reveal Cards In Increasing Order

You are given an integer array `deck`. There is a deck of cards where every card has a unique integer. The integer on the `ith` card is `deck[i]`.

You can order the deck in any order you want. Initially, all the cards start face down (unrevealed) in one deck.

You will do the following steps repeatedly until all cards are revealed:

1. Take the top card of the deck, reveal it, and take it out of the deck.
2. If there are still cards in the deck then put the next top card of the deck at the bottom of the deck.
3. If there are still unrevealed cards, go back to step 1. Otherwise, stop.

Return an ordering of the deck that would reveal the cards in increasing order.

__Note__ that the first entry in the answer is considered to be the top of the deck.

__Example 1:__
```
Input: deck = [17,13,11,2,3,5,7]
Output: [2,13,3,11,5,17,7]
Explanation: 
We get the deck in the order [17,13,11,2,3,5,7] (this order does not matter), and reorder it.
After reordering, the deck starts as [2,13,3,11,5,17,7], where 2 is the top of the deck.
We reveal 2, and move 13 to the bottom.  The deck is now [3,11,5,17,7,13].
We reveal 3, and move 11 to the bottom.  The deck is now [5,17,7,13,11].
We reveal 5, and move 17 to the bottom.  The deck is now [7,13,11,17].
We reveal 7, and move 13 to the bottom.  The deck is now [11,17,13].
We reveal 11, and move 17 to the bottom.  The deck is now [13,17].
We reveal 13, and move 17 to the bottom.  The deck is now [17].
We reveal 17.
Since all the cards revealed are in increasing order, the answer is correct.
```
__Example 2:__
```
Input: deck = [1,1000]
Output: [1,1000]
```

__Constraints:__
* `1 <= deck.length <= 1000`
* `1 <= deck[i] <= pow(10, 6)`
* All the values of deck are __unique__.

### Solution
__O(pow(deck, 2) + deck * log(deck)) Time, O(2 * deck) Space:__
```Swift
class Solution {
    func deckRevealedIncreasing(_ deck: [Int]) -> [Int] {
        let deck: [Int] = deck.sorted()
        var indices: [Int] = Array(0 ..< deck.count)
        var result: [Int] = Array(repeating: 0, count: deck.count)
        for i in 0 ..< deck.count {
            result[indices.removeFirst()] = deck[i]
            if !indices.isEmpty {
                indices.append(indices.removeFirst())
            }
        }
        return result
    }
}
```