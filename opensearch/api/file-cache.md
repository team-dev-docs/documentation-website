# File Cache

The File Cache is a component in OpenSearch that manages caching of files, providing methods to interact with and monitor the cache.

## Overview

The File Cache is implemented in the `FileCache` class, which is part of the `org.opensearch.index.store.remote.filecache` package. It provides functionality to manage cached index files, including operations like adding, retrieving, and removing files from the cache.

## Key Features

1. Reference Counting: The File Cache implements a reference counting mechanism to track the usage of cached files.
2. Cache Statistics: It provides methods to retrieve various statistics about the cache usage.
3. Thread-safe Operations: The cache operations are designed to be thread-safe for concurrent access.

## Usage

### Getting Reference Count

A new method `getRef` has been added to the `FileCache` class to retrieve the reference count of a cached file:

```java
public Integer getRef(Path key) {
    return theCache.getRef(key);
}
```

This method returns the current reference count for a given file path in the cache. If the file is not in the cache, it returns `null`.

### Example

Here's an example of how you might use the `getRef` method:

```java
FileCache fileCache = // ... initialize file cache
Path filePath = // ... path to the file
Integer refCount = fileCache.getRef(filePath);
if (refCount != null) {
    System.out.println("Reference count for " + filePath + ": " + refCount);
} else {
    System.out.println("File " + filePath + " is not in the cache.");
}
```

## Implementation Details

The `FileCache` class delegates most of its operations to an internal cache implementation (`theCache`). The new `getRef` method simply forwards the call to the internal cache:

```java
@Override
public Integer getRef(Path key) {
    return theCache.getRef(key);
}
```

This method is part of the `RefCountedCache` interface, which the internal cache implements.

## Testing

The implementation includes unit tests to verify the correct behavior of the new `getRef` method. These tests check various scenarios, including:

1. Getting the reference count of a file that's in the cache
2. Getting the reference count of a file that's not in the cache
3. Verifying that the reference count increases when a file is added or accessed
4. Verifying that the reference count decreases when a file is released

## Conclusion

The addition of the `getRef` method to the `FileCache` class enhances the ability to monitor and manage cached files in OpenSearch. This feature can be particularly useful for debugging, performance monitoring, and implementing more sophisticated cache management strategies.