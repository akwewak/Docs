---
description: Implemented by status & control module
---

# Status messages

Used for custom telemetry messages generated by the DroneBridge modules or plugins.

**Difference to communication messages/protocol:**

* Higher update rates \(several times/second\)
* No guaranteed delivery/No response
* Shorter

## Received by status module on ground station via long range link \(embedded in raw protocol\)

### UAV-RC status update message

 Sent by control module on AirPi ~5Hz

| Description | Values | Details |
| :---: | :---: | :---: |
| RSSI RC on UAV | signed little-endian \(?\) | 1 byte |
| received RC packets/s | unsigned little-endian | 1 byte |
| AirPi CPU usage \[%\] | unsigned little-endian | 1 byte |
| AirPi CPU temp \[°C\] | unsigned little-endian | 1 byte |
| AirPi low voltage | unsigned little-endian | 1 byte |
| unused | unsigned little-endian | 1 byte |

AirPi low voltage = 1 if low.

## Messages sent by status module to a ground control station \(app\)

### DroneBridge system-status message

This frame is generated and sent by the status module as a UDP packet to port 1608. It is 20 bytes long and has the following structure:



| Description | Values | Details |
| :---: | :---: | :---: |
| Identifier | $D | 2 bytes |
| Message ID | 1 | 1 byte |
| Mode | UTF-8 | 1 byte |
| Received RC packets/s | unsigned | 1 byte |
| RSSI drone \(RC\) | signed | 1 byte |
| RSSI ground station \(video\) | signed | 1 byte |
| Damaged blocks \(video\) | unsigned little-endian | 4 bytes |
| Lost packets \(video\) | unsigned little-endian | 4 bytes |
| Video bit rate Kbit/s | unsigned little-endian | 4 bytes |
| Voltage status | bit 0: GNDPi;  bit 1: AirPi | 1 byte |

Voltage status: 

* bit 0: if set GNDPi is on low voltage
* bit 1: if set AirPi is on low voltage

### DroneBridge RC-status message

Gets the RC-Channel values from the control module \(ground station\). The values are in between 1000 and 2000. These are the values right before they get transformed to DroneBridge RC protocol so calibration issues can be detected!

| Description | Values | Details |
| :---: | :---: | :---: |
| identifier | $D | 2 bytes |
| message id | 2 | 1 byte |
| ch1 | unsigned little-endian | 2 bytes |
| ch2 | unsigned little-endian | 2 bytes |
| ch3 | unsigned little-endian | 2 bytes |
| ch4 | unsigned little-endian | 2 bytes |
| ch5 | unsigned little-endian | 2 bytes |
| ch6 | unsigned little-endian | 2 bytes |
| ch7 | unsigned little-endian | 2 bytes |
| ch8 | unsigned little endian | 2 bytes |
| ch9 | unsigned little endian | 2 bytes |
| ch10 | unsigned little endian | 2 bytes |
| ch11 | unsigned little endian | 2 bytes |
| ch12 | unsigned little endian | 2 bytes |
| ch13 | unsigned little endian | 2 bytes |
| ch14 | unsigned little endian | 2 bytes |

## Messages received the by status module coming from a ground control station \(app\)

### DroneBridge RC-overwrite message

Allows to overwrite the channel values sent by DroneBridge RC via the control module. Can be used by head trackers etc.. Not intended to be used for regular control of the UAV. Please modify the implementation of the control module for that purpose.

| Description | Values | Details |
| :---: | :---: | :---: |
| identifier | $D | 2 bytes |
| message id | 3 | 1 byte |
| ch1 | unsigned little-endian | 2 bytes |
| ch2 | unsigned little-endian | 2 bytes |
| ch3 | unsigned little-endian | 2 bytes |
| ch4 | unsigned little-endian | 2 bytes |
| ch5 | unsigned little-endian | 2 bytes |
| ch6 | unsigned little-endian | 2 bytes |
| ch7 | unsigned little-endian | 2 bytes |
| ch8 | unsigned little endian | 2 bytes |
| ch9 | unsigned little endian | 2 bytes |
| ch10 | unsigned little endian | 2 bytes |
| ch11 | unsigned little endian | 2 bytes |
| ch12 | unsigned little endian | 2 bytes |
| ch13 | unsigned little endian | 2 bytes |
| ch14 | unsigned little endian | 2 bytes |

Channels set to zero will be ignored. Overwrite is only valid for 100ms. To overwrite channels for a longer period of time the update frequency of the RC-overwrite message must be &gt;10Hz. Values must be between 1000-2000.

