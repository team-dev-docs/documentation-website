# File Cache API

The File Cache API provides methods for managing and interacting with the file cache in OpenSearch. This document describes the new method added to the `FileCache` class for getting the reference count of an entry.

## Get Reference Count

The `getRef` method has been added to the `FileCache` class to retrieve the reference count of a specific entry in the file cache.

### Method Signature

```java
public Integer getRef(Path key)
```

### Parameters

- `key`: A `Path` object representing the key of the cache entry.

### Return Value

Returns an `Integer` representing the reference count of the specified cache entry. If the entry does not exist, it returns `null`.

### Usage

To get the reference count of a cache entry:

```java
FileCache fileCache = // ... initialize file cache
Path entryKey = // ... specify the key for the cache entry
Integer refCount = fileCache.getRef(entryKey);

if (refCount != null) {
    System.out.println("Reference count for entry: " + refCount);
} else {
    System.out.println("Entry not found in cache.");
}
```

### Implementation Details

The `getRef` method is implemented in the `FileCache` class and delegates to the underlying cache implementation. It uses a `ReentrantLock` to ensure thread-safety when accessing the cache.

```java
@Override
public Integer getRef(Path key) {
    Objects.requireNonNull(key);
    lock.lock();
    try {
        Node node = data.get(key);
        if (node != null) {
            return node.refCount;
        }
        return null;
    } finally {
        lock.unlock();
    }
}
```

This method is useful for debugging and monitoring purposes, allowing users to inspect the current reference count of cache entries.

## Related Classes

The `getRef` method is also implemented in the following classes that implement or extend the file cache functionality:

- `LRUCache`
- `SegmentedCache`

These implementations follow a similar pattern to the `FileCache` implementation, ensuring thread-safety and consistent behavior across different cache types.

## Note

The addition of this method enhances the observability of the file cache system in OpenSearch, providing more detailed information about the usage and state of cached entries.