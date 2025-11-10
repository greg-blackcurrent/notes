
Greg Lowe SMA login creds:

https://ennexos.sunnyportal.com/
r.AY39D3ySCmUVa
digitalteam@blackcurrent.io


Tri Power
IP 192.168.1.129
Firmware version 03.14.22.R
Serial: 3016934648
SMA3016934648

Sunny Island
Serial: 3018204976
SMA3018204976



https://my.sma-service.com/s/article/Communication-Problem-to-3rd-Party-Monitoring-Modbus-TCP?language=en_US

The inverter must have the parameter 'Modbus TCP Server On' set to YES. 

This parameter can be found under 'Device Parameters' > 'External Communication' > 'Modbus' > 'Modbus TCP Server On'

Typically, a static IP should be set within the inverter. This is configured within 'Device Parameters' > 'System Communication' > 'Speedwire'.
The inverter should have the latest firmware version installed.


https://my.sma-service.com/s/article/Setting-Static-IP-Address-on-macOS-and-iOS-Devices?language=en_US
Default IP: 169.254.12.3 (direct ethernet connection)


Manual
https://files.sma.de/assets/279469.pdf

4.5.6 Modbus
The product is equipped with a Modbus interface. The Modbus interface is deactivated by default and must be
configured as needed.
The Modbus interface of the supported SMA products is designed for industrial use – via SCADA systems, for example
– and has the following tasks:
• Remote query of measured values
• Remote setting of operating parameters
• Setpoint specifications for system control



8.1.1.3 Establishing a Connection via Ethernet in the local network



Does this work if there's DHCP?

https://sma<serial> or "https://sma<serial>.local"



https://www.youtube.com/watch?v=ZtSaUJYneK0






Single inverter, or multiple?

Sunny Tri Power X 25



Is there a "SMA Data Manager" device. Don't want it to manage modbus.


./modbus-cli -address=tcp://192.168.1.129:502 -fn-code 4 -register 30513 -quantity 4 -type-parse uint64


SMA Modbus
```
30513 Total yield RO U64 FIX0 1 AC Side Measurement.Metering.TotWhOut Wh -

30845 Current battery state of charge RO U32 FIX0 1 Battery Measurement.Bat.ChaStt % -

31397 Battery charge RO U64 FIX0 1 Battery Measurement.BatChrg.BatChrg Wh - 
31401 Battery discharge RO U64 FIX0 1 Battery Measurement.BatDsch.BatDsch Wh

31597 Battery charge RO U64 FIX0 1 Battery Measurement.BatChrg.BatChrgArr[0] Wh -
31609 Battery discharge RO U64 FIX0 1 Battery Measurement.BatDsch.BatDschArr[0] Wh -

PV?
32209 Energy released by string RO U64 FIX0 1 DC Side Measurement.DcMs.TotDcEnCntWh[0] Wh -
32213 Energy released by string RO U64 FIX0 1 DC Side Measurement.DcMs.TotDcEnCntWh[1] Wh -
32217 Energy released by string RO U64 FIX0 1 DC Side Measurement.DcMs.TotDcEnCntWh[2] Wh -

32387 Absorbed energy RO U64 FIX0 1 AC Side Measurement.Metering.TotWhIn Wh -

35417 Energy drawn at the grid connection point RO U64 FIX0 1 Grid connection Measurement.Metering.PCCMs.PlntCsmpWh Wh -
35421 Feed-in energy at the grid connection point RO U64 FIX0 1 Grid connection Measurement.Metering.PCCMs.PlntWh Wh -

SOC?
35429 Free capacity of the system RO U32 FIX0 1 Status Measurement.Plnt.TotAvalCha Wh -
35431 Retrievable charge of the system RO U32 FIX0 1 Status Measurement.Plnt.TotAvalDsch Wh -

```





SMA Sunspec

Start At:
40001

```
40210 Total yield (WH), in WhWH_SF (40212): sum of all inverters 2 acc32 RO
40212 Scale factor total yield (WH_SF): 3 1 sunssf RO
```

5.4.1 Table SMA.SSC (Storage System Control)
```
1039 Cnt.TotAcWhOut AC grid feed-in, total in 0.01 MWh 2 Int32 RO
1041 Cnt.TotAcWhOut AC grid feed-in, total in 0.01 MWh 2 Int32 RO

1059 Bsc.WhInAvail Available charging power in 0.1 kWh 2 Int32 RO
1061 Bsc.WhOutAvail Available discharging power in 0.1 kWh 2 Int32 RO

1063 Bsc.WhInAvail Available charging power in 0.1 kWh 2 Int32 RO
1065 Bsc.WhOutAvail Available discharging power in 0.1 kWh

1057 Bat.SOCConn Battery state of charge (SOC) (strings connected) in 0.1 % 2 Int32 RO

1089 Cnt.TotAcWhIn AC energy absorbed, total in 0.01 MWh 2 Int32 RO
1601 Cnt.TotAcWhOut Energy meter Battery charging 4 Uint64 RO
1605 Cnt.TotAcWhIn Energy meter Battery discharging 4 Uint64 RO
```





Page offset: Page 701
TODO injected vs absorbed?
```
19		TotWhInj			uint64		TotWh_SF	Wh				Total Energy Injected	Total active energy injected (Quadrants 1 & 4).		
23		TotWhAbs			uint64		TotWh_SF	Wh				Total Energy Absorbed	Total active energy absorbed (Quadrants 2 & 3).		
```




Article:
https://www.sma-sunny.com/en/how-i-customized-my-inverter-monitoring-via-modbus/