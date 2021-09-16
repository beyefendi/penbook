# Information gathering

- We have 7 attack surfaces

## Hardware

Component | Information
---|---
Button				| Power, reset, volume, control, etc. 
External interfaces		| Ethernet, SD Card slot, USB Type-c, docking connector etc. 
Display type			| LCD
Power and voltage		| 3.3 V
Certifications			| CA
FCC ID labels			|<>
Similar devices			| May be re-branded version of a known device
Modules/Peripherals		| Camera, battery, headphone, led-light
Connectors			 | GPS
Antenna				| GPS, GSM, Wi-Fi
Chipset				| Model
Processor			| Type, model no
RAM					| Model no, capacity
ROM					| Model no, capacity
Storage				| Model no, capacity
Comm. interfaces		| UART and JTAG pins
Packaging type			| DIL TO-220


## Firmware

| ID   | Component | Version |
| ---- | ----------- | ------- |
| A2   | Firmware/OS: Busybox	 | 1.33.3  |


## Radio

| ID   | Component | Functionality                                                |
| ---- | --------- | ------------------------------------------------------------ |
| A3   | BLE       | Initiates authentication \& paring                           |
| A4   | SDR       | Frequency: <> Commands: <> Data: <>                          |
| A5   | ZigBee    | Trust center link key: <> Network (transport) encryption key: <> |


## Network

| ID 	| Component | Functionality	|
| ----- | --------- | ------------- |
| A6 | TCP/9090 - Open | Management page	|


## Cloud

| ID 	| Component | Functionality	|
| ----- | --------- | ------------- |
| A7 	| TCP/443 - Open	| Developer page: JSON data formats of the API	|


## Web

| ID 	| Component | Functionality	|
| ----- | --------- | ------------- |
| A8 	| TCP/80 - Open	| Client page: Monitor devices, view analytic	|
| A9 	| TCP/8080 - Open | Management  page: Add devices, manage permissions	|


## Mobile

| ID  | Component | Functionality |
| --- | --------- | ------------- |
| A10 | Android app	| Native app, add, remove, and control device, JSON API, TLS	|
| A11 | iOS app		| Add, remove, and control device, SOAP API, TLS |


## ID Assets Functionality

### Hub

- Has an Infrared Signal receiver, camera, bumper, charging sensors and various tools for cleaning. 
- The robot will automatically return to the charging station once cleaning is done or battery is running low.
- Also includes a power switch, a voice prompt and a pause button.
	
### Mobile application

- Available for Android and iOS
- Auto cleaning mode
- Taking photo
- monitor 
- audio recording
- Share on social media 
- give motion detection warnings
- instruct a firmware upgrade and a factory reset.
