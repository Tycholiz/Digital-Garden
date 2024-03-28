
This note is for CAN systems on modern vehicles.

## CAN (Controller Area Network)
The CAN Bus acts as a central networking system, allowing any ECU to communicate to the rest of the system.
- in the car, nodes (ie. electronic control units, or ECU) are connected via the CAN Bus.
  - these ECUs might be the Engine Control Unit, Airbags, Audio system etc
- anal: the CAN Bus is analogous to the nervous system in the human body. That is, it facilitates communicates between all parts

An OBD-II (on-board diagnostics) tool can be used to identify what is wrong with your car.
- the DTCs (diagnostic trouble codes) are retrieved by CAN loggers.
- OBD-II is also used for real-time vehicle telematics and post analysis
- OBD-II is vehicle make agnostic

### CAN Frame (ie. CAN Message)
The data in a CAN frame can be broken up into eight one-byte values, sixty-four one-bit values, one sixty-four bit value, or any combination of these

Includes
- CAN-ID, which includes message priority, as well as functional address (e.g. RMP, wheel speed etc.)
- RTR (Remote Transmission Request), which allows ECUs to request messages from other ECUs
- the data itself

Example frame:
- 0CF00400 FF FF FF 68 13 FF FF FF
  - if you have a DBC that contains decoding rules for the CAN ID (here, `0CF00400`), you can extract parameters (ie. signals) from the data bytes
- Decoded: 
```json
{
  "message": "EEC1",
  "signal": "EngineSpeed",
  "value": 621,
  "unit": "RPM"
}
```

## DBC (CAN Bus Databases)
the type of data that communicates over a CAN bus can be read and understood using DBC files.
- DBC files can help identify the data within the CAN frame by describing it.
- DBC files contain information for decoding raw CAN bus to physical values (ie. human-readable form). Thus, functioning as a signal library.

For a DBC file, the signal is not an electrical input or output, but it is a physical parameter, such as temperature, engine speed, voltages etc.

a DBC file helps in understanding what data is communicating through the CAN bus

The DBC file contains the following information.
- The CAN ID of the message in which this signal is present
- The position where the signal is present in the CAN message
- The byte order of the signal
- The conversion details of the signal
- Unit of the signal

ex. you could have a DBC file that contains decoding rules for Speed and EngineSpeed. Therefore, you could extract time-based Speed and EngineSpeed information from cars/trucks, tractors etc.

Using dedicated software, we can upload the data log files (ie. all of the CAN Frames, of which there are probably millions), along with the DBC file.
- the output might be as simple as a `.csv` of all the decoded CAN frames.

Proprietary DBC files are often used by OEM manufacturers for decoding their CAN data for blackbox logging.