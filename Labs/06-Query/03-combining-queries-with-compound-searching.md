# Combining Queries with Compound Searching
- For more complicated queries, you need to be able to combine many different search functions together
- You can do this with compound search functions in Elasticsearch

## Craft a Query That Answers the Question: How Many Web Requests Originate from Either US or CN Where the Response Was Not 200?
### Verify that all indices have been allocated properly:
```
GET _cat/indices?v
```

### In the list of indices returned in the response pane, verify that the kibana_sample_data_logs index is present, which is the dataset you will be querying.

## Craft a query that will determine how many web requests originated from the US or CN where the response was not 200. To do so, you will need to combine a boolean search that:
- Must include the values of US or CN for the geo.src field
- Must not contain the value of 200 for the response.keyword field

```
GET logs/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "terms": {
            "geo.src": [
              "US",
              "CN"
            ]
          }
        }
      ],
      "must_not": [
        {
          "term": {
            "response.keyword": {
              "value": "200"
            }
          }
        }
      ]
    }
  }
}
```

###In the response pane, view the hits > value field to determine the number of web requests that met the query criteria.

###Scroll through some of the web requests that were returned and validate that either US or CN appears in the geo > src field and some number other than 200 appears in the response field.

### Craft a query that will determine how many web requests with a response of 200 and at least 2 of the success, security, or info tags were made from a non-Windows machine. To do so, you will need to combine a boolean search that:
- Must contain the value of 200 for the response.keyword field
- Should include a minimum of 2 matches for the values of success, security, or info for the tags.keyword field
- Must not contain the value of win (to be inclusive of all Windows versions) for the machine.os field
```
GET logs/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "response.keyword": {
              "value": "200"
            }
          }
        }
      ],
      "should": [
        {
          "term": {
            "tags.keyword": {
              "value": "success"
            }
          }
        },
        {
          "term": {
            "tags.keyword": {
              "value": "security"
            }
          }
        },
        {
          "term": {
            "tags.keyword": {
              "value": "info"
            }
          }
        }
      ],
      "minimum_should_match": 2,
      "must_not": [
        {
          "match": {
            "machine.os": "win"
          }
        }
      ]
    }
  }
}
```

###In the response pane, view the hits > value field to determine the number of web requests that met the query criteria.
###Scroll through some of the web requests that were returned and validate that only the number 200 appears in the response field; 2 tags that are either success, security, or info appear in the tags field; and some operating system other than win (such as ios or osx) appears in the os field.

## Craft a query that will determine how many web requests were made for apm-server RPM or DEB files from a machine running OS X, and filter the results to only include files between 1024 and 8192 bytes without affecting relevancy scoring. To do so, you will need to combine a boolean search that:
- Must contain the value of apm-server as the prefix for the request field
- Must contain the values of deb or rpm for the extension.keyword field
- Must contain the value of osx for the machine.os.keyword field
- Filters the results to only display a range of files that are 1024 to 8192 bytes in size
```
GET logs/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match_phrase_prefix": {
            "request": "apm-server"
          }
        },
        {
          "terms": {
            "extension.keyword": [
              "deb",
              "rpm"
            ]
          }
        },
        {
          "term": {
            "machine.os.keyword": {
              "value": "osx"
            }
          }
        }
      ],
      "filter": [
        {
          "range": {
            "bytes": {
              "gte": 1024,
              "lte": 8192
            }
          }
        }
      ]
    }
  }
}
```

###In the response pane, view the hits > value field to determine the number of web requests that met the query criteria.
###Scroll through some of the web requests that were returned and validate that the term apm-server appears somewhere in the request field; only files with the deb or rpm extension are displayed in the extension field; only the osx operating system appears in the os field; and only files between 1024 and 8192 bytes in size, as displayed in the bytes field, were returned.