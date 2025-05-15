# File Cache

## Introduction

The File Cache is a feature introduced to manage and optimize the caching of index files in OpenSearch. This documentation covers the methods and functionality related to the File Cache, as implemented in the `IndexShard` class.

## getRef Method

The `getRef` method has been added to the `FileCache` interface and implemented in relevant classes. This method allows retrieving the reference count for a given key in the file cache.

### Syntax

```java
public Integer getRef(Path key)
```

### Parameters

- `key`: A `Path` object representing the key for which to retrieve the reference count.

### Return Value

Returns an `Integer` representing the reference count for the given key. If the key is not found in the cache, it returns `null`.

## Implementation Details

The `getRef` method has been implemented in the following classes:

1. `FileCache`
2. `LRUCache`
3. `SegmentedCache`

### FileCache Implementation

In the `FileCache` class, the `getRef` method delegates to the underlying cache implementation:

```java
@Override
public Integer getRef(Path key) {
    return theCache.getRef(key);
}
```

### LRUCache Implementation

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

This implementation ensures thread-safety by using a lock when accessing the cache data.

### SegmentedCache Implementation

The `SegmentedCache` class implements the `getRef` method by delegating to the appropriate segment:

```java
@Override
public Integer getRef(K key) {
    if (key == null) throw new NullPointerException();
    return segmentFor(key).getRef(key);
}
```

## Usage

The `getRef` method can be used to check the reference count of a specific key in the file cache. This can be useful for debugging, monitoring, or implementing cache management strategies.

Example usage:

```java
FileCache fileCache = // ... initialize file cache
Path key = // ... path to the file
Integer refCount = fileCache.getRef(key);
if (refCount != null) {
    System.out.println("Reference count for " + key + ": " + refCount);
} else {
    System.out.println("Key not found in cache: " + key);
}
```

## Considerations

- The `getRef` method returns `null` if the key is not found in the cache, so null checks should be performed when using the returned value.
- This method is thread-safe, but the reference count may change immediately after the call if other threads are modifying the cache concurrently.
- The implementation in `SegmentedCache` throws a `NullPointerException` if the key is null, which differs from the `LRUCache` implementation that uses `Objects.requireNonNull(key)`.

By exposing the reference count information, the File Cache system provides more flexibility and control over cache management in OpenSearch.