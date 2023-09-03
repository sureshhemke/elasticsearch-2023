# Managing Indices with Index Lifecycle Management (ILM) in Elasticsearch
- Efficiently ingesting continuously generated data 24/7 is one of the many things Elasticsearch is very good at.
- But eventually, you're going to run out of space. This is why it is important to leverage the index lifecycle management (ILM) feature in Elasticsearch


## Create the audit_policy ILM Policy
### Verify that the node has been allocated properly:
```
GET _cat/nodes?v
```

### Create the audit_policy by executing the following:
```
PUT _ilm/policy/audit_policy
{
  "policy": {
   "phases": {
    "hot": {
     "actions": {}
    },
    "cold": {
     "min_age": "7d",
     "actions": {
       "freeze": {},
       "readonly": {}
     }
   },
   "delete": {
    "min_age": "365d",
    "actions": {
     "delete": {}
    }
   }
  }
 }
}
```

```
GET _ilm/policy/audit_policy
```

### Create the audit_template Index Template
```
PUT _index_template/audit_template
{
  "index_patterns": ["audit-*"],
  "template": {
    "settings": {
     "number_of_shards": 1,
     "number_of_replicas": 0,
     "index.lifecycle.name": "audit_policy"
    }
  }
}
```

```
GET _index_template/audit_template
```

### Create and Verify the First audit-DD-MM-YYYY Index
```
PUT audit-09-13-2021
```

```
GET audit-09-13-2021
```
