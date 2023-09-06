# Setup Metricbeat
```
curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-8.9.1-amd64.deb
```

```
sudo dpkg -i metricbeat-8.9.1-amd64.deb
```

```
nano /etc/metricbeat/metricbeat.yml
```

```
sudo sed -i 's/#host: "localhost:5601"/host: "vm-es009.eastus2.cloudapp.azure.com:5601"/' /etc/metricbeat/metricbeat.yml
sudo sed -i 's/hosts: ["localhost:9200"]/hosts: ["vm-es009.eastus2.cloudapp.azure.com:9200"]/' /etc/metricbeat/metricbeat.yml
```


```
metricbeat modules list
metricbeat modules enable docker
```

```
systemctl status metricbeat
systemctl stop metricbeat
systemctl start metricbeat
systemctl status metricbeat
systemctl enable --now metricbeat
```



```
curl http://vm-es009.eastus2.cloudapp.azure.com:9200/_cat/indices?v
```


## Troubleshooting
```
curl -X GET 'http://vm-es009.eastus2.cloudapp.azure.com:9200'
curl -X GET 'http://vm-es009.eastus2.cloudapp.azure.com:9200/_nodes?pretty'
curl -XPOST -H "Content-Type: application/json" 'http://vm-es009.eastus2.cloudapp.azure.com:9200/cyberithub/_doc/1' -d '{ "message": "Hello World!" }'
curl -XGET -H "Content-Type: application/json" 'http://vm-es009.eastus2.cloudapp.azure.com:9200/cyberithub/_doc/1'
sudo journalctl -xefu metricbeat.service
sudo tail -n 200 -f /var/log/metricbeat/metricbeat.log
```
