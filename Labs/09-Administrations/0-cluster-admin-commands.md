```
GET products/_search?human=true
```

```
GET products/_settings?flat_settings=true
```


```
GET _cluster/health
```


```
GET _cluster/state
```


```
GET _cluster/state/{metrics}/{indices}
```


```
GET _cluster/stats
```


```
GET /_cluster/stats/nodes/node_name_1,master:false
```
## Move shard from one node to another.

#Check the status first.
GET /_cat/shards/products?v

```
POST /_cluster/reroute
{
  "commands": [
    {
      "move": {
        "index": "products",
        "shard": 0,
        "from_node": "data-7",
        "to_node": "data-4"
      }
    }
  ]
}
```

```
POST /_cluster/reroute
{
  "commands": [
    {
      "move": {
        "index": "reroute_me",
        "shard": 0,
        "from_node": "node1",
        "to_node": "node2"
      }
    },
    {
      "allocate_replica": {
        "index": "reroute_me",
        "shard": 1,
        "node": "node3"
      }
    }
  ]
}
```


```
GET /_cluster/settings
```


```
GET /_nodes
```


```
GET /_nodes/_all
```


```
GET _nodes/os
```


```
GET _nodes/process
```


```
PUT _cluster/settings
{
  "persistent": {
    "action.auto_create_index": "true"
  }
}
```


