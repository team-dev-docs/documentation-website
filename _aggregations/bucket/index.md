---
layout: default
title: Bucket aggregations
has_children: true
has_toc: false
nav_order: 3
redirect_from:
  - /opensearch/bucket-agg/
  - /query-dsl/aggregations/bucket-agg/
  - /query-dsl/aggregations/bucket/
  - /aggregations/bucket-agg/
---
# Bucket aggregations

Bucket aggregations categorize sets of documents into buckets. The type of bucket aggregation determines which bucket a given document belongs to.

You can use bucket aggregations to implement faceted navigation (usually placed as a sidebar on a search result landing page) to help your users filter results.

## Supported bucket aggregations

OpenSearch supports the following bucket aggregations:

- [Adjacency matrix]({{site.url}}{{site.baseurl}}/aggregations/bucket/adjacency-matrix/)
- [Auto-interval date histogram]({{site.url}}{{site.baseurl}}/aggregations/bucket/auto-interval-date-histogram/)
- [Children]({{site.url}}{{site.baseurl}}/aggregations/bucket/children/)
- [Date histogram]({{site.url}}{{site.baseurl}}/aggregations/bucket/date-histogram/)
- [Date range]({{site.url}}{{site.baseurl}}/aggregations/bucket/date-range/)
- [Diversified sampler]({{site.url}}{{site.baseurl}}/aggregations/bucket/diversified-sampler/)
- [Filter]({{site.url}}{{site.baseurl}}/aggregations/bucket/filter/)
- [Filters]({{site.url}}{{site.baseurl}}/aggregations/bucket/filters/)
- [Geodistance]({{site.url}}{{site.baseurl}}/aggregations/bucket/geo-distance/)
- [Geohash grid]({{site.url}}{{site.baseurl}}/aggregations/bucket/geohash-grid/)
- [Geohex grid]({{site.url}}{{site.baseurl}}/aggregations/bucket/geohex-grid/)
- [Geotile grid]({{site.url}}{{site.baseurl}}/aggregations/bucket/geotile-grid/)
- [Global]({{site.url}}{{site.baseurl}}/aggregations/bucket/global/)
- [Histogram]({{site.url}}{{site.baseurl}}/aggregations/bucket/histogram/)
- [IP range]({{site.url}}{{site.baseurl}}/aggregations/bucket/ip-range/)
- [Missing]({{site.url}}{{site.baseurl}}/aggregations/bucket/missing/)
- [Multi-terms]({{site.url}}{{site.baseurl}}/aggregations/bucket/multi-terms/)
- [Nested]({{site.url}}{{site.baseurl}}/aggregations/bucket/nested/)
- [Range]({{site.url}}{{site.baseurl}}/aggregations/bucket/range/)
- [Reverse nested]({{site.url}}{{site.baseurl}}/aggregations/bucket/reverse-nested/)
- [Sampler]({{site.url}}{{site.baseurl}}/aggregations/bucket/sampler/)
- [Significant terms]({{site.url}}{{site.baseurl}}/aggregations/bucket/significant-terms/)
- [Significant text]({{site.url}}{{site.baseurl}}/aggregations/bucket/significant-text/)
- [Terms]({{site.url}}{{site.baseurl}}/aggregations/bucket/terms/)

## Common parameters

Most bucket aggregations support the following common parameters:

- `field`: The field to aggregate on.
- `script`: A script to generate values to aggregate on.
- `missing`: A value to use for documents that are missing the field.

Refer to the documentation for each specific aggregation type for details on additional parameters.

## Nesting aggregations

You can nest bucket aggregations inside other bucket or metric aggregations to create more complex analyses. For example:

```json
GET my-index/_search
{
  "aggs": {
    "genres": {
      "terms": {
        "field": "genre"  
      },
      "aggs": {
        "avg_rating": {
          "avg": {
            "field": "rating"
          }
        }
      }
    }
  }
}
```

This example first buckets documents by genre, then calculates the average rating for each genre bucket.

## Performance considerations 

Bucket aggregations can be resource intensive, especially on high-cardinality fields or when nesting multiple aggregations. Consider the following to optimize performance:

- Use `size` to limit the number of buckets returned.
- Apply a `filter` aggregation first to reduce the document set.
- For high-cardinality fields, consider using `significant_terms` instead of `terms`.
- Monitor aggregation performance and adjust as needed.

For more details on each bucket aggregation type, refer to its specific documentation page.
