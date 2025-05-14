# Remote Store Module

## Introduction

The Remote Store module in OpenSearch provides functionality for managing a remote file cache. This module is essential for operations involving remote storage and caching of index data.

## FileCache

The `FileCache` class is a key component of the Remote Store module. It implements the `RefCountedCache` interface and provides methods for managing cached files.

### Key Methods

#### getRef

The `getRef` method has been added to the `FileCache` class to retrieve the reference count for a given key (file path).

```java
@Override
public Integer getRef(Path key) {
    return theCache.getRef(key);
}
```

This method delegates to the underlying cache implementation to get the reference count for the specified key.

## RefCountedCache Interface

The `RefCountedCache` interface has been updated to include the `getRef` method:

```java
/**
 * Get the reference count for key {@code key}.
 */
Integer getRef(K key);
```

This method is implemented by various cache implementations, including `LRUCache` and `SegmentedCache`.

## LRUCache Implementation

The `LRUCache` class implements the `getRef` method as follows:

```java
@Override
public Integer getRef(K key) {
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

This implementation ensures thread-safety by using a lock and returns the reference count of the specified key if it exists in the cache.

## SegmentedCache Implementation

The `SegmentedCache` class implements the `getRef` method as follows:

```java
@Override
public Integer getRef(K key) {
    if (key == null) throw new NullPointerException();
    return segmentFor(key).getRef(key);
}
```

This implementation delegates to the appropriate segment of the cache to retrieve the reference count.

## Usage

The `getRef` method can be used to check the reference count of cached items. This is particularly useful for debugging and managing cache entries. For example:

```java
FileCache fileCache = // initialize FileCache
Path filePath = // specify file path
Integer refCount = fileCache.getRef(filePath);
if (refCount != null) {
    System.out.println("Reference count for " + filePath + ": " + refCount);
} else {
    System.out.println("File not found in cache: " + filePath);
}
```

## Testing

The implementation includes comprehensive tests to ensure the correct functionality of the `getRef` method across different cache implementations. These tests cover various scenarios, including:

- Checking reference counts after adding items to the cache
- Verifying reference counts after incrementing and decrementing
- Ensuring correct behavior when querying non-existent items

For detailed test cases, refer to the `FileCacheTests` and `RefCountedCacheTestCase` classes in the test package.

## Conclusion

The addition of the `getRef` method to the Remote Store module enhances the ability to manage and monitor cached files. This feature provides developers with more control and insight into the state of the file cache, which is crucial for optimizing performance and troubleshooting in OpenSearch deployments that utilize remote storage.