
Cisco Docs for jasper
https://developer.cisco.com/docs/control-center/getting-started/


Before

```
7: eth0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc mq state DOWN group default qlen 1000
    link/ether 00:01:c0:3c:6d:c3 brd ff:ff:ff:ff:ff:ff
8: wwan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UNKNOWN group default qlen 1000
    link/ether 22:89:84:6a:96:ab brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.100/24 metric 20 brd 192.168.0.255 scope global dynamic wwan0
       valid_lft 86319sec preferred_lft 86319sec
    inet6 fe80::2089:84ff:fe6a:96ab/64 scope link proto kernel_ll 
       valid_lft forever preferred_lft forever
```

```
root@bcedge-zero:~# wget blackcurrent.io
Connecting to blackcurrent.io (13.226.59.50:80)
Connecting to blackcurrent.io (13.226.59.50:443)
wget: note: TLS certificate validation not implemented
saving to 'index.html'
index.html           100% |******************************************************************************************************************************************************************| 19704  0:00:00 ETA
'index.html' saved
```

After
```
7: eth0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc mq state DOWN group default qlen 1000
    link/ether 00:01:c0:3c:6d:c3 brd ff:ff:ff:ff:ff:ff
8: wwan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UNKNOWN group default qlen 1000
    link/ether 22:89:84:6a:96:ab brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.100/24 metric 20 brd 192.168.0.255 scope global dynamic wwan0
       valid_lft 86263sec preferred_lft 86263sec
    inet6 fe80::2089:84ff:fe6a:96ab/64 scope link proto kernel_ll 
       valid_lft forever preferred_lft forever
```


Still connected?

minicom -D /dev/ttyACM2

```
AT+CSQ 
+CSQ: 14,0

OK
```

With aerial
```
AT+CSQ
+CSQ: 28,0

OK
```

```
Signal Strength:

<rssi> 0 -113 dBm or less
1 -111 dBm
2…30 -109…
-53 dBm
31 -51 dBm or greater
99 not known or not detectable


Error Rate:
<ber> (in percent)
0 <0.01%
1 0.01% --- 0.1%
2 0.1% --- 0.5%
3 0.5% --- 1.0%
4 1.0% --- 2.0%
5 2.0% --- 4.0%
6 4.0% --- 8.0%
7 >=8.0%
99 not known or not detectable
```

Popped up in console
```
[ 2516.159657] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 6784 ms
[ 2574.242707] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 8480 ms
```

AGAIN 
dmesg
```
 [   38.796046] audit: type=1334 audit(1761608270.323:18): prog-id=17 op=UNLOAD
[ 1231.066862] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 7392 ms
[ 1310.955287] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 7320 ms
[ 1406.975084] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 6976 ms
[ 1529.880411] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 8896 ms
[ 1610.024910] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 9080 ms
[ 1657.906851] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 8704 ms
[ 1743.172387] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 6912 ms
[ 1887.073998] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 9324 ms
[ 2009.979357] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 8192 ms
[ 2143.126775] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 7040 ms
[ 2218.150228] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 10304 ms
[ 2266.032136] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 7936 ms
[ 2367.172941] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 6560 ms
[ 2538.216188] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 10464 ms
[ 2645.246250] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 8840 ms
[ 2820.386378] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 8672 ms
[ 2954.301930] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 8288 ms
[ 3370.387681] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 8608 ms
[ 3482.282735] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 5664 ms
[ 3605.444120] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 8576 ms
[ 3701.463908] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 10560 ms
[ 3823.601076] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 7360 ms
[ 3962.381709] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 8512 ms
[ 4106.539376] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 10336 ms
[ 4181.562836] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 7808 ms
[ 4330.585551] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 5080 ms
[ 4458.611931] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 10076 ms
[ 4580.749147] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 8160 ms
[ 4687.779202] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 8576 ms
[ 4853.701358] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 5340 ms
[ 4981.727733] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 9312 ms
[ 5103.864943] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 7424 ms
[ 5226.770231] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 9568 ms
[ 5380.914042] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 8672 ms
[ 5461.826673] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 6560 ms
[ 5514.829591] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 9312 ms
[ 5604.960205] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 7168 ms
[ 5738.875759] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 6784 ms
[ 5882.777426] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 9204 ms
[ 6015.924852] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 8064 ms
```


"public ip"
```
7: eth0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc mq state DOWN group default qlen 1000
    link/ether 00:01:c0:3c:6d:c3 brd ff:ff:ff:ff:ff:ff
8: wwan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UNKNOWN group default qlen 1000
    link/ether 22:89:84:6a:96:ab brd ff:ff:ff:ff:ff:ff
    inet 100.70.133.23/8 metric 20 brd 100.255.255.255 scope global dynamic wwan0
       valid_lft 85956sec preferred_lft 85956sec
    inet6 fe80::2089:84ff:fe6a:96ab/64 scope link proto kernel_ll 
       valid_lft forever preferred_lft forever
```


Can I get more logs from the modem?



```
minicom -D /dev/ttyACM2
AT+DIALMODE=0
AT$MYCONFIG="usbnetmode",1
```

1,1 doesn't work

reboot? Maybe needs hard power off?

```
AT+USBNETIP=0,69
```


Name is now eth1


Looks ok?
```
[    7.673454] cdc_acm 1-1:1.2: ttyACM0: USB ACM device
[    7.813091] cdc_ether 1-1:1.0 eth1: register 'cdc_ether' at usb-ci_hdrc.0-1, CDC Ethernet Device, 20:89:84:6a:96:aa
[    7.819708] usbcore: registered new interface driver cdc_ether
[    8.109421] cdc_acm 1-1:1.4: ttyACM1: USB ACM device
[    8.456612] cdc_acm 1-1:1.6: ttyACM2: USB ACM device
[    8.795331] cdc_acm 1-1:1.8: ttyACM3: USB ACM device
[    8.800438] usbcore: registered new interface driver cdc_acm
[    8.800449] cdc_acm: USB Abstract Control Model driver for USB modems and ISDN adapters
```


Update from today's modem debugging.

* I'm able to simulate disconnects. Thanks to the bucket.

* Patient Zero generally reconnects after a network outage. But occasionally it doesn't. It's a bit hard to reproduce.
 But I did manage to capture some odd kernel logs that point to an issue in the NDIS driver.

```
[ 1231.066862] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 7392 ms
```

* I switched Patient Zero over to using the CDC ethernet driver. Seems to work ok. I will leave that running for a bit. 
  Along with some more bucket testing.


Tomorrow:

* I'm going to write up an email to Compulab to talk about this stuff.

* Write a script to reboot if network is down.

* Perhaps switch Thomas' modem to use the CDC ethernet driver.
