# Cache notes for a small service

These notes describe this service's cache design, not a universal prescription.

## Reuse

A cache keeps a computed result so the service can reuse it for the same request instead of computing it again.

## Keys

A cache key must include every request parameter that changes the result. In this service, a book's price depends on the book ID and sales region, so the price key includes both values.

## Hits and misses

A hit means the requested result is already in the cache. A miss means it is absent and must be obtained from the underlying source.

## Expiration

TTL sets how long an entry may remain in this cache. A change in the underlying database does not, by itself, expire that entry before its TTL ends.

## Writes

This service first completes a database write and then deletes the corresponding cache entry. If deletion fails, an old cached value may remain until it expires. This design therefore permits a period of stale reads; it does not promise immediate consistency.

## Capacity

When this cache reaches capacity, its LRU policy evicts the entry that has gone unused for the longest time. Expiration and capacity eviction are different removal mechanisms.

## Measurement

The hit ratio is cache hits divided by all cache requests. In one measurement, 80 hits among 100 requests gave a hit ratio of 80%. That ratio alone does not show how much time the cache saved.
