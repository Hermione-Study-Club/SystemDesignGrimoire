# Unique ID Generator 共筆

## Multi master replication

* 利用 db `auto_increment` 建立
* 不易 scale


multi server 效率很慢，因為每一台 master 寫入都會需要 sync

* TODO: 改進方式?



## Clock sync
NTP(Network Time Protocol) 看怎麼定義，可能造成時間倒退

Google True Time API


## Instagram unique id generator

 - 如何做分散流量?
 - 如何做 HA?
 >

> `mikeyk` on Sept 30, 2011
> Thanks for the comment! We don't need the IDs to be exactly sortable, only roughly sortable within a second or so. As long as the clock doesn't move backwards on any given machine (we use ntpd in its gradual-adjustment mode), the IDs are unique.
> The way we move shards is to use PostgreSQL's built-in streaming replication to create an exact, in-sync copy of a set of tablespaces, then 'fail over' to a new machine and start reading/writing to a subset of those tablespaces (this is similar to how Facebook describes their shard-moving process).


### Source
* https://instagram-engineering.com/sharding-ids-at-instagram-1cf5a71e5a5c
* https://news.ycombinator.com/item?id=3058327

## distributed unique id generator

- id 64 bit 主因不只有資料庫 (index 太長，速度更慢?)
- uuid 128 bit 太長存取空間變大速度太慢，沒有時間順序性
- ticket server 在流量不大的時候，是適合使用的方式
- MySQL 比 Postgres 還要早加入 auto increment
- multi master replica 的概念，主要需要給機器初始值和遞增量(靜態)，(動態增加處理比較複雜?)
- 大部分 id generator 都是由 segment pattern and snowflake 變形而來

- 雙 11 Bonus:
不是先 insert data into db，而是先預估各區域商品數量，在 DB 開出相應的數量的 row 與 ID，在交易清算過後才結算資料，所以不用 sync

- Redis cluster 在 master slave 的架構下，persistent 會有問題?

- 時間順序性，基本上看需求
