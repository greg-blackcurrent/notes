
/etc/systemd/system/reboot.timer
```
[Unit]
Description=Reboot daily

[Timer]
# System is using UTC time so this is either 7am, or 8am depending on daylight savings.
# 10 seconds after the hour, so that we get some time to send the message for the sample made at 19:00.
OnCalendar=*-*-* 19:00:10

[Install]
WantedBy=timers.target
```

/etc/systemd/system/reboot.service
```
[Unit]
Description=Reboot

[Service]
Type=oneshot
ExecStart=/sbin/shutdown -r now
```

TODO use /bin/systemctl something to reboot instead?

```
# load config
systemctl daemon-reload

# start timer on next boot
systemctl enable reboot.timer

# start the timer now
systemctl start reboot.timer
```
