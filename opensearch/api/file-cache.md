# File Cache API

## Introduction

The File Cache API provides methods for managing and interacting with the file cache in OpenSearch. This API allows you to get information about the reference count of entries in the file cache.

## Get Reference Count

Returns the reference count for a specific entry in the file cache.

### Path Parameters

Parameter | Data type | Description
:--- | :--- | :---
`key` | String | The key of the file cache entry.

### Example Request

```json
GET /_nodes/file_cache/getRef/{key}
```

### Example Response

```json
{
  "ref_count": 2
}
```

### Response Fields

Field | Data type | Description
:--- | :--- | :---
`ref_count` | Integer | The current reference count for the specified file cache entry.

## Implementation Details

The File Cache API is implemented in the `FileCache` class. It exposes a new method `getRef(Path key)` which returns the reference count of an entry in the cache. This method is thread-safe and can be called concurrently with other cache operations.

The implementation uses a `RefCountedCache` interface, which is implemented by various cache implementations such as `LRUCache` and `SegmentedCache`. These implementations ensure that reference counting is properly managed across different cache structures.

## Usage Considerations

- The reference count indicates how many consumers are currently using the cache entry. An entry with a reference count of 0 is eligible for eviction from the cache.
- This API is particularly useful for debugging and monitoring cache usage patterns.
- Be cautious when using this API in production environments, as frequent calls may impact performance.

## See Also

- [File Cache Documentation](https://opensearch.org/docs/latest/opensearch/file-cache/)
- [Cache Management in OpenSearch](https://opensearch.org/docs/latest/opensearch/cache-management/)

{: .note}
The File Cache API is an internal API and its behavior may change in future releases. Use it with caution in production environments.