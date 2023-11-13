# Redis

Redis is an in-memory data store that uses key-value structures and is primarily used for caching.
The data is stored in RAM and hence its very fast.

### Redis Data Types

Its a key-value database

- strings (numbers are also stored as strings in redis)
- sets
- hashes
- lists
- sorted sets

### Redis key naming convention

| key    | value                                     |
| ------ | ----------------------------------------- |
| books  | {'book 1', 'book 2'}                      |
| book:1 | {author: 'Antony', title: 'Some title'}   |
| book:2 | {author: 'Tony', title: 'Some new title'} |

### Basic Commands using [Upstash Redis](https://upstash.com/)

**Set item**

```js
const data = await redis.set(key, value);
```

**Get item**

```js
const data = await redis.get(key);
```

**Delete item**

```js
const data = await redis.del(key);
```

**Set multiple items**

```js
const data = await redis.mset({
    key1: value1;
    key2: value2;
    key3: value3;
})
```

**Get multiple items**

```js
const data = await redis.mget(["key1", "key2", "key3"]);
```

**GetRange command**

Used to get the substring of the string value stored at the key.

```js
const data = await redis.getrange(key, startRange, endRange);
```

**SetRange command**

Overwrites part of the string stored at key, starting at the specified offset, for the entire length of value.

```js
const data = await redis.setrange(key, offset, value);
```

**Incr command**

Increments the number stored at key by one. If the key does not exist, it is set to 0 before performing the operation.

```js
const data = await redis.incr(key);
```

**Decr command**

Decrements the number stored at key by one. If the key does not exist, it is set to 0 before performing the operation.

```js
const data = await redis.decr(key);
```

**Incrby command**

Increments the number stored at key by increment. If the key does not exist, it is set to 0 before performing the operation.

```js
const data = await redis.incrby(key, value);
```

### Optional Commands

Certain Redis commands can accept additional options

The SET command accepts EX(Set the specified expire time, in seconds), PX(Set the specified expire time, in milliseconds) NX(Only set the key if it does not already exist), XX (Only set the key if it already exists) etc.

```js
const data = await redis.set(key, value, {
  ex: 30,
  nx: true,
});
```

### Sets

A Redis set is an unordered collection of unique strings. You can use Redis sets to efficiently:

- Track unique items (e.g., track all unique IP addresses accessing a given blog post).
- Represent relations (e.g., the set of all users with a given role).
- Perform common set operations such as intersection, unions, and differences.

**Add members to a set**

```js
const data = await redis.sadd(key, member_1, member_2, member_3, ...);
```

**Remove members from a set**

```js
const data = await redis.srem(key, member_1, member_2, member_3, ...);
```

**Test a string for set membership**

```js
const data = await redis.sismember(key, member);
```

**Return the set of members that two or more sets have in common**

```js
const data = await redis.sinter(key_1, key_2, key_3, ...);
```

There are a lot more set commands available [here](https://redis.io/commands/?group=set)

### Lists

Redis lists are linked lists of string values. Redis lists are frequently used to:

- Implement stacks and queues.
- Build queue management for background worker systems.

**Push Items to the list from the left**

```js
const data = await redis.lpush(key, value_1, value_2, ...);
```

**Push Items to the list from the right**

```js
const data = await redis.rpush(key, value_1, value_2, ...);
```

**Remove Items from the list starting from the left**

```js
const data = await redis.lpop(key, count?);
```

**Remove Items from the list starting from the right**

```js
const data = await redis.rpop(key, count?);
```

**Length of a list**

```js
const data = await redis.llen(key);
```

### Hashes

Redis hashes are record types structured as collections of field-value pairs. You can use hashes to represent basic objects and to store groupings of counters, among other things.

**Create/Update a new hash with multiple fields**

```js
const data = await redis.hmset(key, {
  field_1: value_1,
  field_2: value_2,
  ...
});
```

**Get all the fields and their values from a hash**

```js
const data = await redis.hgetall(key);
```

### Sorted Sets

A Redis sorted set is a collection of unique strings (members) ordered by an associated score. When more than one string has the same score, the strings are ordered lexicographically. Some use cases for sorted sets include:

- Leaderboards. For example, you can use sorted sets to easily maintain ordered lists of the highest scores in a massive online game.
- Rate limiters. In particular, you can use a sorted set to build a sliding-window rate limiter to prevent excessive API requests.

**Add items to a Sorted Set**

```js
const data = await redis.zadd(
      key,
      {
        score: score_1,
        member: member_1,
      },
      {
        score: score_2,
        member: member_2,
      }
      ...
    );
```

**Only add new elements. Don't update already existing elements.**

```js
const data = await redis.zadd(
      key,
      {
        nx: true,
      },
      {
        score: score_1,
        member: member_1,
      },
      {
        score: score_2,
        member: member_2,
      }
      ...
    );
```

**Get all the members from a sorted set**

```js
const data = await redis.zrange(key, 0, -1);
```

**Return the number of members in a sorted set**

```js
const data = await redis.zcard(key);
```

### Pipelines

If you want to send a lot of redis requests its better if we create a JavaScript pipeline.

```js
const data = await Promise.all(
  arr.map((item) => {
    return redis.hgetall(`books:${item.id}`);
  })
);
```
