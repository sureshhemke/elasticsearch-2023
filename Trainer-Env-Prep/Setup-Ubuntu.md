# Trainer Tasks

```
sudo apt -y update
sudo apt -y install tree git openssl software-properties-common curl wget
```

## Setup multiple users in Ubuntu
- For each participant, we need to setup login accounts
```
sudo groupadd docker
for ((i=1;i<=9;i++)); do
	export username="u0$i"
	sudo useradd -m -p "p2" $username;sudo usermod -aG sudo $username;sudo usermod -aG docker $username;echo $username:p | sudo /usr/sbin/chpasswd;sudo chown -R  $username:root /home/$username
done
for ((i=10;i<=20;i++)); do
	export username="u$i"
	sudo useradd -m -p "p2" $username;sudo usermod -aG sudo $username;sudo usermod -aG docker $username;echo $username:p | sudo /usr/sbin/chpasswd;sudo chown -R  $username:root /home/$username
done
```

-  Clone Git Repo
```
cd
git clone
cd
```

- Customize linux prompt
```
cat ~/.bashrc
echo 'export PS1="\e[0;31m\e[50m\u@\h\n\w \e[m\n$ "'   >> ~/.bashrc
cat ~/.bashrc
exit
```


- Enable swap space
```
sudo su
cat <<EOT >> /var/lib/cloud/scripts/per-boot/create_swapfile.sh
#!/bin/sh
if [ ! -f '/mnt/swapfile' ]; then
fallocate --length 2GiB /mnt/swapfile
chmod 600 /mnt/swapfile
mkswap /mnt/swapfile
swapon /mnt/swapfile
swapon -a 
else
swapon /mnt/swapfile; fi
EOT
```

```
sudo chmod a+x /var/lib/cloud/scripts/per-boot/create_swapfile.sh
```

```
sudo reboot
```

```
free -m
```