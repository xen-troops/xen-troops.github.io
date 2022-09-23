# Control phidget

Phidget, connected to PC, is present in system as `ttyUSBx`, so you may use dmesg to clarify which device is assigned.

Before usage, phidget have to be initialized
```
$ echo -n -e "\x50" > /dev/ttyUSB1
$ echo -n -e "\x51" > /dev/ttyUSB1
```

After that you can turn relay ON or OFF by setting corresponding bit starting from 0.

Below we
* turn on relay 1
* turn on relays 1 and 2
* turn off both relays
```
$ echo -n -e "\x1" > /dev/ttyUSB1
$ echo -n -e "\x3" > /dev/ttyUSB1
$ echo -n -e "\x0" > /dev/ttyUSB1
```
