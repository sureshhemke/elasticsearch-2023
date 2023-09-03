# Creating Indices in Elasticsearch
## Introduction
- Before you can ingest any data into Elasticsearch, you first need to construct indices for a particular dataset

## Create the leads_b2b-000001 Index
### Verify that all nodes are up and running:
```
GET _cat/nodes?v
```

### Craft a request to create an index named leads_b2b-000001that has 1 primary shard and 0 replica shards:
```
PUT leads_b2b-000001
{
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 0
  }
}
```

### Check the health of the cluster to verify that the index was created successfully with the proper replication configured to maintain a green status:
```
GET _cat/health?v
```

- The cluster should display in the response pane with green status, indicating it was created and configured successfully.

### Then, check the status of all the indices on the cluster to verify the index's individual status:
```
GET _cat/indices?v
```

- The leads_b2b-000001 index should display in the list of indices with green health, 1 primary shard, and 0 replicas, indicating it was created and configured successfully.

### Craft a request to create an index named leads_b2c-000001that has 1 primary shard and 0 replica shards:
```
PUT leads_b2c-000001
{
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 0
  }
}
```

```
GET _cat/indices?v
```


### Create the sales_b2c-000001 Index
```
PUT sales_b2c-000001
{
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 0
  }
}
```

```
GET _cat/indices?v
```

