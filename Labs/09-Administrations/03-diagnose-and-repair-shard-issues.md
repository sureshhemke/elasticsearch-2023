# Diagnose and repair shard issues
```
PUT /logs
```

```
GET _cat/nodes?v
GET _cat/indices?v
```


```
GET _cluster/allocation/explain
```

```
PUT logs/_settings
{
 "number_of_replicas": 3
}
```


```
GET _cat/indices?v
```

```
GET _cluster/allocation/explain
```

```
GET _cat/shards/logs?v
```

```
GET _cat/indices?v
```

```
PUT logs/_settings
{
 "number_of_replicas": 1
}
```

```
GET _cat/indices?v
```

```
GET _cluster/allocation/explain
```

```
GET _cat/shards/logs?v
```

```
GET _cat/indices?v
```

```
PUT logs/_settings
{
 "index.routing.allocation.include._name": "data=3"
}
```


```
GET _cluster/allocation/explain
{
 "index": "logs",
 "shard": 0,
 "primary": true
}
```


# Diagnosing and Repairing Shard Issues
- You are a data infrastructure engineer working as a private contractor. You've recently taken on a contract to help an e-commerce company with some Elasticsearch shard allocation issues
- Your task is to troubleshoot and repair the shard issues for the accounts-1 and accounts-2 indices and then configure the indices so that they don't fail the same way again and have the best possible redundancy and reliability.

```
GET _cat/shards/accounts-1?v
```

- The state should be UNASSIGNED.

## Use the cluster allocation explain API to view the shard:
```
GET _cluster/allocation/explain
```

- Note the information in the allocate_explanation and explanation fields.

## Display the accounts-1 index values and note the number of nodes and replicas:
```
GET _cat/indices/accounts-1?v
```

## Modify accounts-1 to contain only 2 replicas:
```
PUT accounts-1/_settings
{
  "number_of_replicas": 2
}
```

## Display the index's nodes:
```
GET _cat/nodes?v
```

- Note that only nodes es1 and es2 are currently online.

## View the accounts-2 shards and check their state:
```
GET _cat/shards/accounts-2?v
```

- The state should be UNASSIGNED.

## View shard 0 on the accounts-2 index:
```
GET _cluster/allocation/explain
{
  "index": "accounts-2",
  "shard": 0,
  "primary": true
}
```

- Note the information in the allocate_explanation and explanation fields. The accounts-2 shards are all on the offline es3 node that needs to be brought online.

## Open a terminal session and log in to the es3 node using the credentials provided on the lab page:
```
ssh cloud_user@<PUBLIC_IP_ADDRESS>
```

## Start Elasticsearch:
```
sudo systemctl start elasticsearch
```

- Note: Elasticsearch may take several minutes to successfully start up.

## Return to the Kibana console and display the nodes:
```
GET _cat/nodes?v
```

- You should now see the s3 node online.

## Check the accounts-1 and accounts-2 shards:
```
GET _cat/shards/accounts-1?v
GET _cat/shards/accounts-2?v
```

- You should now see that all shards are allocated.

## View the indices:
```
GET _cat/indices?v
```

- You should now see the health status of green for all indices.

## Reconfigure the Accounts' Indices to Avoid Repeated Failure
### Modify accounts-2 to maximize redundancy and availability of nodes:
```
PUT accounts-2/_settings
{
  "number_of_replicas": 2
}
```

### Check the indices:
```
GET _cat/indices?v
```

- accounts-2 should now have a health status of yellow.

### Check the accounts-2 shards:
```
GET _cat/shards/accounts-2?v
```

- The replicas should appear as UNASSIGNED.

### View the replicas on accounts-2:
```
GET _cluster/allocation/explain
{
  "index": "accounts-2",
  "shard": 0,
  "primary": true
}
```

- Note the information in the allocate_explanation and explanation fields. The index routing allocation filter only allows accounts-2 index shards to go to the es3 node.

### Modify accounts-2 again and set the filter to a value of null:
```
PUT accounts-2/_settings
{
  "index.routing.allocation.require._name": null
}
```

### View the accounts-2 shards:
```
GET _cat/shards/accounts-2?v
```

- All shards should now be allocated.

### Check the indices:
```
GET _cat/indices?v
```

- You should see all indices with a health status of green and maximum replication.