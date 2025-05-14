# OpenSearch Documentation Release Notes 2.19.0

## New Features

### File Cache Reference Count Retrieval

A new method has been exposed to retrieve the reference count of an entry in the FileCache. This feature allows for better management and monitoring of file cache entries.

#### Changes in FileCache.java

The `FileCache` class has been updated with a new method:

```java
@Override
public Integer getRef(Path key) {
    return theCache.getRef(key);
}
```

This method allows retrieving the reference count for a given key in the file cache.

#### Changes in LRUCache.java

The `LRUCache` class now implements the `getRef` method:

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

This implementation provides thread-safe access to the reference count of a cache entry.

#### Changes in RefCountedCache.java

The `RefCountedCache` interface has been updated to include the `getRef` method:

```java
/**
 * get the reference count for key {@code key}.
 */
Integer getRef(K key);
```

#### Changes in SegmentedCache.java

The `SegmentedCache` class now implements the `getRef` method:

```java
@Override
public Integer getRef(K key) {
    if (key == null) throw new NullPointerException();
    return segmentFor(key).getRef(key);
}
```

This implementation delegates the reference count retrieval to the appropriate cache segment.

## Impact and Usage

This new feature allows developers and system administrators to:

1. Monitor the usage of specific entries in the file cache
2. Implement more sophisticated cache management strategies
3. Debug potential issues related to file cache reference counting

To use this new functionality, call the `getRef` method on a `FileCache` instance with the desired key:

```java
Integer refCount = fileCache.getRef(path);
```

This will return the current reference count for the specified path in the file cache, or `null` if the entry doesn't exist.

## Compatibility

This change is backwards compatible and does not affect existing functionality. It only adds a new method to retrieve information that was previously not accessible.

## Testing

The pull request includes additional test cases to verify the correct implementation of the new `getRef` method across various cache implementations.

For more detailed information, please refer to the [pull request #18259](https://github.com/opensearch-project/OpenSearch/pull/18259) in the OpenSearch repository.
