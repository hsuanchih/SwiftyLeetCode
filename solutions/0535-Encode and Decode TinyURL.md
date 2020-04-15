
### Encode and Decode TinyURL
```
Note: This is a companion problem to the System Design problem: Design TinyURL.
```
TinyURL is a URL shortening service where you enter a URL such as</br> 
`https://leetcode.com/problems/design-tinyurl`</br>
and it returns a short URL such as</br> 
`http://tinyurl.com/4e9iAk`.

Design the `encode` and `decode` methods for the TinyURL service.</br> 
There is no restriction on how your encode/decode algorithm should work.</br> 
You just need to ensure that a URL can be encoded to a tiny URL and the tiny URL can be decoded to the original URL.

### Solution
```Swift
class Codec {
    
    // The conversion algorithm is to convert the long url
    // to an integer string
    // longUrls implements a long url look-up given a short url (integer string)
    // shortUrls implements a short url look-up given a long url (string)
    var longUrls : [String] = []
    var shortUrls : [String: Int] = [:]
    
    // Encodes a URL to a shortened URL.
    func encode(_ longUrl: String) -> String {
        let shortUrl = shortUrls[longUrl] ?? longUrls.count
        if shortUrl == longUrls.count {
            longUrls.append(longUrl)
            shortUrls[longUrl] = shortUrl
        }
        return String(shortUrl)
    }
    
    // Decodes a shortened URL to its original URL.
    func decode(_ shortUrl: String) -> String {
        guard let shortUrl = Int(shortUrl), shortUrl < longUrls.count else {
            fatalError()
        }
        return longUrls[shortUrl]
    }
}

/**
 * Your Codec object will be instantiated and called as such:
 * let obj = Codec()
 * val s = obj.encode(longUrl)
 * let ans = obj.decode(s)
*/
```