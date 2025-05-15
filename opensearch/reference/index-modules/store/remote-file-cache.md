# Remote File Cache

The Remote File Cache is a feature in OpenSearch that manages caching of files for remote store operations. This document describes the functionality and usage of the `FileCache` class, which is part of the remote store implementation.

## Overview

The `FileCache` class is introduced to solve the problem of local disk space limitations when working with remote stores. It maintains a node-level view of index files with priorities, caching only the index files needed by queries. The file with the lowest priority (Least Recently Used) in the FileCache is replaced first.

## Key Features

- Prioritized caching of index files
- Least Recently Used (LRU) eviction policy
- Thread-safe operations

## Main Interfaces

The `FileCache` class provides two main interfaces:

1. `put`: Adds a new file index input to the cache.
2. `get`: Retrieves a file from the cache and updates its priority.

## Usage

### Adding a File to the Cache

To add a file to the cache:

```java
fileCache.put(filePath, indexInput);
```

This operation adds the file at the cache head, giving it the highest priority.

### Retrieving a File from the Cache

To get a file from the cache:

```java
CachedIndexInput indexInput = fileCache.get(filePath);
```

This operation does not add the file to the cache but promotes its priority by making it the most recently used.

## Eviction

When the file cache reaches its capacity, it starts evictions. The eviction process removes file items from the cache tail and triggers a callback to clean up the file from disk. The cleanup process also includes closing the file's descriptor.

## Thread Safety

The `FileCache` class is designed to be thread-safe, allowing concurrent access from multiple threads.

## Additional Methods

- `capacity()`: Returns the maximum capacity of the cache.
- `size()`: Returns the current number of items in the cache.
- `clear()`: Removes all items from the cache.

## Implementation Details

The `FileCache` uses a `SegmentedCache` internally, which provides better concurrency by sharding the cache into multiple segments. This reduces contention when multiple threads are accessing the cache simultaneously.

## Conclusion

The Remote File Cache provides an efficient way to manage index files for remote store operations in OpenSearch. By prioritizing frequently accessed files and evicting least recently used ones, it helps optimize storage usage and improve query performance when working with remote stores.