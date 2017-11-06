## Tiny URL

#### Problem Statement
Design a service that takes in a long url and returns a tiny url, and vice-versa.

#### Discuss
* How wide is the long url? Say 100s characters
* How wide is the tiny url? Say 7 to 10 characters
* Is the tiny url case-sensitive?

#### API
** String getTinyUrl(String longUrl);

** String getLongUrl(String tinyUrl);

#### Architecture
* Consumer sends a getTinyURL request, say based on REST, to the webapp
* Request is forwarded to a load balancer
* Load Balancer forwards the request to one of the tinyURL services
* tinyURL service generates the tinyURL and persists it in the database
* The generated tinyUrl is cached (centralized to all tinyURL services)
* The generated tinyUrl is returned to the consumer

#### Calculations
* Assuming, tinyUrl
- is 7 characters long
- is case-insensitive
- can have 62 characters (a to z, A to Z, 0 to 9)
the total number of unique combinations is 62 ^ 7 ~= 3.5 trillion

* If the service handles 1K requests per second, it will take 110 years to exhaust 3.5 trillion
* If the service handles 1M requests per second, it will take 40 days to exhaust 3.5 trillion


#### Persistence Layer


#### References
[https://www.youtube.com/watch?v=fMZMm_0ZhK4](https://www.youtube.com/watch?v=fMZMm_0ZhK4)

[Bitly Architecture](http://highscalability.com/blog/2014/7/14/bitly-lessons-learned-building-a-distributed-system-that-han.html)
