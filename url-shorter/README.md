# DESIGN A URL SHORTENER

## Design scope

- Write operation: 100 million URLs are generated per day.
- Write operation per second: 100 million / 24 /3600 = 1160
- Read operation: Assuming ratio of read operation to write operation is 10:1, read operation per second: 1160 * 10 = 11,600
- Assuming the URL shortener service will run for 10 years, this means we must support 100 million * 365 * 10 = 365 billion records.
- Assume average URL length is 100.
- Storage requirement over 10 years: 365 billion * 100 bytes * 10 years = 365 TB

## Ideas

### Hash function

- Hash value length
- Hash + collision (If length is too long)
  - MD5 or SHA256, etc
      - Q: length too long, How can we make it shorter?
      - A: collect the first N characters and append predefined string (avoid collision)

### Base conversion

- Depends on a unique ID generator
- There is a security concern

### Key Generation Service (KGS)

Generates random N-letter strings beforehand and stores them in a database.

Pros:

- quite simple and fast
- don't worry about duplications or collisions
- make sure all the keys inserted into DB are unique

crons:

- Concurrency problem: two or more servers try to read the same key from the database
    - two tables to store keys: one for keys that are not used yet, and one for all the used keys.
- Each server can cache some keys in memory
    - If KGS dies before assigning all the loaded keys to some server, we will be wasting those keys
- Single point of failure
    - Replica

## What kind of database should we use?

A NoSQL DB is a better choice:

- store billions of rows
- don’t need to use relationships between objects

## Data Partitioning

- Range Based Partitioning
    - lead to unbalanced DB servers
- Hash-Based Partitioning
    - lead to overloaded partitions, which can be solved using Consistent Hashing

## Cache

### Which cache eviction policy would best fit our needs?

Least Recently Used (LRU) 

## Lazy cleanup

Should entries stick around forever, or should they be purged? If a user-specified expiration time is reached, what should happen to the link?

- default expiration time
- Whenever a user tries to access an expired link, delete it and return an error
- After removing an expired link, we can put the key back in the key-DB to be reused.

## Telemetry

- country of the visitor
- date and time of access
- web page that referred the click, browser, or platform from where the page was accessed.

## Refs

- [Designing a URL Shortening service like TinyURL](https://www.educative.io/courses/grokking-the-system-design-interview/m2ygV4E81AR)
