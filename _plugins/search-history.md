---
layout: default
title: Search history
nav_order: 140
has_children: false
parent: Plugins
---

# Search history plugin

The search history plugin allows you to save and retrieve a user's search history in OpenSearch. This can be useful for enabling features like recent searches or personalized search suggestions.

## Installation 

The search history plugin is included with OpenSearch by default. No additional installation steps are required.

## Configuration

The plugin has two main configuration settings that can be adjusted in `opensearch.yml`:

```yaml
search.history.max_size: 100  # Maximum number of searches to store per user
search.history.retention_days: 30  # Number of days to retain search history
```

## Usage

The plugin provides REST APIs for saving and retrieving search history:

### Save search history

```
POST /_search_history
{
  "query": "my search query",
  "indices": ["index1", "index2"],
  "hit_count": 53
}
```

This saves the search query, indices searched, and number of hits to the user's search history.

### Retrieve search history  

```
GET /_search_history?from=0&size=20
```

This retrieves the user's search history, paginated by the `from` and `size` parameters.

### Delete search history

```
DELETE /_search_history?query=my+search+query
```

This deletes matching entries from the user's search history.

## Security

The plugin uses the authenticated user's ID to segregate and secure search history data. Users can only access their own search history.

## Limitations

- The plugin currently stores search history data in-memory and is not designed for production use with large volumes of users/searches.
- Search history is not persisted across cluster restarts.

For more details on the plugin implementation, see the [SearchHistoryPlugin](https://github.com/opensearch-project/OpenSearch/blob/main/plugins/search-history/src/main/java/org/opensearch/plugin/searchhistory/SearchHistoryPlugin.java) and [SearchHistoryService](https://github.com/opensearch-project/OpenSearch/blob/main/plugins/search-history/src/main/java/org/opensearch/plugin/searchhistory/SearchHistoryService.java) classes.