# The match query

## Basic usage

```
GET /products/_search
{
  "query": {
    "match": {
      "name": "pasta"
    }
  }
}
```

Full text queries are analyzed (and therefore case insensitive), so the below query yields the same results.

```
GET /products/_search
{
  "query": {
    "match": {
      "name": "PASTA"
    }
  }
}
```

## Searching for multiple terms

```
GET /products/_search
{
  "query": {
    "match": {
      "name": "PASTA CHICKEN"
    }
  }
}
```

## Specifying the operator

Defaults to `or`. The below makes both terms required.

```
GET /products/_search
{
  "query": {
    "match": {
      "name": {
        "query": "pasta chicken",
        "operator": "and"
      }
    }
  }
}
```


# Phrase searches

## Basic usage

```
GET /products/_search
{
  "query": {
    "match_phrase": {
      "name": "mango juice"
    }
  }
}
```

## More examples

```
GET /products/_search
{
  "query": {
    "match_phrase": {
      "name": "juice mango"
    }
  }
}
```

```
GET /products/_search
{
  "query": {
    "match_phrase": {
      "name": "Juice (mango)"
    }
  }
}
```

```
GET /products/_search
{
  "query": {
    "match_phrase": {
      "description": "browse the internet"
    }
  }
}
```
