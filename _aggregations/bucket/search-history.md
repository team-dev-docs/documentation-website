---
layout: default
title: Search history
parent: Bucket aggregations
nav_order: 165
---

# Search history aggregations

The search history aggregation allows you to analyze and aggregate data about users' search history. This aggregation works with the Search History plugin to provide insights into search patterns and behavior.

## Usage

To use the search history aggregation, you need to have the Search History plugin installed and configured. The aggregation can be performed on the `.search_history` index that is automatically created by the plugin.

Here's a basic example of using the search history aggregation:

```json
GET .search_history/_search
{
  "size": 0,
  "aggs": {
    "search_history": {
      "terms": {
        "field": "query.keyword",
        "size": 10
      }
    }
  }
}
```

This aggregation will return the top 10 most frequent search queries.

## Parameters

The search history aggregation supports the following parameters:

- `field`: The field to aggregate on. Common fields include `query.keyword`, `timestamp`, `user_id`, etc.
- `size`: The number of buckets to return.
- `order`: How to order the buckets. Can be based on count, a metric, or other criteria.

## Examples

### Aggregating by user

To see which users have performed the most searches:

```json
GET .search_history/_search
{
  "size": 0,
  "aggs": {
    "top_users": {
      "terms": {
        "field": "user_id",
        "size": 5
      }
    }
  }
}
```

### Aggregating by time

To see the distribution of searches over time:

```json
GET .search_history/_search
{
  "size": 0,
  "aggs": {
    "searches_over_time": {
      "date_histogram": {
        "field": "timestamp",
        "calendar_interval": "day"
      }
    }
  }
}
```

### Combining with other aggregations

You can combine the search history aggregation with other aggregations for more complex analysis:

```json
GET .search_history/_search
{
  "size": 0,
  "aggs": {
    "top_queries": {
      "terms": {
        "field": "query.keyword",
        "size": 5
      },
      "aggs": {
        "hits_stats": {
          "stats": {
            "field": "hit_count"
          }
        }
      }
    }
  }
}
```

This will return the top 5 queries along with statistics about the number of hits for each query.

## Security considerations

The search history contains sensitive information about user queries. Make sure to properly secure the `.search_history` index and limit access to authorized users only. You can use the Security plugin to set up role-based access control for the search history data.