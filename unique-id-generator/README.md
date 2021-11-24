# DESIGN A UNIQUE ID GENERATOR IN DISTRIBUTED SYSTEMS


## Design scope

* IDs must be unique
* IDs are numerical values only
* IDs fit into 64-bit
* IDs are ordered by date
* Ability to generate over 10000 unique IDs per second

## Ideas

### Using UUID?

Very simple solution. But UUID is 128 bits long, which we need is 64 bits. And UUID also include non-numeric

### Ticket Server

* __Single point of failure__ if there is only one server.
* Set up multiple ticket servers will faces challenges about data synchronization

Case study: [Flicker - Ticket Servers: Distributed Unique Primary Keys on the Cheap
](https://code.flickr.net/2010/02/08/ticket-servers-distributed-unique-primary-keys-on-the-cheap/)


### Twitter snowflake approach

How twitter generate unique ID numbers for tweets?

[Announching Snowflake](https://blog.twitter.com/engineering/en_us/a/2010/announcing-snowflake)

* Divide ID into different sections, here is the layout of a 64-bit ID.
    * Sign bit: 1 bit
    * Timestamp: 41 bits
    * Datacenter ID: 5 bits
    * Machine ID: 5 bits
    * Sequence number: 12 bits

* IDs are sorted by time because of Timestamp.
