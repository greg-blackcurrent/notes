/etc/udev/rules.d/60-usbnet0.rules

SUBSYSTEM=="net", ACTION=="add", DRIVERS=="cdc_ether", KERNEL=="eth*", NAME="usbnet0"


