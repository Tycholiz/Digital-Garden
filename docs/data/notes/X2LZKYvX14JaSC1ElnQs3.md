
Serial Port is a Node library that allows us to connect and communicate with an external device via the device's serial ports

![Serial Port](/assets/images/2021-03-20-19-47-20.png)

### Raspberry Pi + Roomba/Arduino
Roomba has a serial port, meaning we can connect it with a Raspberry Pi via the Pi's GPIO pins

On the Pi, we would install Node SerialPort, and run `npx @serialport/list` to scan the devices that are connected via the GPIO pins.
- From there, we open a serial port, we open a REPL, and we define a new port, which uses one of the connected devices and specifies some options
```
$ npx @serialport/terminal -p /dev/tty.usbmodem14301                                                                    
Opening serial port: /dev/tty.usbmodem14301 echo: true
```
```
$ npx @serialport/repl                                                                                                  
port = SerialPort("/dev/tty.usbmodem14301", { autoOpen: false })
globals { SerialPort, portName, port }
```
