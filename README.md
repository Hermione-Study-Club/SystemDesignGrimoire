# Rate Limiter


## Business

* Prevent Dos attack
* Reduce cost
* Prevent server overloaded

## Problems
* Client-side or server-side ? **Server side.**
* Throttle requests based on IP, user Id or others? **Should be flexible for different sets of rules.**
* what's the scale level? **Be able to handle large number of requests.**
* Work in distributed environment? **Yes**
* Implement in seperate service or application code? **Depends on your design.**
* Inform user who are throttled? **Yes**

### Basic requirements

- Total users: **10000/sec**
- Low latency
- Use as little memory as possible: **1M/sec, whcih means 100 byte per user per sec** 
- The rate limiter can be shared across multiple servers or processors
- Show clear exception to throttled user: **return 429**
- High fault tolernce. If rate limiter crashed, it wont affect the entire system.

### Potential issues
- Race condition and sync issues when distributed environment

## Algorithm
- Token bucket
- Leaking bucket
- Fixed window counter
- Sliding window log
- Sliding window counter

## Performance Optimize

## Monitoring
 
## Sources
