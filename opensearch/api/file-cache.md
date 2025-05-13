# File Cache API

The File Cache API provides functionality for managing and interacting with the OpenSearch file cache system. This API allows you to retrieve reference counts for cached files and manage the lifecycle of cached entries.

## Overview

The File Cache is an essential component in OpenSearch that helps improve performance by caching frequently accessed files. This API exposes methods to interact with the cache, primarily focusing on reference counting.

## Key Components

### FileCache

The `FileCache` class is the main interface for interacting with the file cache. It implements the `RefCountedCache` interface and provides methods for managing cached entries.

### RefCountedCache

The `RefCountedCache` interface defines the contract for a reference-counted cache implementation. It includes methods for incrementing, decrementing, and retrieving reference counts.

## API Methods

### getRef

The `getRef` method allows you to retrieve the reference count for a specific key in the cache.

Signature:
```java
Integer getRef(Path key)
```

Parameters:
- `key`: The `Path` object representing the key in the cache.

Returns:
- An `Integer` representing the reference count for the given key, or `null` if the key is not present in the cache.

Example usage:
```java
FileCache fileCache = // initialize FileCache
Path filePath = // path to the file
Integer refCount = fileCache.getRef(filePath);
if (refCount != null) {
    System.out.println("Reference count for " + filePath + ": " + refCount);
} else {
    System.out.println("File not found in cache: " + filePath);
}
```

### incRef

Increments the reference count for a given key in the cache.

### decRef

Decrements the reference count for a given key in the cache.

### prune

Removes all cache entries with a reference count of zero, regardless of current capacity.

## Implementation Details

The File Cache API is implemented using a combination of classes:

1. `FileCache`: The main class that implements the `RefCountedCache` interface.
2. `LRUCache`: An implementation of the Least Recently Used (LRU) cache algorithm.
3. `SegmentedCache`: A cache implementation that divides the cache into segments for improved concurrency.

These implementations ensure thread-safety and efficient management of cached entries.

## Best Practices

1. Always check if a key exists in the cache before performing operations on it.
2. Use the `incRef` and `decRef` methods to properly manage the lifecycle of cached entries.
3. Periodically call the `prune` method to remove unused entries and free up cache space.

## Related Documentation

- [OpenSearch Developer Guide](https://opensearch.org/docs/latest/opensearch/developer-guide/)
- [OpenSearch Performance Tuning](https://opensearch.org/docs/latest/opensearch/performance/)