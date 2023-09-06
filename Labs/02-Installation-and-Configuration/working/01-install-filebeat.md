sudo apt install -y nginx

curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-8.9.1-amd64.deb
sudo dpkg -i filebeat-8.9.1-amd64.deb

filebeat modules list

filebeat modules enable nginx

nano /etc/filebeat/filebeat.yml

filebeat setup -e

sudo systemctl stop filebeat
sudo systemctl start filebeat

tail -f /var/log/filebeat/filebeat.log

