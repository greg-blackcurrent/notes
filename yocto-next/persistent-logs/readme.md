Logs aren’t stored persistently atm. We want to persist them across reboots so we can diagnose problems.

We need to limit the size to make sure it doesn’t get too big. What is a good size limit? Do we need to increase the size of our rootfs partitions?

Docs are here:
journald.conf(5) - Linux manual page

I’ve applied this manually to Thomas' machine for now. I need to add this to the yocto build.


TODO move this to usr/lib ?

/etc/systemd/journald.conf
```
Storage=persistent
SystemMaxUse=300M
MaxRetentionSec=1week
ForwardToSyslog=no 
```

Also remove this extra config

```
rm /usr/lib/systemd/journald.conf.d/00-systemd-conf.conf
```

```
systemctl restart systemd-journald
journalctl -u systemd-journald
```


Fix for syslog issues - along with ForwardToSyslog=no above.

```
systemctl stop syslog.socket
systemctl disable busybox-syslog.service
systemctl stop busybox-syslog.service
```

We also need to change the log storage. Currently /var/log is symlinked to /var/volatile/log which gets cleared on reboot.

There is an openembedded flag for the yocto build, that might change this.

We also need to remove busybox syslog package.


Make logs persistent.
```
systemctl stop systemd-journald
cd /var
rm log
mv volatile/log log
systemctl start systemd-journald
```

To test reboot twice. Then run
```
journalctl --list-boots
```