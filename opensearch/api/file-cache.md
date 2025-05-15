# File Cache API

## Introduction

The File Cache API provides functionality for managing and interacting with the file cache in OpenSearch. This API allows you to retrieve information about the reference count of entries in the file cache.

## Get Reference Count

Retrieves the reference count of an entry in the file cache.

### Path Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| `key` | Path | The key of the file cache entry |

### Request

```
GET /_file_cache/{key}/ref
```

### Response

The response contains the reference count of the specified file cache entry.

```json
{
  "ref_count": 2
}
```

- `ref_count`: The current reference count of the file cache entry.

## Usage

This API is primarily used internally by OpenSearch to manage the file cache. It allows the system to track how many references exist for a particular file in the cache, which is crucial for efficient memory management and cache eviction strategies.

## Implementation Details

The File Cache API is implemented in the `FileCache` class. The `getRef` method has been added to expose the reference count of a specific entry:

```java
@Override
public Integer getRef(Path key) {
    return theCache.getRef(key);
}
```

This method delegates to the underlying cache implementation to retrieve the reference count for the given key.

## Notes

- This API is part of OpenSearch's internal operations and is not typically used directly by end-users.
- The reference count indicates how many active users or processes are currently accessing the file in the cache.
- A reference count of zero typically means the file is eligible for eviction from the cache, subject to the cache's eviction policies.

For more detailed information about the file cache implementation and its role in OpenSearch, please refer to the `FileCache` and related classes in the OpenSearch codebase.