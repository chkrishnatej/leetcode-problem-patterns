---
description: Token bucket is a widely used rate limiter algorithm
---

# Token Bucket - Rate Limiter Algo

Suppose you are building an API for a service which could be exposed publicly and there could be huge number of users that would be consuming this API. You would wanted to rate limit the usage of the APIs for multiple reasons.

1. Preventing the system from DoS attack
2. Limiting the usage based on the selected plan by the tenant.
3. Throttling of requests for performance reasons.

**Rate limiting logic**

The token bucket algorithm is based on an analogy of a fixed capacity bucket into which tokens are added at a fixed rate by the refiller at constant interval of times. Before allowing an API to proceed, the bucket is inspected to see if it contains at least 1 token at that time. If so, one token is removed from the bucket, the API is allowed to proceed. In case there are no tokens available, the API is returned saying that the quota has exceeded during that window!

&#x20;                                          ![](<.gitbook/assets/image (1) (1) (1) (1).png>)

In order scale this in a distributed enviroment way, use the bucket tokens by persisting them in cache like redis.
