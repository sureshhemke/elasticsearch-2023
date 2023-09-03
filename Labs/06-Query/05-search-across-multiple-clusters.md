# Search Across Multiple Clusters
- It is not uncommon to have multiple Elasticsearch clusters whenever you’re dealing with several use cases that have different performance requirements
- To make it easier to interact with multiple clusters, you can leverage cross-cluster search

- In a web browser, navigate to the public IP address of the first node

- Note: While you are querying the es2 and es3 servers, you do not need to log in to them to do so.

## Configure Cross-Cluster Search on the Primary Server
```
GET _cat/indices?v
```

###In the list of indices returned in the response pane, verify that the metricbeat-7.13.4 index is present, which is the dataset you will be querying.

### Add a persistent remote cluster setting for the es2 and es3 clusters:
- For es2, name it cluster_2, use the 10.0.1.102 IP address, and make sure to use the transport interface on port 9300
- For es3, name it cluster_3, use the 10.0.1.103 IP address, and make sure to use the transport interface on port 9300
```
PUT _cluster/settings
{
  "persistent": {
    "cluster": {
      "remote": {
        "cluster_2": {
          "seeds": ["10.0.1.102:9300"]
        },
        "cluster_3": {
          "seeds": ["10.0.1.103:9300"]
        }
      }
    }
  }
}
```

```
GET _cluster/settings
```


- ###The cluster settings should display in the response pane, indicating it was configured successfuly.

## Execute the Cross-Cluster Search on the Secondary Servers
### Query the metricbeat-7.13.4 dataset on es2:
```
GET cluster_2:metricbeat-7.13.4/_search
```

- Scroll through the response pane and verify that the data returned is from the dataset on es2.

### Query the metricbeat-7.13.4 dataset on es3:
```
GET cluster_3:metricbeat-7.13.4/_search
```

###Scroll through the response pane and verify that the data returned is from the dataset on es3.

## Execute the Cross-Cluster Search on All Clusters Simultaneously
### Query the metricbeat-7.13.4 dataset on es1, es2. and es3 simultaneously with a single query:
```
GET metricbeat-7.13.4,cluster_2:metricbeat-7.13.4,cluster_3:metricbeat-7.13.4/_search
```

- Scroll through the response pane and verify that the data returned is from the datasets on es1, es2, and es3