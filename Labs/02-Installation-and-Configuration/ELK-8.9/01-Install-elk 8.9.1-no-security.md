# Install ElasticSearch, Logstash and Kibana

## First Remove ELK stack if required (Optional)
```
sudo systemctl stop elasticsearch
sudo systemctl stop kibana
sudo systemctl stop logstash
sudo systemctl disable logstash
sudo systemctl disable kibana
sudo systemctl disable elasticsearch
sudo apt remove --purge elasticsearch kibana logstash metricbeat
sudo rm -rf /run/systemd/propagate/elasticsearch.service
sudo rm -rf /etc/rc0.d/K01elasticsearch
sudo rm -rf /etc/rc1.d/K01elasticsearch
sudo rm -rf /etc/rc2.d/K01elasticsearch
sudo rm -rf /etc/rc3.d/K01elasticsearch
sudo rm -rf /etc/rc4.d/K01elasticsearch
sudo rm -rf /etc/rc5.d/K01elasticsearch
sudo rm -rf /etc/rc6.d/K01elasticsearch
sudo rm -rf /etc/elasticsearch
sudo rm -rf /var/lib/elasticsearch
sudo rm -rf /etc/rc0.d/K01kibana
sudo rm -rf /etc/rc1.d/K01kibana
sudo rm -rf /etc/rc2.d/K01kibana
sudo rm -rf /etc/rc3.d/K01kibana
sudo rm -rf /etc/rc4.d/K01kibana
sudo rm -rf /etc/rc5.d/K01kibana
sudo rm -rf /etc/rc6.d/K01kibana
sudo rm -rf /etc/kibana
sudo rm -rf /etc/kibana/.kibana.keystore.initial_md5sum
sudo rm -rf /etc/kibana/kibana.keystore
sudo rm -rf /etc/kibana/kibana.yml.bkp
sudo rm -rf /etc/rc4.d/K01kibana
sudo rm -rf /etc/rc2.d/K01kibana
sudo rm -rf /var/lib/kibana
sudo rm -rf /run/systemd/propagate/kibana.service
sudo rm -rf /etc/rc3.d/S01metricbeat
sudo rm -rf /etc/rc1.d/K01metricbeat
sudo rm -rf /etc/rc6.d/K01metricbeat
sudo rm -rf /etc/rc5.d/S01metricbeat
sudo rm -rf /etc/rc4.d/S01metricbeat
sudo rm -rf /etc/rc2.d/S01metricbeat
sudo rm -rf /var/lib/logstash
sudo rm -rf /etc/default/logstash
sudo rm -rf /var/log/logstash/
sudo rm -rf /var/log/kibana/
sudo rm -rf /var/log/elasticsearch/
sudo rm -rf /var/lib/metricbeat
sudo rm -rf /var/log/metricbeat
sudo rm -rf /etc/systemd/system/multi-user.target.wants/metricbeat.service
sudo rm -rf /home/esuser/kibana1.yml
sudo rm -rf /etc/rc0.d/K01metricbeat
sudo rm -rf /etc/metricbeat
sudo rm -rf /sys/fs/cgroup/cpu,cpuacct/system.slice/metricbeat.service
sudo rm -rf /sys/fs/cgroup/pids/system.slice/metricbeat.service
sudo rm -rf /sys/fs/cgroup/memory/system.slice/metricbeat.service
sudo rm -rf /sys/fs/cgroup/blkio/system.slice/metricbeat.service
sudo rm -rf /sys/fs/cgroup/devices/system.slice/metricbeat.service
sudo rm -rf /sys/fs/cgroup/systemd/system.slice/metricbeat.service
sudo rm -rf /sys/fs/cgroup/unified/system.slice/metricbeat.service
sudo rm -rf /var/lib/dpkg/info/metricbeat*
sudo rm -rf /run/systemd/units/invocation:metricbeat.service
sudo rm -rf /etc/init.d/metricbeat
sudo rm -rf /etc/metricbeat
sudo rm -rf /usr/share/doc/metricbeat
sudo rm -rf /usr/share/metricbeat
sudo rm -rf /usr/lib/systemd/system/metricbeat.service
sudo rm -rf /usr/bin/metricbeat
sudo find / -name '*elasticsearch*'
sudo find / -name '*kibana*'
sudo find / -name '*logstash*'
sudo find / -name '*metricbeat*'
```


```
sudo apt update
```


## Install Elasticsearch 8.9
```
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.9.1-amd64.deb
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.9.1-amd64.deb.sha512
shasum -a 512 -c elasticsearch-8.9.1-amd64.deb.sha512 
sudo dpkg -i elasticsearch-8.9.1-amd64.deb
```

```
cat <<EOT >> /etc/elasticsearch/elasticsearch.yml
network.host: [_local_, _site_]
discovery.seed_hosts: ["10.0.0.10"]
transport.host: 0.0.0.0
EOT
```

```
sudo sed -i 's/xpack.security.enabled: true/xpack.security.enabled: false/' /etc/elasticsearch/elasticsearch.yml
sudo sed -i 's/xpack.security.enrollment.enabled: true/xpack.security.enrollment.enabled: false/' /etc/elasticsearch/elasticsearch.yml
```


```
cat /etc/elasticsearch/elasticsearch.yml
```

```
sudo systemctl stop elasticsearch
sudo systemctl start elasticsearch
sudo systemctl status elasticsearch
sudo systemctl enable elasticsearch
```

## Troubleshooting Elasticsearch
```
curl -X GET 'http://localhost:9200'
curl -X GET 'http://localhost:9200/_nodes?pretty'
curl -XPOST -H "Content-Type: application/json" 'http://localhost:9200/cyberithub/_doc/1' -d '{ "message": "Hello World!" }'
curl -XGET -H "Content-Type: application/json" 'http://localhost:9200/cyberithub/_doc/1'
sudo journalctl -xeu elasticsearch.service
sudo journalctl -fu elasticsearch.service
sudo tail -n 200 -f /var/log/elasticsearch/elasticsearch.log
```


## Install Kibana
```
wget https://artifacts.elastic.co/downloads/kibana/kibana-8.9.1-amd64.deb
shasum -a 512 kibana-8.9.1-amd64.deb
sudo dpkg -i kibana-8.9.1-amd64.deb
```

```
cat <<EOT >> /etc/kibana/kibana.yml
server.host: "0.0.0.0"
EOT
```

```
cat /etc/kibana/kibana.yml
```


```
sudo systemctl stop kibana
sudo systemctl status kibana
sudo systemctl start kibana
sudo systemctl status kibana
sudo systemctl enable kibana
curl localhost:5601
```


## Troubleshooting
```
sudo journalctl -xeu kibana.service
sudo journalctl -fu kibana.service
sudo tail -f /var/log/kibana/kibana.log
```


## Install Logstash (Optional)
```
wget https://artifacts.elastic.co/downloads/logstash/logstash-8.9.1-amd64.deb
shasum -a 512 logstash-8.9.1-amd64.deb
sudo dpkg -i logstash-8.9.1-amd64.deb
```

## Run Logstash
```
bin/logstash -f logstash.conf
```

```
sudo systemctl start logstash
sudo systemctl enable logstash
```

## Troubleshooting
```
curl -X GET 'http://localhost:9200'
curl -X GET 'http://localhost:9200/_nodes?pretty'
curl -XPOST -H "Content-Type: application/json" 'http://localhost:9200/cyberithub/_doc/1' -d '{ "message": "Hello World!" }'
curl -XGET -H "Content-Type: application/json" 'http://localhost:9200/cyberithub/_doc/1'
sudo journalctl -xeu elasticsearch.service
sudo journalctl -xeu kibana.service
sudo journalctl -fu elasticsearch.service
sudo journalctl -fu kibana.service
sudo tail -n 200 -f /var/log/kibana/kibana.log
sudo tail -n 200 -f /var/log/elasticsearch/elasticsearch.log
```