
### Design Twitter

Design a simplified version of Twitter where users can post tweets,</br> 
follow/unfollow another user and is able to see the 10 most recent tweets in the user's news feed.</br> 
Your design should support the following methods:

1. __postTweet(userId, tweetId)__: Compose a new tweet.
2. __getNewsFeed(userId)__: Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent.
3. __follow(followerId, followeeId)__: Follower follows a followee.
4. __unfollow(followerId, followeeId)__: Follower unfollows a followee.

__Example:__
```
Twitter twitter = new Twitter();

// User 1 posts a new tweet (id = 5).
twitter.postTweet(1, 5);

// User 1's news feed should return a list with 1 tweet id -> [5].
twitter.getNewsFeed(1);

// User 1 follows user 2.
twitter.follow(1, 2);

// User 2 posts a new tweet (id = 6).
twitter.postTweet(2, 6);

// User 1's news feed should return a list with 2 tweet ids -> [6, 5].
// Tweet id 6 should precede tweet id 5 because it is posted after tweet id 5.
twitter.getNewsFeed(1);

// User 1 unfollows user 2.
twitter.unfollow(1, 2);

// User 1's news feed should return a list with 1 tweet id -> [5],
// since user 1 is no longer following user 2.
twitter.getNewsFeed(1);
```

### Solution
__O(nums1+nums2):__
```Swift
class Twitter {
    
    private struct User {
        typealias UserID = Int
        
        var id : UserID
        var tweets : [Tweet]
        var following : Set<UserID>
        init(id: UserID) {
            self.id = id
            tweets = []
            following = []
        }
    }
    
    private struct Tweet {
        typealias TweetID = Int
        
        var id: TweetID
        var timestamp : TimeInterval
        init(id: TweetID) {
            self.id = id
            timestamp = Date().timeIntervalSince1970
        }
    }
    
    private var users : [Int: User]

    /** Initialize your data structure here. */
    init() {
        self.users = [:]
    }
    
    /** Compose a new tweet. */
    func postTweet(_ userId: Int, _ tweetId: Int) {
        var user = users[userId] ?? User(id: userId)
        user.tweets.append(Tweet(id: tweetId))
        users[userId] = user
    }
    
    /** Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent. */
    func getNewsFeed(_ userId: Int) -> [Int] {
        guard let user = users[userId] else { return [] }
        return (
            user
            .following
            .lazy
            .compactMap { self.users[$0]?.tweets }
            .flatMap { $0 } + user.tweets
        )
        .sorted { $0.timestamp > $1.timestamp }
        .prefix(10)
        .map { $0.id }
    }
    
    /** Follower follows a followee. If the operation is invalid, it should be a no-op. */
    func follow(_ followerId: Int, _ followeeId: Int) {
        var follower = users[followerId] ?? User(id: followerId),
        followee = users[followeeId] ?? User(id: followeeId)
        defer {
            users[followerId] = follower
            users[followeeId] = followee
        }
        follower.following.insert(followeeId)
    }
    
    /** Follower unfollows a followee. If the operation is invalid, it should be a no-op. */
    func unfollow(_ followerId: Int, _ followeeId: Int) {
        var follower = users[followerId] ?? User(id: followerId),
        followee = users[followeeId] ?? User(id: followeeId)
        defer {
            users[followerId] = follower
            users[followeeId] = followee
        }
        follower.following.remove(followeeId)
    }
}

/**
 * Your Twitter object will be instantiated and called as such:
 * let obj = Twitter()
 * obj.postTweet(userId, tweetId)
 * let ret_2: [Int] = obj.getNewsFeed(userId)
 * obj.follow(followerId, followeeId)
 * obj.unfollow(followerId, followeeId)
 */
```