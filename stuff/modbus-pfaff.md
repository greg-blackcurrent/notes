
Set device id to 247 for section 5.1




EVCC grid-x modbus fork? Did this get upstreamed?
https://github.com/evcc-io/modbus/tree/master

Also util with unit id:
https://github.com/evcc-io/evcc/tree/dcc08ca022cd2a90fcaa36088575137ec70c72a8/util/modbus


https://github.com/evcc-io/evcc/discussions/16246

./modbus-cli -address=tcp://192.168.0.188:502 -fn-code 4 -register 31027 -quantity 1 -type-parse int16


Model type
./modbus-cli -address=tcp://192.168.0.188:502 -fn-code 4 -register 30500 -quantity 15 -type-parse raw

SigenStor EC 8.0 SP



Firmware version
./modbus-cli -address=tcp://192.168.0.188:502 -fn-code 4 -register 30525 -quantity 15 -type-parse raw

V100R001C22SPC111B064E


Phase A voltage
./modbus-cli -address=tcp://192.168.0.188:502 -fn-code 4 -register 31011 -quantity 2 -type-parse uint32
24356 / 100



[ESS] Accumulated charge energy  kwh  /100
./modbus-cli -address=tcp://192.168.0.188:502 -fn-code 4 -register 30568 -quantity 4 -type-parse uint64
199565

[ESS] Accumulated discharge energy   kwh  /100
./modbus-cli -address=tcp://192.168.0.188:502 -fn-code 4 -register 30574 -quantity 4 -type-parse uint64
191320

Running state
./modbus-cli -address=tcp://192.168.0.188:502 -fn-code 4 -register 30578 -quantity 1 -type-parse uint16

Standby 0x00
Running 0x01
Fault 0x02
Shutdown 0x03
Environmental Abnormality 0x07


[ESS]Battery SOC   %   /10
./modbus-cli -address=tcp://192.168.0.188:502 -fn-code 4 -register 30601 -quantity 1 -type-parse uint16
100%

[ESS] Charge / discharge power    kW    /1000
./modbus-cli -address=tcp://192.168.0.188:502 -fn-code 4 -register 30599 -quantity 2 -type-parse uint32
0



PV total generation   kWh  /100
./modbus-cli -address=tcp://192.168.0.188:502 -fn-code 4 -register 31511 -quantity 2 -type-parse uint32
3233.74 kWh





// Note device id 247  (not using the un-pc term)

[Grid sensor] Active power +buy -sell     kW    /1000
./modbus-cli -address=tcp://192.168.0.188:502 -slaveID 247 -fn-code 4 -register 30005 -quantity 2 -type-parse int32

Plant active power   kW     /1000
./modbus-cli -address=tcp://192.168.0.188:502 -slaveID 247 -fn-code 4 -register 30031 -quantity 2 -type-parse int32

Photovoltaic power   kW    /1000
./modbus-cli -address=tcp://192.168.0.188:502 -slaveID 247 -fn-code 4 -register 30035 -quantity 2 -type-parse int32

[ESS] Power +charging -discharging    kW    /1000
./modbus-cli -address=tcp://192.168.0.188:502 -slaveID 247 -fn-code 4 -register 30037 -quantity 2 -type-parse int32




System Time (use as backup clock)
./modbus-cli -address=tcp://192.168.0.188:502 -slaveID 247 -fn-code 4 -register 30000 -quantity 2 -type-parse uint32

System Time Zone 
./modbus-cli -address=tcp://192.168.0.188:502 -slaveID 247 -fn-code 4 -register 30002 -quantity 1 -type-parse uint16
(12 hours)


Plant PV total generation    kWh   /100
./modbus-cli -address=tcp://192.168.0.188:502 -slaveID 247 -fn-code 4 -register 30088 -quantity 4 -type-parse uint64

Total load consumption      kWh    /100
./modbus-cli -address=tcp://192.168.0.188:502 -slaveID 247 -fn-code 4 -register 30094 -quantity 4 -type-parse uint64

Total charged energy of the ESS   kWh  /100
./modbus-cli -address=tcp://192.168.0.188:502 -slaveID 247 -fn-code 4 -register 30200 -quantity 4 -type-parse uint64

Total discharged energy of the ESS   kWh /100
./modbus-cli -address=tcp://192.168.0.188:502 -slaveID 247 -fn-code 4 -register 30204 -quantity 4 -type-parse uint64

Total imported energy     kWh   /100 
./modbus-cli -address=tcp://192.168.0.188:502 -slaveID 247 -fn-code 4 -register 30216 -quantity 4 -type-parse uint64

Total exported  energy   kWh   /100
./modbus-cli -address=tcp://192.168.0.188:502 -slaveID 247 -fn-code 4 -register 30220 -quantity 4 -type-parse uint64

[ESS] SOC   %   /10
./modbus-cli -address=tcp://192.168.0.188:502 -slaveID 247 -fn-code 4 -register 30014 -quantity 1 -type-parse uint16
