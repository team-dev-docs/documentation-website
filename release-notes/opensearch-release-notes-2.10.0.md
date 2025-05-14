# OpenSearch 2.10.0 Release Notes

## Features

### Segment Replication GA

Segment Replication has reached General Availability (GA) status in this release. This feature enhances data replication and improves cluster performance by replicating index segments directly instead of replicating individual operations.

### Remote Store GA

Remote Store functionality is now Generally Available (GA). This feature allows for storing index data in remote storage systems, improving scalability and data management capabilities.

### Shallow Snapshots

A new feature called Shallow Snapshots has been introduced. This functionality enhances the snapshot and restore process, potentially improving efficiency for certain use cases.

### Custom Codecs

Support for custom codecs has been added, allowing for more flexible data compression and storage options.

## Enhancements

### Remote Store Improvements

- Added new options to the Snapshot API related to Remote Store functionality.
- Introduced Remote Store buffer and index interval settings for performance tuning.
- Implemented Remote Segment and Remote Translog store statistics, accessible via nodes/indices/cluster stats APIs.

### Search Improvements

- Added term frequency functions to score script queries, enhancing relevance scoring capabilities.
- Implemented score normalization and combination features for more flexible result ranking.
- Introduced an 'ext' object in search responses for extended result information.

### Cluster Management

- Added a new cluster setting for shard movement strategy, allowing more control over shard allocation and relocation.

### Security

- Introduced a new permission that provides access to system indexes, enhancing granular access control.

## Bug Fixes and Optimizations

Various bug fixes and performance optimizations have been implemented across different components of OpenSearch.

## Upgrade Notes

Please refer to the [OpenSearch documentation](https://opensearch.org/docs/latest/opensearch/upgrade/) for detailed upgrade instructions.

For a comprehensive list of changes, please refer to the [OpenSearch 2.10.0 changelog](https://github.com/opensearch-project/OpenSearch/blob/main/CHANGELOG.md).

## Conclusion

OpenSearch 2.10.0 brings significant improvements in data replication, storage management, and search capabilities. We encourage users to upgrade to this version to benefit from these enhancements and new features.