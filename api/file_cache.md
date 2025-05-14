# File Cache

The File Cache (FC) is a component in OpenSearch designed to manage index files on a node level, optimizing storage for remote indices. It addresses the challenge of local disks being unable to hold the entire dataset from a remote store.

## Overview

The File Cache maintains a prioritized view of index files, caching only those needed for queries. It uses a Least Recently Used (LRU) eviction strategy, where files with the lowest priority are replaced first when the cache reaches capacity.

## Key Features

- **Priority-based Caching**: Files are added to the cache head, giving them the highest priority.
- **Dynamic Priority Updates**: The `get` function promotes the priority of a given file by making it the most recently used.
- **Eviction Mechanism**: When the cache reaches capacity, it evicts items from the cache tail and cleans up the file from disk.

## API

The `FileCache` class implements the `RefCountedCache` interface, providing the following key methods:

### put

```java
public CachedIndexInput put(Path filePath, CachedIndexInput indexInput)
```

Adds a new file index input to the cache with the highest priority.

### get

```java
public CachedIndexInput get(Path filePath)
```

Retrieves the corresponding file index input from the cache and updates its priority.

### remove

```java
public void remove(final Path filePath)
```

Removes a file from the cache, even if it's pinned or still in use.

### getRef

```java
public Integer getRef(Path key)
```

Gets the reference count for the specified key.

## Usage

Here's a basic example of how to use the FileCache:

```java
SegmentedCache segmentedCache = // initialize SegmentedCache
CircuitBreaker circuitBreaker = // initialize CircuitBreaker
FileCache fileCache = new FileCache(segmentedCache, circuitBreaker);

// Add a file to the cache
Path filePath = // specify file path
CachedIndexInput indexInput = // create CachedIndexInput
fileCache.put(filePath, indexInput);

// Retrieve a file from the cache
CachedIndexInput cachedInput = fileCache.get(filePath);

// Remove a file from the cache
fileCache.remove(filePath);

// Get reference count for a file
Integer refCount = fileCache.getRef(filePath);
```

## Circuit Breaker Integration

The FileCache integrates with a circuit breaker to prevent excessive memory usage. When adding entries to the cache, it checks the circuit breaker to ensure it doesn't trip:

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

## Restoration from Directory

The FileCache can be restored from a directory, useful for node startup:

```java
public void restoreFromDirectory(List fileCacheDataPaths)
```

This method scans the provided paths, creates `RestoredCachedIndexInput` objects for existing files, and adds them to the cache.

## Statistics

The FileCache provides statistics through the `fileCacheStats()` method:

```java
public FileCacheStats fileCacheStats()
```

This returns a `FileCacheStats` object containing information such as active usage, capacity, total usage, eviction weight, hit count, and miss count.

## Thread Safety

The FileCache utilizes a `SegmentedCache` internally, which provides thread-safe operations through the use of `ReentrantLock` in its underlying `LRUCache` implementation.

## Conclusion

The FileCache is a crucial component for managing remote index files efficiently in OpenSearch. It provides a balance between performance and resource utilization by prioritizing frequently accessed files and evicting less important ones when necessary.