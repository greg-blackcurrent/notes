
```
# First remove /etc/hostname I made it a symlink in the yocto build. This doesn't work.
rm /etc/hostname

mkdir /data/system
vi /data/system/hostname
bcedge-dignam
```

```
hostnamectl set-hostname $(< /data/system/hostname)
```

TODO what happens if the data/hostname file doesn't exist yet?

/usr/local/bin/hostname-syncer
```
# After a rootfs update, reset the hostname from the data partition on first boot.

ETC_HOSTNAME = $(cat /etc/hostname | tr -d '[:space:]')
DATA_HOSTNAME = $(cat /data/system/hostname | tr -d '[:space:]')

if [ "$ETC_HOSTNAME" != "$DATA_HOSTNAME" ]; then
    hostnamectl set-hostname $DATA_HOSTNAME
fi
```


/etc/systemd/system/hostname-syncer.service
```
[Unit]
Description=Reset the hostname after a rootfs update, to the device specific hostname from the data partition.
After=data.mount
Requires=data.mount

[Service]
Type=oneshot
ExecStart=/usr/local/bin/hostname-syncer

[Install]
WantedBy=multi-user.target
```