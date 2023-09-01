# Install Elastic Search

```
sudo apt update
```


```
curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```

```
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
```

```
sudo apt update
```

```
sudo apt install elasticsearch -y
```


```
sudo apt install kibana -y
```

```
sudo systemctl enable kibana
```

```
sudo systemctl start kibana
```

```
sudo apt install logstash -y
```

```
#sudo nano /etc/logstash/conf.d/02-beats-input.conf
```

```
#sudo nano /etc/logstash/conf.d/30-elasticsearch-output.conf
```

```
sudo -u logstash /usr/share/logstash/bin/logstash --path.settings /etc/logstash -t
```

```
sudo systemctl start logstash
sudo systemctl enable logstash
```
