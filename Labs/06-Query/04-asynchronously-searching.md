# Asynchronously Searching
- Occasionally, we need to ask data questions that may take some time to answer
- In these situations, it is beneficial to have the option of executing said query asynchronously

```
GET _cat/indices?v
```


```
POST /filebeat-7.13.4/_async_search?wait_for_completion_timeout=0
{
  "size": 100,
  "query": {
    "match": {
      "message": "ssh"
    }
  }
}
```

###Note the id.
###Check the Status of the Asynchronous Search
###Note: If you did not get an async search ID, then make sure you set the wait_for_completion_timeout parameter to 0 when creating the async search.

### Use the async search ID to get the status of the request:
```
GET _async_search/status/<ASYNC_SEARCH_ID>
```

```
GET _cat/indices?v
```

###there is only 1 shard.


## Get the Results of the Asynchronous Search
### Use the async search ID to get the results of the request:
```
GET _async_search/<ASYNC_SEARCH_ID>
```


###Scroll through the results to confirm the log messages mention SSH.

### Delete the Asynchronous Search
#### Use the async search ID to delete the request:
```
DELETE _async_search/<ASYNC_SEARCH_ID>
```

```
GET _async_search/status/<ASYNC_SEARCH_ID>
```

###Confirm the request was deleted. You should see error