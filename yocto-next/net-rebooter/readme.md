

/usr/local/bin/net-rebooter
/etc/systemd/system/net-rebooter.service
/etc/systemd/system/net-rebooter.timer


systemctl daemon-reload
systemctl enable net-rebooter.timer
systemctl start net-rebooter.timer
