# Maintaining Nested Arrays of Objects
- Elasticsearch likes flattened and denormalized data. So when it comes to storing arrays of objects, you need to take some special precautions in order to maintain the relationships between each object

## Create the ecommerce_fixed Index
```
GET _cat/indices?v
```

## View the mappings of the ecommerce index:
```
GET ecommerce
```


## Copy all the mappings
### Create an ecommerce fixed index:
```
PUT ecommerce_fixed
{
  "mappings": {
  }
}
```

- Paste the mappings under the mappings line.
- Click the circular icon in the top right corner of the console.
- From the dropdown, select Auto indent to fix the indentation.

### Under the products line, add the nested type:
```
"products": {
  "type": "nested",
}
```

### Under the mappings section, set the number of shards:
```
"settings": {
  "number_of_shards": 1,
  "number_of_replicas": 0
}
```

### View the ecommerce_fixed index and verify that products have a nested type:
```
GET ecommerce_fixed
```

## Reindex the ecommerce Index to ecommerce_fixed Index
### Reindex the data:
```
POST _reindex
{
  "source": {
    "index": "ecommerce"
  }
  "dest": {
    "index": "ecommerce_fixed"
  }
}
```

### Check the ecommerce_fixed index:
```
GET ecommerce_fixed/_search
```

## Perform a Nested Search on the Products Object of the ecommerce_fixed Index
### Search the ecommerce index for the SKU and product name of the product:
```
GET ecommerce/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "products_sku": {
              "value": "Z00549605496"
            }
          }
        },
        {
          "term": {
            "products_id": {
              "value": "sold_product_584677_19400"
            }
          }
        }
      ]
    }
  }
}
```

```
GET ecommerce_fixed/_search
{
  "query": {
    "nested": {
      "path": "path_to_nested.doc", 
      "query": {
        "bool": {
          "must": [
            {
              "term": {
                "products"_sku": {
                  "value": "Z00549605496"
            }
          ]
        } 
      }
    }
  }
}
```


- In the ecommerce search, paste in the SKU and product ID from the same product.

- Press Ctrl + Enter to execute the request.