# Creating Index Templates in Elasticsearch
- When working with time-series or data streaming use cases, you typically want to spread your data out into multiple indices as to not make an index too large and incur performance penalties as a result
- By using index templates, we can instantiate an index for a given dataset very quickly and even automatically.

### Verify that all indices have been allocated properly:
```
GET _cat/nodes?v
GET _cat/indices?v
```


```
PUT _index_templates/earthquakes
{
 "index_patterns": ["earthquakes-*"],
 "template":  {
   "settings": {
     "number_of_shards": 1,
     "number_of_replicas": 0      
   }
 }
}
```

```
GET _index_templates/earthquakes
```


```
PUT earthquakes-1
```

```
GET earthquakes-1
```

```
DELETE earthquakes-1
```

```
DELETE _index_templates/earthquakes
```

```
PUT _component_template/shards
{
  "template":  {
   "settings": {
     "number_of_shards": 1,
     "number_of_replicas": 0      
   }
 }
}
```


```
GET _component_template/shards
```

```
PUT _index_templates/earthquakes
{
 "index_patterns": ["earthquakes-*"],
 "composed_of": ["shards"]
}
```


```
GET _index_templates/earthquakes
```


```
PUT earthquakes-1
```

```
GET earthquakes-1
```


### Create Load index template:
```
PUT _index_template/load
{
  "index_patterns": ["load-*"], 
  "template": {
     "settings": {
        "number_of_shards": 1,
        "number_of_replicas": 0 
       } 
     } 
}
```

```
GET _index_template/load
```

```
PUT load-1
```

```
GET load-1
```


### Create the CPU Index Template
```
GET _cat/indices?v
```

```
PUT _index_template/cpu
{
  "index_patterns": ["cpu-*"],
  "template": {
        "settings": {
              "number_of_shards": 1,
              "number_of_replicas": 0
           } 
     } 
}
```

```
GET _index_template/cpu
```

```
PUT cpu-1
```

```
GET cpu-1
```


### Create the Memory Index Template
```
GET _cat/indices?v
```

PUT _index_template/memory
{
  "index_patterns": ["memory-*"],
  "template": {
     "settings": {
          "number_of_shards": 1,
          "number_of_replicas": 0 
       } 
  } 
}


```
GET _index_template/memory
```

```
PUT memory-1
```

```
GET memory-1
```