# FileCache

The `FileCache` class is part of OpenSearch's remote store functionality, providing a caching mechanism for remote files. It implements the `RefCountedCache` interface for `Path` keys and `IndexInput` values.

## Overview

`FileCache` is designed to cache `IndexInput` objects associated with file paths. It utilizes a underlying cache implementation to manage the stored entries and their reference counts.

## Key Features

1. Reference Counting: Maintains a count of references for each cached entry.
2. Thread-Safety: Ensures thread-safe operations on the cache.
3. Customizable Cache Implementation: Uses a configurable underlying cache (default is `LRUCache`).

## Methods

### Constructor

```java
public FileCache(Settings settings, String cacheName)
```

Initializes a new `FileCache` instance with the given settings and cache name.

### put

```java
public IndexInput put(Path key, IndexInput indexInput)
```

Adds a new entry to the cache or returns an existing one if the key is already present.

### get

```java
public IndexInput get(Path key)
```

Retrieves the `IndexInput` associated with the given key.

### remove

```java
public void remove(Path key)
```

Removes the entry associated with the given key from the cache.

### incRef

```java
public void incRef(Path key)
```

Increments the reference count for the entry associated with the given key.

### decRef

```java
public void decRef(Path key)
```

Decrements the reference count for the entry associated with the given key.

### getRef

```java
public Integer getRef(Path key)
```

Retrieves the current reference count for the entry associated with the given key.

### prune

```java
public long prune()
```

Removes all cache entries with a reference count of zero.

## Usage Example

```java
Settings settings = Settings.builder().build();
FileCache fileCache = new FileCache(settings, "myCache");

Path filePath = Paths.get("example.txt");
IndexInput indexInput = // create IndexInput
fileCache.put(filePath, indexInput);

IndexInput cachedInput = fileCache.get(filePath);
fileCache.incRef(filePath);

// Use cachedInput...

fileCache.decRef(filePath);
Integer refCount = fileCache.getRef(filePath);
System.out.println("Reference count: " + refCount);

fileCache.remove(filePath);
```

## Thread Safety

The `FileCache` class is designed to be thread-safe. It delegates thread safety concerns to the underlying cache implementation, which is typically an instance of `LRUCache` that provides its own synchronization mechanisms.

## Performance Considerations

- The `getRef` method allows for efficient checking of an entry's reference count without modifying it.
- The `prune` method can be used to remove unused entries and free up memory.
- Proper use of `incRef` and `decRef` is crucial for managing the lifecycle of cached entries.

## Note

This documentation is based on the implementation as of OpenSearch 2.19.0. Refer to the latest source code and official OpenSearch documentation for the most up-to-date information.