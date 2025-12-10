# BIOS (ASUS)

- Advance --> APM Configuration or power configuration --> Power on by PCI-E: `Enabled`

# Proxmox --> Shell

- Configuration

```
sudo apt-get Install ethtool
ip addr # note the adapter name `enp6s0`
ethtool enp6s0 # check "Suppoer Wake-on", "Wake-on"
ethtool-s enp6s0 wol g # enables Wake On LAN (WOL)
nano /etc/system/system/wol.service
```

- File: `/etc/system/system/wol.service`

```
[Unit]
Description=Enable Wake-on-LAN
After=network.target

[Service]
Type=oneshot
ExecStart=/usr/sbin/ethtool -s enp6s0 wol g

[Install]
WantedBy=multi-user.target
```

- Enable and start the service

```
sudo systemctl enable wol.service
sudo systemctl start wol.service
```

- reboot the system
