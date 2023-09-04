# Bucket aggregations

## Creating a bucket for each `status` value

```
GET /orders/_search
{
  "size": 0,
  "aggs": {
    "status_terms": {
      "terms": {
        "field": "status"
      }
    }
  }
}
```

## Including `3` terms instead of the default `10`

```
GET /orders/_search
{
  "size": 0,
  "aggs": {
    "status_terms": {
      "terms": {
        "field": "status",
        "size": 3
      }
    }
  }
}
```



## Ordering the buckets

```
GET /orders/_search
{
  "size": 0,
  "aggs": {
    "status_terms": {
      "terms": {
        "field": "status",
        "size": 20,
        "order": {
          "_key": "asc"
        }
      }
    }
  }
}
```