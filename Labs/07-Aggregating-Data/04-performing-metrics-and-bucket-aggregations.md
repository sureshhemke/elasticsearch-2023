# Performing Metrics and Bucket Aggregations 
- Performing metrics and bucket aggregations is essential to asking questions of your data in Elasticsearch. 

```
GET _cat/indices?v
```

## Craft an aggregation that will determine the total lifetime sales before tax. To do so, you will need to create a metrics aggregation named total_salesthat performs the sum aggregation on the taxless_total_price field:
```
GET ecommerce/_search
{
  "size": 0,
  "aggs": {
    "total_sales": {
      "sum": {
        "field": "taxless_total_price"
      }
    }
  }
}
```

- In the response pane, view the aggregations > total_sales > value field to determine the total amount of sales before tax that have been generated.


## Craft an aggregation that will determine the number of orders per day on each calendar day. To do so, you will need to create a bucket aggregation named orders_per_daythat performs the date_histogram aggregation on the order_date field with a calendar_interval of a day:
```
GET ecommerce/_search
{
  "size": 0,
  "aggs": {
    "orders_per_day": {
      "date_histogram": {
        "field": "order_date",
        "calendar_interval": "day"
      }
    }
  }
}
```

- In the response pane, scroll through the results and view the aggregations > orders_per_day > buckets > doc_count field to determine the number of orders that were placed/received on each calendar day.


## Craft an aggregation that will determine the total sales before tax for each calendar day. To do so, you will need to create an aggregation called total_sales_per_day that combines the orders_per_day bucket aggregation with the nested total_sales metrics aggregation.
```
GET ecommerce/_search
{
  "size": 0,
  "aggs": {
    "total_sales_per_day": {
      "date_histogram": {
        "field": "order_date",
        "calendar_interval": "day"
      },
      "aggs": {
        "total_sales": {
          "sum": {
            "field": "taxless_total_price"
          }
        }
      }
    }
  }
}
```

- In the response pane, scroll through the results and view the aggregations > total_sales_per_day > buckets > total_sales > value field to determine the total amount of sales before tax that was generated on each calendar day.