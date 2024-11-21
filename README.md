# Fernwartung "Anwendung und Kryptographie"
## SSH

### Server
#### Setup
```bash
$ apt update
$ apt install openssh-server

$ nano /etc/ssh/sshd_conf
#/etc/ssh/sshd_conf

PermitRootLogin no
AllowTCPForwarding yes
GatewayPorts yes
ListenAddress 127.0.0.1

# As root
$ ssh-keygen -t rsa -b 4096


$ ssh-copy-id -i /root/.ssh/id_rsa.pub <user>@<client_ip>
```
#### Autostart ssh-tunel
```bash
$ sudo nano /etc/systemd/system/ssh-tunel.service
# /etc/systemd/system/ssh-tunel.service

[Unit]
Description=SSH-Tunnel
After=network.target
Requires=network-online.target

[Service]
Type=simple
ExecStart= /usr/bin/ssh -i /root/.ssh/id_rsa -N -R 222:localhost:22 <user>@<client_ip>
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target

$ systemctl daemon-reload
$ systemctl enable ssh-tunel.service
$ systemctl start ssh-tunel.service
```

## Client
```bash
$ apt update
$ apt install openssh-server

$ nano /etc/ssh/sshd_conf
#/etc/ssh/sshd_conf

PermitRootLogin no
AllowTCPForwarding yes
GatewayPorts no

# As normal user
$ ssh-keygen -t rsa -b 4096

$ ssh -p 2222 localhost
```

## Webaccessed Desktop Environment

Requirements:
* docker
* docker-compose

```bash
$ cd webtop
$ sh ./docker-compose-helper.sh build # To build the docker image
$ sh ./docker-compose-helper.sh start # To start the docker container
$ sh ./docker-compose-helper.sh stop # To stop the docker container
```

## RDP
Mittels anydesk (Nur Xorg)

### Installation NixOS
```bash
$ nix-shell -p anydesk
$ anydesk
```

#### Client
![Client](./assets/client.png)

#### Remote Host
![Remote-Host](./assets/remote-host.png)
