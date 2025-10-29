Background:

I have a deployed a IoT Link device at a site. It is periodically disconnecting from the cellular network and not 
automatically reconnecting. This happens approximately once a day. The device comes back up after a scheduled 
daily reboot.

I have reproduced the issue on a IoT Link in my office. I simulate a lost signal by removing the aerial and covering 
the device with a metal bucket. Then removing the bucket and reconnecting the aerial. Most of the time it reconnects 
automatically, but I have also seen it fail to reconnect a couple of times.

I saw this message in the kernel logs when this happened (dmesg):

```
[ 1231.066862] rndis_host 1-1:1.0 wwan0: NETDEV WATCHDOG: CPU: 0: transmit queue 0 timed out 7392 ms
```

Looking at the Simcom documentation, the device supports RNDIS and ECM drivers on Linux, but defaults 
to RNDIS.

Given the above error, and that the RNDIS driver is likely to be [removed from the kernel soon](https://www.phoronix.com/news/Linux-Disabling-RNDIS-Attempt),
I decided to try the ECM driver (aka CDC ethernet).

To do this I run the following:
```
minicom -D /dev/ttyACM2
AT+DIALMODE=0
AT$MYCONFIG="usbnetmode",1
```

I also reconfigured networking, as the virtual ethernet device is now attached to `/dev/eth1`.

Question:

Is "CDC Ethernet" the right driver to use for this Simcom modem? Are there any other options that I should consider?

I've attached the Simcom documentation that I've used.


