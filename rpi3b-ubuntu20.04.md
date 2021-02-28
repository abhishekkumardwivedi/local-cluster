### SSH issue:

```
sudo apt install ssh
sudo ssh-keygen -A
sudo systemctl start ssh.service
sudo systemctl status ssh.service
```

### Wifi connection issue:

#### apt install connman

```
$  /usr/sbin/connmanctl 
connmanctl> enable wifi
connmanctl> scan wifi 
Scan completed for wifi

connmanctl> services 
$SSID    wifi_f8d111090ed6_6d617269636f6e5f64655f6d6965726461_managed_psk
...      ...

connmanctl> agent on
Agent registered

connmanctl> connect wifi_f8d111090ed6_6d617269636f6e5f64655f6d6965726461_managed_psk 
Agent RequestInput wifi_f8d111090ed6_6d617269636f6e5f64655f6d6965726461_managed_psk
Passphrase = [ Type=psk, Requirement=mandatory, Alternates=[ WPS ] ]
WPS = [ Type=wpspin, Requirement=alternate ]
Passphrase? $PASS
Connected wifi_f8d111090ed6_6d617269636f6e5f64655f6d6965726461_managed_psk

connmanctl> quit
```

### Installing mDNS
* https://www.raspberrypi.org/forums/viewtopic.php?t=267113

Install avahi and config daemon
```
sudo apt-get install avahi-utils 
sudo vi /etc/avahi/avahi-daemon.conf
```
```
# this makes any hostname.local refer to hosts on your lan reachable via mdns
domain-name=local
# they are apparently turned off as default, i'd guess for privacy / security reasons
# this broadcast your hostname and hostinfo on the lan via mdns
publish-hinfo=yes
publish-workstation=yes
```
set hostname
```
sudo vi /etc/hostname
sudo vi /etc/hosts
# now reboot teh device
```
Test mDNS lists the found devices
```
avahi-browse -a 
```

### Creating new user in Linux

Create user and set password
```
sudo useradd abhishek
sudo passwd abhishek
```
Set user mode as root
```
sudo usermod -aG sudo abhishek
```
Add home directory for user and set
```
sudo vi /etc/passwd
```
Change `abhishek:x:1001:1001::/home/abhishek:/bin/bash` to `abhishek:x:1001:1001:,,,:/home/abhishek:/bin/bash` by adding `,,,`
Make sure the shell is `/bin/bash` not `/bin/sh` to land to home directory.
```
sudo mkdir /home/abhishek
sudo chown -R abhishek:abhishek /home/abhishek
```

### Password-less login
```
ssh-copy-id abhishek@rpi3.local
```

### Install influxdb

https://computingforgeeks.com/install-influxdb-on-debian-10-buster-linux/
```
sudo apt update
sudo apt install -y gnupg2 curl wget
wget -qO- https://repos.influxdata.com/influxdb.key | sudo apt-key add -
echo "deb https://repos.influxdata.com/debian buster stable" | sudo tee /etc/apt/sources.list.d/influxdb.list
sudo apt update
sudo apt install -y influxdb
sudo systemctl enable --now influxdb
systemctl status influxdb
# check the status
sudo apt -y install ufw
sudo ufw enable
sudo ufw allow 22/tcp
sudo ufw allow 8086/tcp
sudo vim /etc/influxdb/influxdb.conf 
[http]
 auth-enabled = true
curl -XPOST "http://localhost:8086/query" --data-urlencode "q=CREATE USER influxdb WITH PASSWORD 'password' WITH ALL PRIVILEGES"
```
