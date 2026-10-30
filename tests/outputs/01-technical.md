# Knowledge cards: Cache notes for a small service

## Card 1: Reuse computed results

**Core knowledge**: A cache keeps a computed result for reuse when the same request occurs again.

**Brief explanation**: In this service, reusing that result avoids computing it again for the same request.

**Self-test**: What work can be avoided when the same request reuses a cached result?

**Answer**: Computing the result again.

## Card 2: Include result-changing parameters in the key

**Core knowledge**: A cache key must include every request parameter that changes the result.

**Brief explanation**: The service's key must reflect both inputs to its price result, rather than identifying only the book.

**Example**: The book-price key includes both the book ID and the sales region because the price depends on both.

## Card 3: Distinguish a hit from a miss

**Core knowledge**: A hit finds the requested result in the cache; a miss requires obtaining it from the underlying source.

**Brief explanation**: The distinction is whether the requested result is already present in the cache.

**Self-test**: If the requested result is absent from the cache, is it a hit or a miss?

**Answer**: A miss; the result must be obtained from the underlying source.

## Card 4: TTL does not immediately reflect database changes

**Core knowledge**: TTL limits how long an entry may remain, but a database change alone does not expire it early.

**Brief explanation**: In this cache, changing the underlying value and expiring the cached entry are separate events.

**Self-test**: Does a database change, by itself, expire an entry before its TTL ends?

**Answer**: No.

## Card 5: Write-then-delete permits stale reads

**Core knowledge**: This service writes to the database before deleting the corresponding cache entry, and this design permits a period of stale reads.

**Brief explanation**: If deletion fails, an old cached value may remain until expiration, so the design does not promise immediate consistency.

**Self-test**: What may happen if the database write completes but cache deletion fails?

**Answer**: An old cached value may remain until it expires.

## Card 6: LRU removes an entry when capacity is reached

**Core knowledge**: At capacity, this cache's LRU policy evicts the entry that has gone unused for the longest time.

**Brief explanation**: Capacity eviction and expiration are different ways an entry can be removed.

**Self-test**: Which entry does this cache's LRU policy evict at capacity?

**Answer**: The entry that has gone unused for the longest time.

## Card 7: Interpret the hit ratio within its limits

**Core knowledge**: The hit ratio is cache hits divided by all cache requests, but it alone does not show how much time was saved.

**Brief explanation**: The ratio measures the share of requests that were hits, rather than the time saved by caching.

**Example**: The notes report 80 hits among 100 requests, giving a hit ratio of 80%.
