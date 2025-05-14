# Remote Store Metadata API

The Remote Store Metadata API allows you to fetch metadata about remote store segments and translogs for specific indices and shards. This functionality enhances observability and debugging capabilities for indices backed by remote storage.

## Endpoints

```
GET /_remotestore/metadata/{index}
GET /_remotestore/metadata/{index}/{shard_id}
```

## Path parameters

Parameter | Data type | Description
:--- | :--- | :---
`index` | String | (Required) Name of the index to fetch remote store metadata for.
`shard_id` | String | (Optional) ID of the specific shard to fetch metadata for. If not provided, metadata for all shards of the specified index will be returned.

## Query parameters

Parameter | Data type | Description
:--- | :--- | :---
`local` | Boolean | (Optional) If set to `true`, the request will prefer to execute on the local node only. Defaults to `false`.

## Example request

The following request fetches remote store metadata for the index named 'my-index':

```json
GET /_remotestore/metadata/my-index
```

To fetch metadata for a specific shard (e.g., shard 0) of 'my-index':

```json
GET /_remotestore/metadata/my-index/0
```

## Example response

```json
{
  "indices": {
    "my-index": {
      "shards": {
        "0": [
          {
            "segments": {
              "metadata__1": {
                "uploaded_segments": {
                  "_0.cfe": {
                    "original_name": "_0.cfe",
                    "checksum": "1234567890",
                    "length": 1000
                  },
                  "_0.cfs": {
                    "original_name": "_0.cfs",
                    "checksum": "0987654321",
                    "length": 5000
                  }
                },
                "replication_checkpoint": {
                  "shard_id": "[my-index][0]",
                  "primary_term": 1,
                  "generation": 1,
                  "version": 1,
                  "length": 6000,
                  "codec": "Lucene90",
                  "created_timestamp": 1621234567890
                },
                "generation": 1,
                "primary_term": 1
              }
            },
            "translog": {
              "metadata__1_1_1621234567890_node1_0_1": {
                "primary_term": 1,
                "generation": 1,
                "timestamp": 1621234567890,
                "node_id_hash": "node1",
                "min_translog_gen": 0,
                "min_primary_term": "1",
                "version": "1",
                "content": {
                  "primary_term": 1,
                  "generation": 1,
                  "min_translog_generation": 0,
                  "generation_to_term_mapping": {
                    "0": "1"
                  }
                }
              }
            }
          }
        ]
      }
    }
  },
  "_shards": {
    "total": 1,
    "successful": 1,
    "failed": 0
  }
}
```

## Response body fields

Field | Data type | Description
:--- | :--- | :---
`indices` | Object | Contains metadata for each requested index.
`indices.[index_name].shards` | Object | Contains metadata for each shard of the index.
`indices.[index_name].shards.[shard_id]` | Array | An array of metadata objects for the specified shard.
`segments` | Object | Contains metadata about the segments in the remote store.
`segments.[metadata_file].uploaded_segments` | Object | Details about each uploaded segment file.
`segments.[metadata_file].replication_checkpoint` | Object | Information about the latest replication checkpoint.
`segments.[metadata_file].generation` | Integer | The generation number of the segment metadata.
`segments.[metadata_file].primary_term` | Integer | The primary term associated with the segment metadata.
`translog` | Object | Contains metadata about the translog in the remote store.
`translog.[metadata_file]` | Object | Details about each translog metadata file.
`translog.[metadata_file].content` | Object | The actual content of the translog metadata.
`_shards` | Object | Provides information about the shards that were queried.

## Permissions

If you use the Security plugin, you must have the `cluster:admin/remote_store/metadata` cluster privilege to use this API.

## Notes

- This API is designed to work with indices that have remote store enabled.
- The response structure may vary depending on the number of indices, shards, and the amount of metadata available in the remote store.
- Large indices with many shards may result in a significant amount of metadata being returned. Consider using the `shard_id` parameter to limit the response size when debugging specific shards.

For more information about remote store and its configuration, see the [Remote Store documentation](https://opensearch.org/docs/latest/opensearch/remote-store/).