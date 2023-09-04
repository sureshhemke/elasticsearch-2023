# Reindexing and Updating Documents
- If you’ve already indexed a bunch of data into Elasticsearch and changed your mind on how you want it stored

```
GET _cat/indices?v
```

## Run a search for "King Lear" in Shakespeare:
```
GET shakespeare/_search
{
  "query": {
    "term": {
      "play_name.keyword": {
        "value": "King Lear"
      }
    }
  }
}
```


## Run a search for the misspelled "King Leer" in Shakespeare:
```
GET shakespeare/_search
{
  "query": {
    "term": {
      "play_name.keyword": {
        "value": "King Leer"
      }
    }
  }
}
```


## Post to Shakespeare using the update by query API to replace all instances of "King Leer" with "King Lear":
```
POST shakespeare/_update_by_query
{
  "script": {
    "source": "ctx._source.play_name = 'King Lear'"
  },
  "query": {
    "term": {
      "play_name.keyword": {
        "value": "King Leer"
      }
    }
  }
}
```

## Run the search for "King Leer" to verify that zero results are returned.
```
GET shakespeare/_search
{
  "query": {
    "term": {
      "play_name.keyword": {
        "value": "King Leer"
      }
    }
  }
}
```

## Copy the Shakespeare Index from the es1 Node to the Shakespeare Node
- Navigate to the public IP address of the shakespeare node by copy-pasting the Public IP address of shakespeare provided in the Credentials into a new web browser window or tab.

### Log in as the elastic user with the password elastic_acg.

### View the indices for this node:
```
GET _cat/indices?v
```

### Migrate the data over using a POST _reindex:
```
POST _reindex
{
  "source": {
    "remote": {
      "host": "http://10.0.1.101:9200",
      "username": "elastic",
      "password": "elastic_acg"
    },
    "index": "shakespeare"
  },
  "dest": {
    "index": "shakespeare"
  }
}
```

### View the indices for this node again:
```
GET _cat/indices?v
```