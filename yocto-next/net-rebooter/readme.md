

/usr/local/bin/net-rebooter
/usr/lib/systemd/system/net-rebooter.service
/usr/lib/systemd/system/net-rebooter.timer


systemctl daemon-reload
systemctl enable net-rebooter.timer
systemctl start net-rebooter.timer
