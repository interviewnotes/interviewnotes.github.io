## Tiny URL

Design a service that takes in a long url and returns a tiny url, and vice-versa.

### Discuss
* How wide is the long url? Say 100s characters
* How wide is the tiny url? Say 7 to 10 characters
* Is the tiny url case-sensitive?
* Can the tiny url be sequential? Not so great to prevent hacking.

### API
```
GET https://host/get-tiny-url?long-url=<val>
GET https://host/get-long-url?tiny-url=<val>
```
### Architecture
* Consumer sends a get-tiny-url request, say based on REST, to the service
* Request is forwarded to a load balancer
* Load balancer forwards the request to one of the tiny-url services
* tiny-url service generates the tiny-url and persists it in the database
* The generated tiny-url is cached (centralized to all tiny-url services)
* The generated tiny-url is returned to the consumer

### Numbers
* Assuming, tiny-url
  * is 7 characters long 
  * is case-insensitive
  * can have 62 characters (a to z, A to Z, 0 to 9),
* the total number of unique combinations is 62 ^ 7 ~= 3.5 trillion
  * If the service handles 1K requests per second, it will take 110 years to exhaust 3.5 trillion
  * If the service handles 1M requests per second, it will take 40 days to exhaust 3.5 trillion
* number of bits required to represent 3.5 trillion = 42 (2 ^ 42 ~= 4.2 trillion) 

### Techniques to generate tiny-url
#### Random
1. on every request, generate a random number. Encode the number to a 7 character string.
2. check if the generated string exsits in the database.
3. if does not exist, use the generated string as the tiny-url. Otherwise, repeat the process.

* may not work if multiple services perform step 3 with the same random string.
  
#### Hashing
1. Hash the long-url using a hashing function, such as MD5 (which produces 128-bit hash). From the hash value, get first 42 bits. The numeric value from the first 42 bits can be encoded to a 7 character string. 
2. Follow step 2 and 3 from the Random approach

* compared to Random  approach, this will return the same value for the same long-url.
* same issues as Random approach

#### Counter
Generate a number from a sequence and encode it to a 7 character string.

Single host: A single host can keep a counter that can be atomically incremented. This host can be a database or a ZooKeeper node.

* host is a single point of failure.
* can become a bottleneck if there are too many total requests.
  
Distributed counter: Multiple hosts can have its own counter. The pattern of numbers can be unique to each host. Say, we have 64 hosts - the first 6 bits of the number can represent the host id, next 32-bits can be the counter, final 4 bits can be random.

* adding/removing hosts is complex.
* with only 4 random bits, the chances of collision within the same host is high. Increasing this size can help.
  
Range-based counter: Maintain ranges of numbers in a highly available service, such as a ZooKeeper node. Ranges can be 0-1000000, 1000001-2000000, etc. For a billion numbers, there will be 1K ranges. A tiny-url service will check-out a range from the node. The service will use that range as its counter range. Any new tiny-url service will check-out the next available range. 
* additional dependency on ZooKeeper like service.
  

* the output is predictable. Add some random bits to provide some obfuscation.

#### References
[https://www.youtube.com/watch?v=fMZMm_0ZhK4](https://www.youtube.com/watch?v=fMZMm_0ZhK4)

[Bitly Architecture](http://highscalability.com/blog/2014/7/14/bitly-lessons-learned-building-a-distributed-system-that-han.html)
