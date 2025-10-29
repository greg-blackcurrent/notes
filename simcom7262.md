
Consider testing Telit modems if possible too.



https://gist.github.com/westh/8a39a86bbd20089d867db8ed7a0c6adc
```
minicom -D /dev/ttyACM2
```

From Linux doc:
```
AT$MYCONFIG?
$MYCONFIG: "usbnetmode",0
```

```
AT+DIALMODE
```

From linux doc:
```
The Auto Dial status Enable/Disable, the default value is 1.
0 –Enable USBNET network
1 –Disable USBNET network
The function will take effect immediately.
```


From linux doc:

I assume they're disabling USB NET mode to toggle the config.
Does it get automatically re-enabled?

```
3.4 ECM Dial Up
Enable ECM mode and auto dial up.
# cat /dev/ttyUSB1 &
# echo -e "AT+DIALMODE=0\r\n">/dev/ttyUSB1
#
# OK
# echo -e "AT\$MYCONFIG=\"usbnetmode\"
,1 ">/dev/ttyUSB1
#
# OK
```

Only useful for interactive debugging.
```
ATI
Manufacturer: SIMCOM INCORPORATED
Model: SIM7672G-MNGV
Revision: V1.9.05
IMEI: 865031063845878

OK
```

```
AT&V
&F: 0; &W: 0; E: 1; Q: 0; &C: 1; &D: 0; V: 1; Z: 0; +IPR: 115200; +IPREX: 11520 
+CGDCONT: 1,"IP","internet","100.73.19.33",,,,,,,,,,,;
```

```
AT+CEREG?
+CEREG: 0,1
```

Apparently the zero means it won't automatically reconnect if the connection is lost.

```
AT+CEREG=1
```
This command enables Unsolicited Response Codes (URCs) for the packet-switched network status. Basically, this tells the
modem to respond when the network status changes, without having to check connectivity using AT+CEREG?.
https://onomondo.com/blog/at-command-cereg/#at-cereg-1-2


```
AT+CGDCONT?
+CGDCONT: 1,"IP","internet","100.73.19.33",,,,,,,,,,,
```

Other things to query about state:
```
AT+CGATT?
```


GPS - no data received. Perhaps because inside a concrete building?
```
AT
OK
AT+CGNSSPWR?
+CGNSSPWR: 0

OK
AT+CGNSSPWR=1
OK
AT+CGNSSPWR?
+CGNSSPWR: 1

OK
AT+CGPSCOLD
OK
AT+CGNSSINFO?
+CGNSSINFO: 0

OK
AT+CGNSSINFO
+CGNSSINFO: ,,,,,,,,

OK
```

Similar docs here:
https://www.waveshare.com/wiki/A7600X_RNDIS_Dail-up_Networking




Older version more detailed document (7672 is supposed to be compatible with 800 series):
https://cdn-shop.adafruit.com/datasheets/sim800_series_at_command_manual_v1.01.pdf


---------------------------------------------------------


The $\text{AT\&V}$ command in your SIMCom SIM7672 modem output provides a summary of the **active configuration profiles** and the current settings, which are crucial for debugging. The command essentially reports the values stored in the $\text{S}$-registers and settings that control the modem's behavior.

Here is the explanation of your specific output:

---

## ⚙️ AT&V Command Output Breakdown

The $\text{AT\&V}$ output is divided into two main parts: the core **Modem Configuration** (similar to classic Hayes modem commands) and the **Extended GSM/LTE Configuration** (using $\text{+C...}$ commands).

### 1. Core Modem Configuration

This section lists the settings that govern basic serial communication and command echo:

| Setting | Value | Description | Debugging Relevance |
| :--- | :--- | :--- | :--- |
| **`&F: 0`** | **0** | **Factory Defined Configuration:** Indicates the modem is **not** currently using the default factory profile. | Usually set to **1** (loading the default profile) or **0** (using a user-defined profile). |
| **`&W: 0`** | **0** | **Write to User Profile:** Indicates a configuration change has **not** been saved to a non-volatile user profile. | A value of **1** would mean the settings were saved. |
| **`E: 1`** | **1** | **Command Echo:** $\text{ATE1}$ - **Command echo is ON** (commands sent to the modem will be echoed back). | This is usually desirable for debugging. $\text{ATE0}$ disables it. |
| **`Q: 0`** | **0** | **Result Code Suppression:** $\text{ATQ0}$ - **Result codes are enabled** (e.g., $\text{OK}$, $\text{ERROR}$). | Essential for knowing the outcome of commands. $\text{ATQ1}$ suppresses them. |
| **`&C: 1`** | **1** | **DCD (Data Carrier Detect):** $\text{AT\&C1}$ - $\text{DCD}$ reflects the **actual data connection** status. | The $\text{DCD}$ pin will be **high** when a data connection is active. |
| **`&D: 0`** | **0** | **DTR (Data Terminal Ready):** $\text{AT\&D0}$ - $\text{DTR}$ pin changes are **ignored** by the modem. | The modem will not enter command mode or reset based on the host's $\text{DTR}$ line. |
| **`V: 1`** | **1** | **Result Code Format:** $\text{ATV1}$ - **Verbose result codes** (full words like $\text{OK}$, $\text{ERROR}$). | Recommended for readability and debugging. $\text{ATV0}$ gives numeric codes. |
| **`Z: 0`** | **0** | **Reset to Profile:** Indicates the modem was **not** reset to a stored user profile on boot. | $\text{ATZ0}$ means reset was a default power-on or hard reset. |

### 2. Extended GSM/LTE Configuration

This section contains settings for important communication parameters and the Packet Data Protocol ($\text{PDP}$) context:

| Setting | Value | Description | Debugging Relevance |
| :--- | :--- | :--- | :--- |
| **`+IPR: 115200`** | **115200** | **Serial Port Baud Rate:** The $\text{UART}$ baud rate is set to **115200 bps**. | Verify your host system's serial terminal matches this rate. |
| **`+IPREX: 11520`** | **11520** | **Local Rate (Auto-Baud):** This is typically the minimum rate used when **auto-baud detection** is running. | Indicates the lower bound for automatic speed detection on startup. |
| **`+CGDCONT: 1,"IP","internet","100.73.19.33",,,,,,,,,,,;`** | See below | **PDP Context Definition:** This is the most critical part for data connectivity. | Essential for GPRS/LTE data connections. |

#### Detailed Breakdown of +CGDCONT

The $\text{+CGDCONT}$ command defines the Packet Data Protocol context, which is necessary to connect to the cellular data network:

* **`1`**: This is the **Context Identifier (CID)**. Data commands use this ID to specify which connection profile to use.
* **`"IP"`**: This is the **PDP Type**, meaning the connection is for the **Internet Protocol ($\text{IPv4}$)**.
* **`"internet"`**: This is the **Access Point Name (APN)**. This must be correctly set to the APN provided by your $\text{SIM}$ card's cellular operator.
* **`"100.73.19.33"`**: This is the **IP Address**. This is the $\text{IP}$ address **assigned to the modem** by the network when the $\text{PDP}$ context was last activated.
    * **Note:** The $\text{100.x.x.x}$ address range often indicates a **Carrier-Grade NAT ($\text{CGNAT}$)** network, meaning your modem does not have a public, routable $\text{IP}$ address.

**In summary, your SIMCom modem is configured for data communication at 115200 bps, with standard echo and result codes enabled, and it has successfully connected (or previously connected) using the "internet" APN, having been assigned the private IP address 100.73.19.33.**


---------------------------------------------------------

The AT command response:

**AT+CEREG?**
**+CEREG: 0,1**

breaks down as follows:

* **`AT+CEREG?`**: This is the AT command to *query* the **Evolved Packet System (EPS) Network Registration Status**. This status applies to LTE (4G) and newer technologies (like NB-IoT or LTE-M).
* **`+CEREG: <n>,<stat>`**: This is the format of the response.

### The first parameter: `<n>` is **0**

* **`<n>` = 0**: This indicates the modem's setting for Unsolicited Result Codes (URCs) for network registration status is **disabled**.
    * This means the modem will *not* automatically send a `+CEREG:` message when the network registration status changes (e.g., if you lose service). You have to manually query it with `AT+CEREG?`.

### The second parameter: `<stat>` is **1**

* **`<stat>` = 1**: This indicates the current **Network Registration Status** is **Registered, home network**.
    * The device is successfully attached to the Evolved Packet System (LTE/4G) network and is operating on its home Public Land Mobile Network (PLMN), not a roaming one.

**In summary, the response means: The modem is successfully registered to its home LTE/4G network, and automatic network status updates (URCs) are disabled.**


----

A76XX ?? vs SIM7672X


SIM7672 series adopts LCC+LGA form factor and **is compatible with A7672 series** (Cat1 modules), SIM7000/SIM7070 series (NB/Cat M modules), and SIM800/SIM800F series (2G modules), which enables smooth migration from 2G/NB/Cat M products to LTE Cat 1 products and greatly facilitates more compatible product design for the customer needs.

```
3.1.2 AT$MYCONFIG Set RNDIS/ECM Mode
```

```
AT+USBNETIP=?
```


-------

`AT&F` Set all current parameters to manufacturer defaults



https://onomondo.com/blog/at-commands-guide-for-iot-devices/#what-are-at-commands




Full docs here:
https://techship.com/product/sim-com-sim-7672-g-mngv-lte-cat-1bis-m-pc-ie/?variant=001&tab=technical-documentation&page=1



https://techship.com/product/sim-com-sim-7672-g-mngv-lte-cat-1bis-m-pc-ie/?variant=001&tab=faq&page=1




http://www.linux-usb.org/usbnet/


journalctl --dmesg

So I can probably see the logs from the time of the outage.



TODO is this the actual model number - perhaps double check stickers?

Simcom SIM7262G uses a Qualcomm QCX216 chipset.

https://en.simcom.com/product/SIM7672.html

SIM767XX Series_Linux USB_Application_Note_V1.00





Gemini says:
The Qualcomm QCX216 is an LTE modem chipset, not a direct consumer-facing device that requires a separate driver 
download. The driver support is integrated within the firmware of the modules that use the QCX216 and is often managed 
through standard Linux modem interfaces.

Because the QCX216 is a component of a larger cellular module, the specific driver implementation depends on the module 
manufacturer, such as SIMCom or Quectel.

How the QCX216 works with Linux USB CDC-ACM/QMI drivers: Modules built around the QCX216 typically connect to a host 
system (like a PC or single-board computer) via USB. They emulate standard devices that are supported by the Linux 
kernel out of the box, such as: 

USB CDC-ACM: This emulates a serial port interface for communication via AT commands.

QMI (Qualcomm Mobile Interface) WWAN: This is a more advanced protocol that exposes the modem as a network interface. 
Linux kernels typically include the qmi_wwan driver to handle this. You may need to use a tool like libqmi-glib to fully
utilize the QMI interface for modem control.

Built-in network management: Because the required drivers are often included in the kernel, the modem is managed using 
standard Linux network tools. These include:
ModemManager: This daemon and its command-line interface, mmcli, can detect, configure, and control the modem.
NetworkManager: You can use the nmcli command-line tool to configure the connection and manage the APN settings.

https://docs.sixfab.com/page/setting-up-a-data-connection-over-qmi-interface-using-libqmi




You can find the driver your Simcom modem is using by checking your system's hardware and network interface details. 
Most modern Simcom modems, including the SIM7672, connect via USB and are often automatically handled by standard Linux 
kernel modules.

The most common drivers for this type of modem are:

* **qmi\_wwan**: A popular driver for Qualcomm-based modems (which many Simcom modules use) that exposes a `wwan0` network interface.
* **cdc\_ether** or **cdc\_ncm**: Drivers that make the modem appear as a standard USB Ethernet adapter (e.g., `usb0` or `ethX`).
* **option**: An older, versatile driver for USB serial modems.

Here are the commands to identify the exact driver on your device.

-----

## Step 1: Find the Network Interface

First, identify the network interface name created by the modem.

1.  Make sure the modem is plugged in and powered on.
2.  Run the `ip a` or `ifconfig` command to list all network interfaces.
3.  Look for an interface that isn't your regular Ethernet (`eth0`) or Wi-Fi (`wlan0`). Common names for cellular modems are **`wwan0`**, **`usb0`**, or sometimes a new `ethX` interface.

<!-- end list -->

```bash
# Example output to look for
$ ip a
...
4: wwan0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UNKNOWN group default qlen 1000
    link/none 
    inet 192.168.10.15/24 brd 192.168.10.255 scope global wwan0
       valid_lft forever preferred_lft forever
...
```

In this example, the interface is `wwan0`.

-----

## Step 2: Identify the Driver

Once you have the interface name, use one of these commands to find the driver managing it.

### Method 1: Using `lshw` (Recommended)

This is often the easiest and clearest method. Run this command to list network devices and their drivers.

```bash
sudo lshw -C network
```

Look for the section matching your interface name (e.g., `wwan0`). The driver will be listed under the `configuration:` line.

**Example Output:**

```
  *-network
       description: Wireless interface
       physical id: a
       bus info: usb@1:1.4
       logical name: wwan0
       serial: 0c:12:34:56:78:9a
       capabilities: ethernet physical
       configuration: broadcast=yes driver=qmi_wwan driverversion=5.15.0-52-generic firmware=...
```

Here, you can clearly see **`driver=qmi_wwan`**.

### Method 2: Using `ethtool`

If `lshw` isn't available, `ethtool` can also show driver information. Replace `wwan0` with your interface name.

```bash
ethtool -i wwan0
```

**Example Output:**

```
driver: qmi_wwan
version: 5.15.0-52-generic
firmware-version: 
expansion-rom-version: 
bus-info: 1-1.4
supports-statistics: no
...
```

### Method 3: Checking Kernel Messages (`dmesg`)

You can also check the kernel log, which records what happens when a device is plugged in. This is very useful for seeing which driver "claimed" the device.

```bash
dmesg | grep -i "simcom"
dmesg | grep -i "ttyUSB"
dmesg | grep -i "wwan"
```

Look for lines that mention your interface name or a kernel module.

**Example Output:**

```
[ 3456.123456] usb 1-1.4: qmi_wwan 1-1.4:1.4: wwan0: register 'qmi_wwan' at usb-0000:00:14.0-1.4, WWAN/QMI device
[ 3456.123457] usb 1-1.4: cdc_wdm0: 'Simcom SIM7672'
```

This output shows the **`qmi_wwan`** driver creating the **`wwan0`** interface. It also shows the modem exposing serial ports (like `/dev/ttyUSB0`, `/dev/ttyUSB1`, etc.) which are typically handled by the **`option`** or **`qcserial`** driver for AT commands.



Kernel logs dmesg
```
[    7.073364] usbcore: registered new interface driver cdc_ether
[    7.496847] cdc_acm 1-1:1.2: ttyACM0: USB ACM device
[    7.508528] rndis_host 1-1:1.0: rndis media connect
[    7.786929] rndis_host 1-1:1.0 usb0: register 'rndis_host' at usb-ci_hdrc.0-1, RNDIS device, 22:89:84:6a:96:ab
[    7.787491] usbcore: registered new interface driver rndis_host
[    8.184683] cdc_acm 1-1:1.4: ttyACM1: USB ACM device
[    8.192233] audit: type=1334 audit(1761435439.290:13): prog-id=17 op=LOAD
[    8.493789] cdc_acm 1-1:1.6: ttyACM2: USB ACM device
[    8.765372] cdc_acm 1-1:1.8: ttyACM3: USB ACM device
[    8.765554] usbcore: registered new interface driver cdc_acm
[    8.765561] cdc_acm: USB Abstract Control Model driver for USB modems and ISDN adapters
[    8.789632] rndis_host 1-1:1.0 wwan0: renamed from usb0
```


TODO check dmesg logs around time of outage.





Prompt: Is it possible to use cdc_ether with this sim7672 modem?

Yes, it is very likely possible, and your modem's specification sheet confirms it.

Your `dmesg` log shows the **`rndis_host`** driver is being used. This driver handles the RNDIS (Remote NDIS) protocol, which is a Microsoft-proprietary way of making a USB device act like an Ethernet card.

The **`cdc_ether`** driver is used for the open standard equivalent, called **CDC-ECM** (Ethernet Control Model).

According to the official SIMCom specification sheet for the SIM7672X series, the modem supports both **RNDIS** and **ECM** (which `cdc_ether` handles). Your modem is simply defaulting to RNDIS mode.

-----

## How to Switch from RNDIS to CDC-Ether (ECM)

You can change the modem's USB mode by sending it a specific AT command. The command for most modern Simcom modules is `AT+CUSBCFG` or a variant.

### Step 1: Connect to the Modem's AT Port

Your `dmesg` log showed several `cdc_acm` devices were created:

* `ttyACM0`
* `ttyACM1`
* `ttyACM2`
* `ttyACM3`

One of these is the port for sending AT commands (the others are for GNSS, debug, etc.). You can find the correct one by trial and error. A good guess is often `ttyACM2` or `ttyACM3`.

You can use a serial terminal program like `minicom`, `screen`, or `picocom` to connect. (If you don't have one, `picocom` is a simple one to install: `sudo apt-get install picocom`).

```bash
# Try connecting to one of the ports. 
# 115200 is a common baud rate.
picocom /dev/ttyACM2 --baud 115200
```

### Step 2: Send the AT Commands

ALL LIES!

1.  **Test the connection:** Once connected, type `AT` and press Enter. You should see `OK` in response.
2.  **Check current configuration:** Ask the modem for its current USB network mode. The command for the SIM7672 series is `AT+CUSBCFG?`.
    ```
    AT+CUSBCFG?
    +CUSBCFG: "USBNET","RNDIS"
    OK
    ```
    *(Your output might be slightly different, but it will likely indicate RNDIS).*
3.  **Change to ECM mode:** To switch to CDC-ECM (which uses `cdc_ether`), send the following command:
    ```
    AT+CUSBCFG="USBNET","ECM"
    OK
    ```
4.  **Reboot the modem:** For the change to take effect, you must power-cycle the modem or send the reboot command:
    ```
    AT+CRESET
    OK
    ```

### Step 3: Verify the New Driver

After the modem reboots, unplug it and plug it back in (or reboot your Linux device). Now, when you run `dmesg`, you should see `cdc_ether` claiming the network interface instead of `rndis_host`.




modemmanager cdc ecm
ModemManager and CDC-ECM are two different methods for managing cellular modems in Linux, and they can sometimes conflict.
What is CDC-ECM?
CDC-ECM (Ethernet Control Model) is a standard USB protocol that allows a cellular modem to present itself as a standard USB Ethernet network adapter.
Emulates an Ethernet device: With CDC-ECM, the Linux kernel's cdc_ether driver recognizes the modem as if it were a physical Ethernet card, requiring minimal configuration.
Simple connection: The modem is treated like a regular network interface. After some initial setup (like the Access Point Name), network management can be handled with standard tools, like a DHCP client.
Firewall-like behavior: In this mode, the modem often behaves like a router, with its own internal software managing the cellular connection. Your computer is simply "connected" to it via the USB-Ethernet link.
What is ModemManager?
ModemManager is a Linux system service (daemon) that provides a unified, high-level API for managing mobile broadband devices.
Vendor-independent: It can communicate with a wide range of modems using different protocols, such as QMI, MBIM, and standard AT command sets.
Automatic management: It detects modems, configures the connections, and exposes modem information (signal strength, SIM info) to desktop network managers like NetworkManager.
Control over hardware: It takes direct control of the modem's underlying communication channels, not just its network interface.
The conflict between ModemManager and CDC-ECM
A problem arises when both ModemManager and the Linux kernel's cdc_ether driver try to manage the same device.
Modem detection: If a cellular modem supports both a modem-specific protocol (like QMI) and CDC-ECM, ModemManager might recognize it as a modem and try to take control of its ports.
Interference: This can interfere with the standard network interface created by the cdc_ether driver, preventing the CDC-ECM connection from working properly.
"No modem" behavior: Since ModemManager controls the underlying device, the modem may appear as unrecognized or "missing" to the network manager, even though a network interface exists.
How to solve the conflict
If you prefer to use the simpler CDC-ECM approach, you can prevent ModemManager from interfering with your modem.
Disable or remove ModemManager: The most straightforward solution is to stop and disable the ModemManager service on your Linux system, particularly on embedded devices like a Raspberry Pi.
sh
sudo systemctl stop ModemManager.service
sudo systemctl disable ModemManager.service
Use code with caution.

Whitelist the device: In later versions of ModemManager (1.12.0+), you can use udev rules to make ModemManager ignore a specific device. This is done by setting the ID_MM_DEVICE_IGNORE udev property based on the device's Vendor ID and Product ID.
Manually blacklist the device: For older versions of ModemManager, you might need to add the device to a blacklist file to prevent it from being probed.
AI can make mistakes, so double-check responses



Linux MBIM commit. Is this the same modem?
https://github.com/torvalds/linux/commit/cc6730df08a291e51e145bc65e24ffb5e2f17ab6


root@bcedge-zero:~# lsusb
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 002: ID 05c6:9330 Qualcomm, Inc. QualComm Compo
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub


root@bcedge-zero:~# lsusb -d 05c6:9330 -v

Bus 001 Device 002: ID 05c6:9330 Qualcomm, Inc. QualComm Compo
Device Descriptor:
bLength                18
bDescriptorType         1
bcdUSB               2.00
bDeviceClass          239 Miscellaneous Device
bDeviceSubClass         2 [unknown]
bDeviceProtocol         1 Interface Association
bMaxPacketSize0        64
idVendor           0x05c6 Qualcomm, Inc.
idProduct          0x9330 QualComm Compo
bcdDevice            2.00
iManufacturer           1 QualComm
iProduct                2 QualComm Compo
iSerial                 3 000000000001
bNumConfigurations      1
Configuration Descriptor:
bLength                 9
bDescriptorType         2
wTotalLength       0x0153
bNumInterfaces         10
bConfigurationValue     1
iConfiguration          0
bmAttributes         0xa0
(Bus Powered)
Remote Wakeup
MaxPower              200mA
Interface Association:
bLength                 8
bDescriptorType        11
bFirstInterface         0
bInterfaceCount         2
bFunctionClass        224 Wireless
bFunctionSubClass       1 Radio Frequency
bFunctionProtocol       3 RNDIS
iFunction               6 rndis
Interface Descriptor:
bLength                 9
bDescriptorType         4
bInterfaceNumber        0
bAlternateSetting       0
bNumEndpoints           1
bInterfaceClass       224 Wireless
bInterfaceSubClass      1 Radio Frequency
bInterfaceProtocol      3 RNDIS
iInterface              6 rndis
** UNRECOGNIZED:  05 24 00 10 01
** UNRECOGNIZED:  05 24 01 00 01
** UNRECOGNIZED:  04 24 02 00
** UNRECOGNIZED:  05 24 06 00 01
Endpoint Descriptor:
bLength                 7
bDescriptorType         5
bEndpointAddress     0x81  EP 1 IN
bmAttributes            3
Transfer Type            Interrupt
Synch Type               None
Usage Type               Data
wMaxPacketSize     0x0008  1x 8 bytes
bInterval              14
Interface Descriptor:
bLength                 9
bDescriptorType         4
bInterfaceNumber        1
bAlternateSetting       0
bNumEndpoints           2
bInterfaceClass        10 CDC Data
bInterfaceSubClass      0 [unknown]
bInterfaceProtocol      0
iInterface              0
Endpoint Descriptor:
bLength                 7
bDescriptorType         5
bEndpointAddress     0x01  EP 1 OUT
bmAttributes            2
Transfer Type            Bulk
Synch Type               None
Usage Type               Data
wMaxPacketSize     0x0200  1x 512 bytes
bInterval               0
Endpoint Descriptor:
bLength                 7
bDescriptorType         5
bEndpointAddress     0x82  EP 2 IN
bmAttributes            2
Transfer Type            Bulk
Synch Type               None
Usage Type               Data
wMaxPacketSize     0x0200  1x 512 bytes
bInterval               0
Interface Association:
bLength                 8
bDescriptorType        11
bFirstInterface         2
bInterfaceCount         2
bFunctionClass          2 Communications
bFunctionSubClass       2 Abstract (modem)
bFunctionProtocol       1 AT-commands (v.25ter)
iFunction               7 at
Interface Descriptor:
bLength                 9
bDescriptorType         4
bInterfaceNumber        2
bAlternateSetting       0
bNumEndpoints           1
bInterfaceClass         2 Communications
bInterfaceSubClass      2 Abstract (modem)
bInterfaceProtocol      1 AT-commands (v.25ter)
iInterface              7 at
CDC Header:
bcdCDC               1.10
CDC Call Management:
bmCapabilities       0x00
bDataInterface          3
CDC ACM:
bmCapabilities       0x02
line coding and serial state
CDC Union:
bMasterInterface        2
bSlaveInterface         3
Endpoint Descriptor:
bLength                 7
bDescriptorType         5
bEndpointAddress     0x83  EP 3 IN
bmAttributes            3
Transfer Type            Interrupt
Synch Type               None
Usage Type               Data
wMaxPacketSize     0x0010  1x 16 bytes
bInterval              16
Interface Descriptor:
bLength                 9
bDescriptorType         4
bInterfaceNumber        3
bAlternateSetting       0
bNumEndpoints           2
bInterfaceClass        10 CDC Data
bInterfaceSubClass      0 [unknown]
bInterfaceProtocol      0
iInterface              0
Endpoint Descriptor:
bLength                 7
bDescriptorType         5
bEndpointAddress     0x02  EP 2 OUT
bmAttributes            2
Transfer Type            Bulk
Synch Type               None
Usage Type               Data
wMaxPacketSize     0x0200  1x 512 bytes
bInterval               0
Endpoint Descriptor:
bLength                 7
bDescriptorType         5
bEndpointAddress     0x84  EP 4 IN
bmAttributes            2
Transfer Type            Bulk
Synch Type               None
Usage Type               Data
wMaxPacketSize     0x0200  1x 512 bytes
bInterval               0
Interface Association:
bLength                 8
bDescriptorType        11
bFirstInterface         4
bInterfaceCount         2
bFunctionClass          2 Communications
bFunctionSubClass       2 Abstract (modem)
bFunctionProtocol       1 AT-commands (v.25ter)
iFunction               8 log
Interface Descriptor:
bLength                 9
bDescriptorType         4
bInterfaceNumber        4
bAlternateSetting       0
bNumEndpoints           1
bInterfaceClass         2 Communications
bInterfaceSubClass      2 Abstract (modem)
bInterfaceProtocol      1 AT-commands (v.25ter)
iInterface              8 log
CDC Header:
bcdCDC               1.10
CDC Call Management:
bmCapabilities       0x00
bDataInterface          5
CDC ACM:
bmCapabilities       0x02
line coding and serial state
CDC Union:
bMasterInterface        4
bSlaveInterface         5
Endpoint Descriptor:
bLength                 7
bDescriptorType         5
bEndpointAddress     0x85  EP 5 IN
bmAttributes            3
Transfer Type            Interrupt
Synch Type               None
Usage Type               Data
wMaxPacketSize     0x0010  1x 16 bytes
bInterval              16
Interface Descriptor:
bLength                 9
bDescriptorType         4
bInterfaceNumber        5
bAlternateSetting       0
bNumEndpoints           2
bInterfaceClass        10 CDC Data
bInterfaceSubClass      0 [unknown]
bInterfaceProtocol      0
iInterface              0
Endpoint Descriptor:
bLength                 7
bDescriptorType         5
bEndpointAddress     0x03  EP 3 OUT
bmAttributes            2
Transfer Type            Bulk
Synch Type               None
Usage Type               Data
wMaxPacketSize     0x0200  1x 512 bytes
bInterval               0
Endpoint Descriptor:
bLength                 7
bDescriptorType         5
bEndpointAddress     0x86  EP 6 IN
bmAttributes            2
Transfer Type            Bulk
Synch Type               None
Usage Type               Data
wMaxPacketSize     0x0200  1x 512 bytes
bInterval               0
Interface Association:
bLength                 8
bDescriptorType        11
bFirstInterface         6
bInterfaceCount         2
bFunctionClass          2 Communications
bFunctionSubClass       2 Abstract (modem)
bFunctionProtocol       1 AT-commands (v.25ter)
iFunction               9 ppp
Interface Descriptor:
bLength                 9
bDescriptorType         4
bInterfaceNumber        6
bAlternateSetting       0
bNumEndpoints           1
bInterfaceClass         2 Communications
bInterfaceSubClass      2 Abstract (modem)
bInterfaceProtocol      1 AT-commands (v.25ter)
iInterface              9 ppp
CDC Header:
bcdCDC               1.10
CDC Call Management:
bmCapabilities       0x00
bDataInterface          7
CDC ACM:
bmCapabilities       0x02
line coding and serial state
CDC Union:
bMasterInterface        6
bSlaveInterface         7
Endpoint Descriptor:
bLength                 7
bDescriptorType         5
bEndpointAddress     0x87  EP 7 IN
bmAttributes            3
Transfer Type            Interrupt
Synch Type               None
Usage Type               Data
wMaxPacketSize     0x0010  1x 16 bytes
bInterval              16
Interface Descriptor:
bLength                 9
bDescriptorType         4
bInterfaceNumber        7
bAlternateSetting       0
bNumEndpoints           2
bInterfaceClass        10 CDC Data
bInterfaceSubClass      0 [unknown]
bInterfaceProtocol      0
iInterface              0
Endpoint Descriptor:
bLength                 7
bDescriptorType         5
bEndpointAddress     0x04  EP 4 OUT
bmAttributes            2
Transfer Type            Bulk
Synch Type               None
Usage Type               Data
wMaxPacketSize     0x0200  1x 512 bytes
bInterval               0
Endpoint Descriptor:
bLength                 7
bDescriptorType         5
bEndpointAddress     0x88  EP 8 IN
bmAttributes            2
Transfer Type            Bulk
Synch Type               None
Usage Type               Data
wMaxPacketSize     0x0200  1x 512 bytes
bInterval               0
Interface Association:
bLength                 8
bDescriptorType        11
bFirstInterface         8
bInterfaceCount         2
bFunctionClass          2 Communications
bFunctionSubClass       2 Abstract (modem)
bFunctionProtocol       1 AT-commands (v.25ter)
iFunction              10 com
Interface Descriptor:
bLength                 9
bDescriptorType         4
bInterfaceNumber        8
bAlternateSetting       0
bNumEndpoints           1
bInterfaceClass         2 Communications
bInterfaceSubClass      2 Abstract (modem)
bInterfaceProtocol      1 AT-commands (v.25ter)
iInterface             10 com
CDC Header:
bcdCDC               1.10
CDC Call Management:
bmCapabilities       0x00
bDataInterface          9
CDC ACM:
bmCapabilities       0x02
line coding and serial state
CDC Union:
bMasterInterface        8
bSlaveInterface         9
Endpoint Descriptor:
bLength                 7
bDescriptorType         5
bEndpointAddress     0x89  EP 9 IN
bmAttributes            3
Transfer Type            Interrupt
Synch Type               None
Usage Type               Data
wMaxPacketSize     0x0010  1x 16 bytes
bInterval              16
Interface Descriptor:
bLength                 9
bDescriptorType         4
bInterfaceNumber        9
bAlternateSetting       0
bNumEndpoints           2
bInterfaceClass        10 CDC Data
bInterfaceSubClass      0 [unknown]
bInterfaceProtocol      0
iInterface              0
Endpoint Descriptor:
bLength                 7
bDescriptorType         5
bEndpointAddress     0x05  EP 5 OUT
bmAttributes            2
Transfer Type            Bulk
Synch Type               None
Usage Type               Data
wMaxPacketSize     0x0200  1x 512 bytes
bInterval               0
Endpoint Descriptor:
bLength                 7
bDescriptorType         5
bEndpointAddress     0x8a  EP 10 IN
bmAttributes            2
Transfer Type            Bulk
Synch Type               None
Usage Type               Data
wMaxPacketSize     0x0200  1x 512 bytes
bInterval               0
Device Qualifier (for other device speed):
bLength                10
bDescriptorType         6
bcdUSB               2.00
bDeviceClass          239 Miscellaneous Device
bDeviceSubClass         2 [unknown]
bDeviceProtocol         1 Interface Association
bMaxPacketSize0        64
bNumConfigurations      1
Device Status:     0x0000
(Bus Powered)
root@bcedge-zero:~# 