# Remote File Cache

The Remote File Cache is a component of OpenSearch that manages caching of index files for remote storage. This document describes the functionality and usage of the `FileCache` class, which is a key part of the Remote File Cache system.

## Overview

The `FileCache` class is designed to solve the problem of local disk storage limitations when working with remote datasets. It maintains a node-level view of index files with priorities, caching only the index files needed for queries. The cache uses a Least Recently Used (LRU) strategy for file replacement.

## Key Features

- Node-level caching of index files
- LRU-based file replacement strategy
- Reference counting for cached files
- Circuit breaker integration for memory management
- Restoration of cache state from disk

## Usage

The `FileCache` class implements the `RefCountedCache` interface, providing methods for putting, getting, and managing cached index inputs.

### Adding Files to Cache

To add a new file to the cache:

```java
Path filePath = ...;
CachedIndexInput indexInput = ...;
fileCache.put(filePath, indexInput);
```

### Retrieving Files from Cache

To retrieve a file from the cache:

```java
Path filePath = ...;
CachedIndexInput cachedInput = fileCache.get(filePath);
```

### Managing References

The cache uses reference counting to manage the lifecycle of cached files:

```java
// Increase reference count
fileCache.incRef(filePath);

// Decrease reference count
fileCache.decRef(filePath);

// Get current reference count
Integer refCount = fileCache.getRef(filePath);
```

### Removing Files from Cache

To remove a file from the cache:

```java
fileCache.remove(filePath);
```

### Cache Maintenance

The `prune()` method can be used to remove entries with zero reference counts:

```java
long prunedBytes = fileCache.prune();
```

### Cache Statistics

To get current cache statistics:

```java
FileCacheStats stats = fileCache.fileCacheStats();
```

## Circuit Breaker Integration

The `FileCache` integrates with OpenSearch's circuit breaker to prevent out-of-memory errors. If adding an entry to the cache would trip the circuit breaker, a `CircuitBreakingException` is thrown, and the entry is not added to the cache.

## Cache Restoration

The `FileCache` can restore its state from disk on node startup:

```java
List fileCacheDataPaths = ...;
fileCache.restoreFromDirectory(fileCacheDataPaths);
```

This method scans the specified directories for cached files and adds them to the in-memory cache.

## Important Considerations

- The `FileCache` is designed to work with remote storage systems and should be used in conjunction with other components of the remote store functionality in OpenSearch.
- Proper management of reference counts is crucial for efficient cache operation. Always ensure that `decRef()` is called when a cached file is no longer needed.
- The cache capacity and circuit breaker settings should be tuned based on the specific requirements and available resources of your OpenSearch deployment.

For more detailed information on the remote store functionality and its configuration, please refer to the OpenSearch documentation on remote stores and index modules.