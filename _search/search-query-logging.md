# Search Query Logging

Search query logging allows you to capture and analyze information about search queries executed on your OpenSearch cluster. This feature can be useful for monitoring, debugging, and optimizing search performance.

## Enabling Search Query Logging

Search query logging is disabled by default. To enable it:

1. Set the `opensearch.search.query.logging.enabled` setting to `true`:

```yaml
opensearch.search.query.logging.enabled: true
```

2. Restart your OpenSearch nodes for the setting to take effect.

## Configuring Logging Settings

You can configure the following settings to control search query logging behavior:

```yaml
opensearch.search.query.logging:
  min_time_to_log: 1s  # Minimum query time to log
  log_slow_queries: true # Log slow queries exceeding min_time_to_log
```

- `min_time_to_log`: Queries that take longer than this threshold will be logged. Default is 1 second.
- `log_slow_queries`: Whether to log slow queries exceeding the `min_time_to_log` threshold. Default is true.

## Log Output

When enabled, search query logs will be written to the OpenSearch log file. A typical log entry looks like:

```
[2023-05-15T10:30:15,123][INFO][o.o.s.SearchQueryLogger] Search query executed: indices=[my-index], query=[{"match":{"title":"OpenSearch"}}], took=[1200ms], hits=[100], total_hits=[1000]
```

The log entry contains the following information:
- Timestamp
- Indices searched  
- Query body
- Query execution time
- Number of hits returned
- Total number of matching documents

## Using Search Query Logs

Search query logs can be used for:

- Identifying slow queries that may need optimization
- Analyzing popular search terms and patterns
- Debugging unexpected search results
- Monitoring overall search performance

You can use log analysis tools or the OpenSearch stack itself to analyze and visualize the logged query data.

## Considerations

- Enabling search query logging may have a small performance impact, especially for high-volume search workloads. 
- Be mindful of any sensitive information that may be contained in search queries when storing or analyzing logs.
- Adjust the `min_time_to_log` setting as needed to capture an appropriate level of detail without generating excessive log volume.