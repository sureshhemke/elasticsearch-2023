# Perform CRUD Operations

## Indexing document with auto generated ID:

```
POST /products/_doc
{
  "name": "Coffee Maker",
  "price": 64,
  "in_stock": 10
}
```

## Indexing document with custom ID:

```
PUT /products/_doc/100
{
  "name": "Toaster",
  "price": 49,
  "in_stock": 4
}
```

## Updating an existing field
```
POST /products/_update/100
{
  "doc": {
    "in_stock": 3
  }
}
```

## Adding a new field
```
POST /products/_update/100
{
  "doc": {
    "tags": ["electronics"]
  }
}
```

## Replacing documents
```
PUT /products/_doc/100
{
  "name": "Toaster",
  "price": 79,
  "in_stock": 4
}
```


## Deleting documents

```
DELETE /products/_doc/101
```

## Matches all of the documents within the `products` index
```
GET /products/_search
{
  "query": {
    "match_all": {}
  }
}
```


## Updating documents matching a query
- Replace the `match_all` query with any query that you would like.

```
POST /products/_update_by_query
{
  "script": {
    "source": "ctx._source.in_stock--"
  },
  "query": {
    "match_all": {}
  }
}
```

## Deleting documents that match a given query

```
POST /products/_delete_by_query
{
  "query": {
    "match_all": { }
  }
}
```

