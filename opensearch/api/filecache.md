# File Cache API

The File Cache API provides methods for managing and retrieving information about entries in the OpenSearch file cache. This API is particularly useful for monitoring and debugging cache usage in OpenSearch clusters.

## Get reference count

Retrieves the reference count for a specific entry in the file cache.

### Endpoints

```
GET /_filecache/getRef
```

### Query parameters

| Parameter | Data type | Description |
|:--- |:--- |:--- |
| key | String | The key (path) of the cache entry to get the reference count for. |

### Example request

The following request gets the reference count for a specific cache entry:

```json
GET /_filecache/getRef?key=/path/to/cached/file
```

### Example response

A successful response returns the reference count as an integer:

```json
{
  "ref_count": 2
}
```

If the key is not found in the cache, the response will be:

```json
{
  "ref_count": null
}
```

### Response body fields

| Field | Data type | Description |
|:--- |:--- |:--- |
| ref_count | Integer or null | The reference count for the specified cache entry. Returns null if the entry is not found. |

## Notes

- The File Cache API is primarily intended for internal use and debugging purposes.
- Excessive use of this API in a production environment may impact performance.
- If you use the Security plugin, make sure you have the appropriate permissions to access this API.

{: .note}
The File Cache API is a low-level API and should be used with caution in production environments.