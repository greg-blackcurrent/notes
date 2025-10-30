/usr/share/mender/identity/mender-device-identity


```
#!/bin/sh

set -ue

# Use compulab device serial from eeprom
# https://mediawiki.compulab.com/w/index.php?title=IOT-LINK:_Linux:_How-To_Guide#Device_Serial_Number_and_Configuration
# Note we need to trim a trailing null byte

echo "serial=$(cat /proc/device-tree/baseboard-sn | tr -d '\0' )"
```
