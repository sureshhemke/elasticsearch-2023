# Configure Kibana
``````
sudo su
ip add show
```

```
cat /etc/kibana/kibana.yml
master_ip="10.0.0.6"
```

```
cat <<EOT >> /etc/kibana/kibana.yml
server.host: "$master_ip"
EOT
```

```
cat /etc/kibana/kibana.yml
```

```
systemctl stop kibana
systemctl enable kibana
systemctl start kibana
```


```
tail -f  /var/log/kibana/kibana.log
```


- After Kibana has finished starting up, navigate to http://PUBLIC_IP_ADDRESS_OF_MASTER-1:5601 in your web browser and navigate to Dev Tools > Console

  - http://elk-vm-master.eastus.cloudapp.azure.com:5601/
