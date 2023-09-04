# Configure and create a cluster

## On Each node
ip addr show
sudo su

# Change IP Address on below line
```
seed_hosts=10.0.0.4   # Should be private ip of master node
```

```
cat /etc/elasticsearch/elasticsearch.yml
cat <<EOT >> /etc/elasticsearch/elasticsearch.yml
cluster.name: cluster-1
network.host: [_local_, _site_]
discovery.seed_hosts: ["$seed_hosts"]
cluster.initial_master_nodes: ["master-1"]
EOT
cat /etc/elasticsearch/elasticsearch.yml
exit
```

## On Master Node
```
sudo su
```


```
node_name=master-1
cat <<EOT >> /etc/elasticsearch/elasticsearch.yml
node.name: $node_name
node.master: true
node.data: false
node.ingest: false
node.ml: false
EOT
```

```
cat /etc/elasticsearch/elasticsearch.yml
```

```
sudo cat /etc/elasticsearch/jvm.options | grep Xms4g
sudo cat /etc/elasticsearch/jvm.options | grep Xmx4g
sudo sed -i 's/Xms4g/Xms3g/' /etc/elasticsearch/jvm.options
sudo sed -i 's/Xmx4g/Xmx3g/' /etc/elasticsearch/jvm.options
sudo cat /etc/elasticsearch/jvm.options | grep Xms3g
sudo cat /etc/elasticsearch/jvm.options | grep Xmx3g
```

## On Data Node:
```
sudo su
```

```
node_name=data-1
```

```
cat <<EOT >> /etc/elasticsearch/elasticsearch.yml
node.name: $node_name
node.attr.temp: hot
node.master: false
node.data: true
node.ingest: true
node.ml: false
EOT
```

```
sudo sed 's/Xms1g/Xms2g/' /etc/elasticsearch/jvm.options
sudo sed 's/Xmx1g/Xmx2g/' /etc/elasticsearch/jvm.options
```

## On Data Node:
```
node_name=data-2
```

```
cat <<EOT >> /etc/elasticsearch/elasticsearch.yml
node.name: $node_name
node.attr.temp: warm
node.master: false
node.data: true
node.ingest: true
node.ml: false
EOT
```

```
sudo sed 's/Xms1g/Xms2g/' /etc/elasticsearch/jvm.options
sudo sed 's/Xmx1g/Xmx2g/' /etc/elasticsearch/jvm.options
```

```
sudo systemctl enable elasticsearch
sudo systemctl stop elasticsearch
sudo systemctl start elasticsearch
curl -X GET "localhost:9200"
```

### Check the startup process:
```
sudo less /var/log/elasticsearch/cluster-1.log
```


### Check the node configuration:
```
curl localhost:9200/_cat/nodes?v
```
