## SSH issue:

```
sudo apt install ssh
sudo ssh-keygen -A
sudo systemctl start ssh.service
sudo systemctl status ssh.service
```

## Wifi connection issue:

### apt install connman

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

## Installing mDNS
* https://www.raspberrypi.org/forums/viewtopic.php?t=267113