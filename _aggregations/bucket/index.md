# Bucket aggregations

Bucket aggregations categorize sets of documents into buckets. The type of bucket aggregation determines the bucket for a given document.

You can use bucket aggregations to implement faceted navigation (usually placed as a sidebar on a search result landing page) to help your users filter the results.

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

## Usage

To use a bucket aggregation, specify it in the `aggs` section of your search request:

```json
GET _search
{
  "aggs": {
    "my_bucket_agg": {
      "BUCKET_TYPE": {
        ...
      }
    }
  }
}
```

Replace `BUCKET_TYPE` with the type of bucket aggregation you want to use.

## Common parameters

Most bucket aggregations support the following common parameters:

- `field`: The field to aggregate on
- `script`: A script to generate values to aggregate on
- `missing`: A value to use for documents that are missing the field

## Nesting aggregations

You can nest bucket aggregations within other bucket aggregations to create more complex aggregations. For example:

```json
GET _search
{
  "aggs": {
    "my_date_histo": {
      "date_histogram": {
        "field": "date",
        "interval": "month"
      },
      "aggs": {
        "my_terms": {
          "terms": {
            "field": "category"
          }
        }
      }
    }
  }
}
```

This creates date histogram buckets and then subdivides those buckets using a terms aggregation.

## Performance considerations 

Bucket aggregations can be computationally expensive, especially on large datasets. To improve performance:

- Use filters to reduce the number of documents before aggregating
- Set a reasonable `size` parameter to limit the number of buckets returned
- Consider using sampling aggregations like `sampler` for approximate results on very large datasets

For more details on each bucket aggregation type, refer to the individual documentation pages linked above.