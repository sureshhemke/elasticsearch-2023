# Searching with Term-Level Queries
- Elasticsearch offers a variety of different search functions that can be used to perform term-level, non-analyzed searching.

## Craft a Query That Answers the Question: Who Was the Customer That Placed Order ID 725676?
```
GET _cat/indices?v
```

```
GET ecommerce/_search
{
  "query": {
    "term": {
      "order_id": {
        "value": "725676"
      }
    }
  }
}
```

## Craft a Query That Answers the Question: From What Cities Did Samir Garza, Selena Gibbs, and Tariq Duncan Place Orders?

```
GET _cat/indices?v
```

### From what cities did Samir Garza, Selena Gibbs, and Tariq Duncan place orders?
```
GET ecommerce/_search
{
  "query": {
    "terms": {
      "customer_full_name.keyword": [
        "Samir Garza",
        "Selena Gibbs",
        "Tariq Duncan"
      ]
    }
  }
}
```

## Craft a Query That Answers the Question: What Orders Were Placed with a Total Taxed Price of 10 to 20 Euro?
```
GET _cat/indices?v
```

### What orders were placed with a total taxed price of 10 to 20 euro?
```
GET ecommerce/_search
{
  "query": {
    "range": {
      "taxful_total_price": {
        "gte": 10,
        "lte": 20
      }
    }
  }
}
```

