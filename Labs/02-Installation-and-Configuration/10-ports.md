- Elasticsearch: 9200
- Kibana: 5601
- Logstash: 5044
- Beats are client libraries, so no port number.

- Port 9200 is used for all API calls over HTTP. This includes search and aggregations, monitoring and anything else that uses a HTTP request. All client libraries will use this port to talk to Elasticsearch

- Port 9300 is a custom binary protocol used for communications between nodes in a cluster. For things like cluster updates, master elections, nodes joining/leaving, shard allocation

