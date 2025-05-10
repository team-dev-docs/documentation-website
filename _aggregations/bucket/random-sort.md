---
layout: default
title: Random sort
parent: Bucket aggregations
nav_order: 165
---

# Random sort

The random sort plugin allows you to sort search results randomly. This can be useful for implementing features like randomized search results or A/B testing.

## Installation

The random sort plugin is not installed by default. To install it, use the plugin install command:

```bash
bin/opensearch-plugin install random-sort
```

## Usage

To use random sorting, add a `sort` parameter to your search request with the following syntax:

```json
{
  "sort": [
    {
      "_script": {
        "type": "number",
        "script": {
          "lang": "randomsort",
          "source": "random_sort"
        },
        "order": "asc"
      }
    }
  ]
}
```

You can optionally provide a `seed` parameter to ensure consistent randomization across requests:

```json
{
  "sort": [
    {
      "_script": {
        "type": "number",
        "script": {
          "lang": "randomsort",
          "source": "random_sort",
          "params": {
            "seed": 1234
          }
        },
        "order": "asc"
      }
    }
  ]
}
```

## Example

Here's a complete example of a search request using random sort:

```json
GET /my-index/_search
{
  "size": 10,
  "query": {
    "match_all": {}
  },
  "sort": [
    {
      "_script": {
        "type": "number",
        "script": {
          "lang": "randomsort",
          "source": "random_sort",
          "params": {
            "seed": 1234
          }
        },
        "order": "asc"
      }
    }
  ]
}
```

This will return 10 documents from `my-index`, sorted in a random order. The randomization will be consistent across requests as long as the same seed is used.

## How it works

The random sort plugin implements a custom script engine that generates a random number for each document. This number is then used as the sort key. When a seed is provided, it ensures that the same sequence of random numbers is generated for each document across multiple requests.

## Performance considerations 

While random sorting can be useful, it may impact query performance, especially on large datasets. The plugin has to generate a random number for each matching document, which can be computationally expensive. Use this feature judiciously and consider its impact on your overall system performance.