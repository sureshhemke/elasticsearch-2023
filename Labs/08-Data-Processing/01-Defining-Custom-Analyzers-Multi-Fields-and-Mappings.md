## Analyzing a string with the `standard` analyzer
```
POST /_analyze
{
  "text": "2 guys walk into   a bar, but the third... DUCKS! :-)",
  "analyzer": "standard"
}
```

## Building the equivalent of the `standard` analyzer
```
POST /_analyze
{
  "text": "2 guys walk into   a bar, but the third... DUCKS! :-)",
  "char_filter": [],
  "tokenizer": "standard",
  "filter": ["lowercase"]
}
```

## Testing the `keyword` analyzer
```
POST /_analyze
{
  "text": "2 guys walk into   a bar, but the third... DUCKS! :-)",
  "analyzer": "keyword"
}
```

## Understanding type coercion

### Supplying a floating point
```
PUT /coercion_test/_doc/1
{
  "price": 7.4
}
```

### Supplying a floating point within a string
```
PUT /coercion_test/_doc/2
{
  "price": "7.4"
}
```

### Supplying an invalid value
```
PUT /coercion_test/_doc/3
{
  "price": "7.4m"
}
```

### Retrieve document
```
GET /coercion_test/_doc/2
```

### Clean up
```
DELETE /coercion_test
```


## Adding explicit mappings

### Add field mappings for `reviews` index
```
PUT /reviews
{
  "mappings": {
    "properties": {
      "rating": { "type": "float" },
      "content": { "type": "text" },
      "product_id": { "type": "integer" },
      "author": {
        "properties": {
          "first_name": { "type": "text" },
          "last_name": { "type": "text" },
          "email": { "type": "keyword" }
        }
      }
    }
  }
}
```

### Index a test document
```
PUT /reviews/_doc/1
{
  "rating": 5.0,
  "content": "Outstanding course! Bo really taught me a lot about Elasticsearch!",
  "product_id": 123,
  "author": {
    "first_name": "John",
    "last_name": "Doe",
    "email": "johndoe123@example.com"
  }
}
```

## Retrieving mappings

### Retrieving mappings for the `reviews` index
```
GET /reviews/_mapping
```

### Retrieving mapping for the `content` field
```
GET /reviews/_mapping/field/content
```

### Retrieving mapping for the `author.email` field
```
GET /reviews/_mapping/field/author.email
```

## Using dot notation in field names

### Using dot notation for the `author` object
```
PUT /reviews_dot_notation
{
  "mappings": {
    "properties": {
      "rating": { "type": "float" },
      "content": { "type": "text" },
      "product_id": { "type": "integer" },
      "author.first_name": { "type": "text" },
      "author.last_name": { "type": "text" },
      "author.email": { "type": "keyword" }
    }
  }
}
```

### Retrieve mapping
```
GET /reviews_dot_notation/_mapping
```

## Adding mappings to existing indices

### Add new field mapping to existing index
```
PUT /reviews/_mapping
{
  "properties": {
    "created_at": {
      "type": "date"
    }
  }
}
```

### Retrieve the mapping
```
GET /reviews/_mapping
```

