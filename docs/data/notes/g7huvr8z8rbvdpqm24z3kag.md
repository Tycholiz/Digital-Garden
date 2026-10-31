
A driver is software that is run by the [[kernel|os.kernel]] that gives it the ability to control a particular piece of hardware (e.g. keyboard, audio interface)

A driver communicates with a device via a [[computer bus|hardware.bus]]
1. When a calling program invokes a routine in the driver, the driver issues commands to the device (drives it). 
2. Once the device sends data back to the driver, the driver may invoke routines in the original calling program.

Drivers are hardware dependent and [[operating-system|os]] specific
