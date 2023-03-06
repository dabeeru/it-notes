Redis is in-momory data structure store which can be used as:
- database
- cache
- message broker
- streaming engine

# Data structures
- strings
- hashes
- lists
- sets
- sorted sets
- bitmpas
- hyperloglogs
- geospatial idnexes
- streams

# Replication
> **WARNING**: This topic is much more complicated - refer to [docs](https://redis.io/docs/management/replication/) for further information
- Master and replicas
- Replicas are exact copy of master
- Replicas reconnecting to the master automatically

## Synchronization
- When connection between master and replicais established, master is pushing data (in case of client writes, keys expired etc.) to the replica
- If connection break, replica is reconnecting to the master and asks for partial synchronization
- If partial sychroniation is not possible then replica asks for full resynchronization which includes master sending full snapshot of the data
- Synchronization is asynchronou by deafult but can be changed to synchronous
- Replicas can be connected to replicas and data can be synchronized in cascadde manner
- Synchroniation is non-blocking - replica will return the old version of the data if not yet synchronized new one

# Key eviction
- Key evection is about removing old data when new arrives. 
- *LRU* (*last recently used*) is a popular eviction algorithm.
- When memory is reached Redis starts evicting old items
- The amount of memory used by redis can be set in `redis.conf` with `maxmemory` attribute
- LRU algorithm is approximated which means that is not checking all of the items but only sampling sample of them and picking the best entires from the sample
- 
## Eviction policies
- `noeviction` - new items are not saved after memory is reached
- `allkeys-lru` - keep most recently used keys, removes least recently used
- `allkeys-lfu` - keep frequently used keys, removes least frequently used keys
- `volatile-lru` - removes least recently used with expire field set to true
- `volatile-lfu` - removes least frequently used with expire set to true
- `allkeys-random` - randomly removes keys
- `volatile-random` - randomly removes keys with expire set to true
- `volatile-ttl` - removes expire field set to true and the shortest remaining TTL

Voliatile configurations works like noeviction if there is no key with expire set to true

# Redis Cluster
- Horizontally scaled
- Increases availability

## Consistency guarantees
- No strong consistency guarantees
- Asynchronous synchronization with replicas might result in data loss if master will fail before replicating data to replica
- In Redis most of data is written in memory so in case of node failue this data is lost if not synchronized with other node

## Sharding
- Every master node is responsible for subset of hashes
- Removing and adding nodes require no downtime

## Replicas
- Every hash slot can be saved on multiple replica nodes
- If master node mails its replica is promoted to master role


# Redis Sentinel
- High availability without *Redis Cluster* 
- Additional capabilities
	- Monitoring
	- Notification
	- Automatic failover
	- Configuration provider
	- Clients service discovery

# Other capabilitites
- Lua scripting
- Transactions
- on-disk persistence
	- perodically dumpin dataset to disk
	- appending each command to a disk based logs
- Pub/Sub
- Data retention
- Automatic failover
- Client libraries