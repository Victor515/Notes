[TOC]

# System Design Flow (Twitter as an example)

https://youtu.be/PMCdWr6ejpw

## Step 1: Clarify the requirements

### Type 1 Functional Requirements

### Type 2 Non-Functional Requirements

## Step 2: Capacity Estimation

### Storage Estimate

### Bandwidth Estimate

## Step 3: System APIs

## Step 4: High-level System Design

case by case

## Step 5: Data Storage

Storage solution comparison

## Step 6: Scalability

- Identify potential bottlenecks
- Discussion solutions, focusing on tradeoffs
  - Data sharing
    - data store, cache
  - Load balancing
    - user<->application server; application server <->cache server; application server <-> db
  - Data caching
    - Read heavy

### Sharding

Why?

* Impossible to store/process all data in a single machine

How?

- Break large tables into smaller shards on multiple servers

Pros

- Horizontal scaling

Cons

- Complexity (distributed query, resharding)



### Tweet table as an example

#### Option 1: shard by tweet's creation time

Pros: 

- Limited shards to query

Cons:

- **Hot/cold data** issue
- New shards fill up quickly

#### Option 2: Shard by hash(userId): store all the data of a user on a single shard

Pros:

- Simple
- Query user timeline is straightforward

Cons:

- Home timeline still needs to query multiple shards
- Non-uniform distribution of storage
  - User data might not be able to fit into a single shard
- Hot users

#### Option 3: Shard by hash(tweetId)

Pros:

- Uniform distribution
- High availability

Cons:

- Need to query all shards in order to generate user/home timeline



### Caching

Why?

- Read heavy
- Distributed queries can be slow and costly

How

- Store hot/precomputed data in memory, reads can be much faster

Timeline service (tweet example)

- User timeline: user_id -> {tweet_id} # varies a lot, T = 1k~100k, Trump: ~60k
- Home timeline: user_id -> {tweet_id} # this can be huge, sum(followee' tweets)
- Tweets: tweet_id -> tweet  # common data that can be shared

Topics

- Caching policy
- Sharding in cache
- Performance