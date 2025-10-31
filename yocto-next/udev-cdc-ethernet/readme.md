/etc/udev/rules.d/60-usbnet0.rules  and existing wwan rule.

```
SUBSYSTEM=="net", ACTION=="add", DRIVERS=="cdc_ether", KERNEL=="eth*", NAME="usbnet0"
```

vi /etc/udev/rules.d/70-wiredeth0.rules

```
SUBSYSTEM=="net", ACTION=="add", DRIVERS=="imx-dwmac", NAME="wiredeth0"
```

Remember to rename eth0.network to 20-wiredeth0.network


Also need a new network config:

more /usr/lib/systemd/network/10-usbnet0.network
```
[Match]
Name=usbnet0

[Network]
DHCP=yes

[DHCP]
UseMTU=yes
RouteMetric=10
ClientIdentifier=mac
```






# Run book - for Thomas swap

Check: 
 * is udev rule ok?
 * is network config file ok?


Do driver swap:
```
minicom -D /dev/ttyACM2
```

Careful copy and paste
```
AT$MYCONFIG?
```


Set ECM, and "public" IP address
```
AT$MYCONFIG="usbnetmode",1
AT+USBNETIP=1
```

Ctrl-a Q

TODO I think public IP means. Set the IP of the cdc_ether device to match that of the actual wwan device on the 
telcos network.

We could also set this to private. `AT+USBNETIP=1,25` this results in a IP of `192.168.25.100`. The 25 needs to be 
chosen to avoid conflicts with the modbus network.


GPIO reset
```
gpioset -c 0 --toggle 0 22=1; sleep 0.3 ; gpioset -c 0 --toggle 0 22=0
```

Postmortem: perhaps need a reboot on the end. And a nohup to keep it running.
```
nohup gpioset -c 0 --toggle 0 22=1; sleep 0.3 ; gpioset -c 0 --toggle 0 22=0; reboot
```

TODO systemctl reboot (instead?)


Tab dies.

Check dmesg. Expect:

```
dmesg | tail -n 20
```

Expect
```
[    7.055571] cdc_ether 1-1:1.0 eth1: register 'cdc_ether' at usb-ci_hdrc.0-1, CDC Ethernet Device, 20:89:84:6a:96:aa
[    7.055755] usbcore: registered new interface driver cdc_ether
[    7.525554] cdc_acm 1-1:1.2: ttyACM0: USB ACM device
[    7.811040] cdc_acm 1-1:1.4: ttyACM1: USB ACM device
[    8.092020] cdc_acm 1-1:1.6: ttyACM2: USB ACM device
[    8.094179] cdc_ether 1-1:1.0 usbnet0: renamed from eth1
[    8.393929] cdc_acm 1-1:1.8: ttyACM3: USB ACM device
[    8.394443] usbcore: registered new interface driver cdc_acm
[    8.394454] cdc_acm: USB Abstract Control Model driver for USB modems and ISDN adapters
```

This should be empty:
```
dmesg | grep ndis
```


Check `ip a`:

Expect `usbnet0`

`ping 8.8.8.8`


Failed after GPIO reset. Probably need to add a reboot to the end