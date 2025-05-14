# FileCache

## Overview

The FileCache is a component introduced in OpenSearch to solve the problem of local disk capacity limitations when dealing with remote store datasets. It maintains a node-level view of index files with priorities, caching only those index files needed by queries. The file with the lowest priority (Least Recently Used) in the FileCache is replaced first when the cache reaches its capacity.

## Key Features

- Maintains a node-level view of index files with priorities
- Caches only the index files needed by queries
- Implements a Least Recently Used (LRU) eviction policy
- Provides thread-safe operations for adding, retrieving, and removing cached items

## Main Interfaces

The two main interfaces of FileCache are:

1. `put`: When a new file index input is added to the file cache, it is placed at the cache head, giving it the highest priority.
2. `get`: This function does not add a file to the cache but promotes the priority of a given file by making it the most recently used.

## Important Methods

### `put(Path filePath, CachedIndexInput indexInput)`

Adds a new index input to the cache.

```java
public CachedIndexInput put(Path filePath, CachedIndexInput indexInput) {
    CachedIndexInput cachedIndexInput = theCache.put(filePath, indexInput);
    checkParentBreaker(filePath);
    return cachedIndexInput;
}
```

### `get(Path filePath)`

Retrieves an index input from the cache and updates its priority.

```java
public CachedIndexInput get(Path filePath) {
    return theCache.get(filePath);
}
```

### `remove(final Path filePath)`

Removes a file from the cache.

```java
public void remove(final Path filePath) {
    theCache.remove(filePath);
}
```

### `prune()`

Removes all cache entries with a reference count of zero, regardless of current capacity.

```java
public long prune() {
    return theCache.prune();
}
```

## Cache Eviction

Once the file cache reaches its capacity, it starts evictions. The eviction process removes file items from the cache tail and triggers a callback to clean up the file from disk. This cleanup process also includes closing the file's descriptor.

## Thread Safety

The FileCache implements thread-safe operations to ensure data consistency in a multi-threaded environment. It uses a `SegmentedCache` internally, which offers concurrent access with less contention.

## Circuit Breaker Integration

The FileCache integrates with OpenSearch's circuit breaker service to prevent out-of-memory errors. When adding new entries to the cache, it checks if the operation would trip the circuit breaker:

```java
private void checkParentBreaker(Path filePath) {
    try {
        circuitBreaker.addEstimateBytesAndMaybeBreak(0, "filecache_entry");
    } catch (CircuitBreakingException ex) {
        theCache.remove(filePath);
        throw new CircuitBreakingException(
            "Unable to create file cache entries",
            ex.getBytesWanted(),
            ex.getByteLimit(),
            ex.getDurability()
        );
    }
}
```

## Usage

The FileCache is typically used in conjunction with remote store operations in OpenSearch. It helps optimize performance by caching frequently accessed index files locally, reducing the need for repeated remote store access.

For more detailed information on how to configure and tune the FileCache for your specific use case, please refer to the OpenSearch configuration documentation.