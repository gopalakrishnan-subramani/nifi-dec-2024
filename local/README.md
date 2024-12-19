## Setup Nifi on Local Linux

```
sudo apt update
sudo apt install openjdk-17-jdk unzip -y
```

Download and setup Nifi
```
wget https://dlcdn.apache.org/nifi/1.28.1/nifi-1.28.1-bin.zip
unzip nifi-1.28.1-bin.zip
sudo mv nifi-1.28.1 /opt/nifi
sudo useradd -r -d /opt/nifi -s /bin/false nifi
sudo chown -R nifi:nifi /opt/nifi

sudo chown -R nifi:nifi /opt/nifi
sudo chmod -R 755 /opt/nifi

ls -ld /opt/nifi
```

to change the properties

``
nano /opt/nifi/conf/nifi.properties
```

```

```
nano .bashrc
```

```
export NIFI_HOME=/opt/nifi
export PATH=$NIFI_HOME/bin:$PATH

export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
export PATH=$JAVA_HOME/bin:$PATH

```

```
cd /opt/nifi/bin
./nifi.sh start
./nifi.sh status
```

sudo nano /etc/systemd/system/nifi.service
```

```
cd /opt/nifi/bin
./nifi.sh start
./nifi.sh status
```

```
sudo nano /etc/systemd/system/nifi.service
```

```
[Unit]
Description=Apache NiFi Service
After=network.target

[Service]
User=nifi
Group=nifi
ExecStart=/opt/nifi/bin/nifi.sh start
ExecStop=/opt/nifi/bin/nifi.sh stop
Restart=on-abort

[Install]
WantedBy=multi-user.target
```

sudo systemctl daemon-reload
sudo systemctl enable nifi
sudo systemctl start nifi
sudo systemctl status nifi



Stop the NiFi service

```
sudo systemctl stop nifi
```


Disable the service from starting at boot
```
sudo systemctl disable nifi
```

Remove the systemd service file
```
sudo rm /etc/systemd/system/nifi.service
```

Reload the systemd daemon
```
sudo systemctl daemon-reload
```


Remove the NiFi installation directory

```
sudo rm -rf /opt/nifi
'''


# Remove the NiFi user
'''
sudo userdel nifi
'''

# Optional: Remove the NiFi group if it exists

'''
sudo groupdel nifi
```

Ensure the service is no longer present

```
sudo systemctl status nifi
```

Confirm the files and user have been removed
```
ls /opt | grep nifi
id nifi
```
