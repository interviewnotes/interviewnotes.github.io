## Tiny URL

#### Problem Statement
Design a service that takes in a long url and returns a tiny url, and vice-versa.

#### Discuss
* How wide is the long url? Say 100s characters
* How wide is the tiny url? Say 7 to 10 characters
* Is the tiny url case-sensitive?

#### API
```java
String getTinyUrl(String longUrl);
String getLongUrl(String tinyUrl);
```

#### Database Schema


#### Architecture
* Consumer sends a getTinyURL request, say based on REST, to the webapp
* Request is forwarded to a load balancer
* Load Balancer forwards the request to one of the tinyURL services
* tinyURL service generates the tinyURL and persists it in the database
* The generated tinyUrl is cached (centralized to all tinyURL services)
* The generated tinyUrl is returned to the consumer

#### Numbers
* Assuming, tinyUrl
  * is 7 characters long 
  * is case-insensitive
  * can have 62 characters (a to z, A to Z, 0 to 9),
* the total number of unique combinations is 62 ^ 7 ~= 3.5 trillion
  * If the service handles 1K requests per second, it will take 110 years to exhaust 3.5 trillion
  * If the service handles 1M requests per second, it will take 40 days to exhaust 3.5 trillion
* number of bits required to represent 3.5 trillion = 42 (2 ^ 42 ~= 4.2 trillion) 

### Techniques to generate tinyURL
#### Random
1. on every request, generate a random string of 7 characters.
2. check if the generated string exits in the database.
3. if does not exist, use the generated string as the tinyURL. Otherwise, repeat the process.

Cons: 
  * may not work if multiple services perform step 3 with the same random string.
  
#### Hashing
1. Hash the longUrl using a hashing function, such as MD5 (which produces 128-bit hash). From the hash value, get first 42 bits
2. Follow step 2 and 3 from the Random approach

Pros: compared to Random  approach, this will return the same value for the same longUrl
Cons: same as 1st





#### Persistence Layer


#### References
[https://www.youtube.com/watch?v=fMZMm_0ZhK4](https://www.youtube.com/watch?v=fMZMm_0ZhK4)

[Bitly Architecture](http://highscalability.com/blog/2014/7/14/bitly-lessons-learned-building-a-distributed-system-that-han.html)
