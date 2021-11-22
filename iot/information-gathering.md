# Information gathering

- We have 7 attack surfaces

## Hardware

A smart device is embedded equipment that can be a hub (smart IoT gateway) for the IoT product, a smart sensor (e.g., light bulb, motion detector, doorbell) that collects data from the environment, or a controller (actuator) that performs an action in response to a user request (e.g., switching the light bulb, adding new devices to the smart home) or analyzes and displays the data. 

The visual observation involves identifying the available ports, slots, buttons, etc., whereas the physical examination also includes disassembly of the device.
IoT devices typically have some modules on printed circuit boards (PCBs) such as processors, RAM, ROM, peripherals, connectors, antennas, screens, and ribbon cables. 
The board can also expose debug ports (JTAG, SWD, etc.) and communication interfaces (UART, SPI, I2C, etc.) through particular pins (Tx, Rx, TDO, etc.).
The modules on the PCB vary in size, shape, and other aspects depending on the functionality of the device; this is known as packaging (DIL and SMD).
The packaging type of a module is essential because associated hardware adapters and other utilities are required to interact with them when conducting the pentest. 
Hardware testing commonly entails taking photographs of the components on the PCB.

The official technical specifications are carefully interpreted, including user and developer manuals.
The processor datasheet usually contains information about interfaces, I/O ports, interrupts, and more.
Moreover, documentation that is publicly available (i.e., on fccid.io) is explored to reveal incomplete information (e.g., chip pin-outs).
An additional search can be executed on repositories of available datasheets (www.alldatasheet.com),(www.datasheets360.com) for specifications that are missing from the manufacturer's website.
Moreover, the logo can be searched online for modules that do not contain a model number (www.westfloridacomponents.com/manufacturer-logos.html).

Finally, to derive use case diagrams, the relevant information is summarized, including what each device does and the way in which it operates, how devices communicate with each other, how the input and output of the device work, and the functionality of the buttons and external interfaces.

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

A device consists of not only hardware but also firmware, which can be proprietary or open-source software.
To perform firmware analysis, it is necessary to first obtain the firmware. 
The simplest method is to download it from the manufacturer's page. 
The second method involves taking an image of the removable SD card on the device. 
Unfortunately, contrary to popular belief, these two methods are not very valid today. 
As a third option, we can trigger the update process manually via the control software (e.g., mobile app) and capture the update URL or network traffic.
When the traffic is encrypted, we utilize MitM. 
However, a common problem for researchers is that manufacturers today often send update packets instead of full firmware. 
In any case, this is still valuable as it may contain sensitive information (e.g., credentials). 
The fourth option is to obtain the operating system command line of the device by physically connecting to the hardware debug ports. 
If these ports are authorized or if they are inaccessible components, the integrated storage on the device is removed and soldered to an external reader, which enables a disk image to be obtained. 
However, we propose this method as the last choice, as there may be situations where it can damage the board or cause failure when soldering it onto the board again. 
Finally, as mentioned later, as a result of the discovery of remote command execution vulnerabilities for network services, the command line of the device can be accessed such that a firmware copy can be taken or a live analysis can be performed.
If we obtain command-line access, we can also examine the config files and reverse engineer the libraries that are part of the update process to identify the update URLs.

| ID   | Component | Version |
| ---- | ----------- | ------- |
| A2   | Firmware/OS: Busybox	 | 1.33.3  |


## Radio

IoT devices communicate with each other and share and exchange data via radio signals, such as Bluetooth Low Energy (BLE), LoRaWAN, and ZigBee. 
The radio chipsets on the PCB provide information about the radio protocols supported by the device.
Manufacturer documentation and public resources help reveal useful information, such as the operating frequency of the device.
It is also valuable to investigate whether similar devices operate in the same frequency range as these devices.
The final goal is to capture a sample of radio traffic for a preliminary analysis. 
It is vital to capture radio traffic while devices have just started sending and receiving their first bits; otherwise, the collected data might not be accurate.
This is because devices are known to exchange certain types of data only when they are first introduced into the network.
After capturing the raw signals, we usually need to convert them into network packets via transformers (e.g., GNU companion).
In this way, packet analysis tools (e.g., Wireshark) can be used to examine the pcap files.
Eventually, the following findings are obtained: the component that initiates the authentication and pairing mechanism, the internals of the pairing operation, the number of devices each component can handle simultaneously, the nature of the data transmitted through protocols, the operating frequencies, the commands and data, and the default encryption keys (e.g., trust center link key and network (transport) encryption key).

| ID   | Component | Functionality                                                |
| ---- | --------- | ------------------------------------------------------------ |
| A3   | BLE       | Initiates authentication \& paring                           |
| A4   | SDR       | Frequency: <> Commands: <> Data: <>                          |
| A5   | ZigBee    | Trust center link key: <> Network (transport) encryption key: <> |


## Network

Today, firmware has evolved into an operating system (e.g., Ubuntu core, RIOT) that provides network services (e.g., SNMP, FTP, HTTP), especially for remote access or utilities (e.g., BusyBox). 
Traditional port scanning techniques (e.g., nmap) are used to identify open ports along with running applications with versions.
In addition, services that require authentication are identified, and samples of network traffic are captured for the initial analysis.

| ID 	| Component | Functionality	|
| ----- | --------- | ------------- |
| A6 | TCP/9090 - Open | Management page	|


## Cloud

IoT devices usually send the collected data to the cloud component, which can be accessed via APIs. 
This allows the user to manage the permissions of who can control the devices, monitor the device, view data analytics, peruse usage information, etc. 
The ability to locate Cloud API documentation is valuable because it contains sample requests and response pairs.
It is important to note that such documentation may not contain all APIs, especially backdoor APIs that are usually sensitive and missing.

| ID 	| Component | Functionality	|
| ----- | --------- | ------------- |
| A7 	| TCP/443 - Open	| Developer page: JSON data formats of the API	|


## Web

Firmware commonly provides a website to host an HTTP service designed for management, configuration, and status checking purposes.
Traditional webpage crawling and enumeration techniques (e.g., dirb, dirbuseter, wfuzz, gobuster, nikto) are used to discover all web pages and development technologies with their versions.
Web pages and input fields that are accessible without authentication are identified.
The user roles and user credentials exposed to documentation are also noted.

| ID 	| Component | Functionality	|
| ----- | --------- | ------------- |
| A8 	| TCP/80 - Open	| Client page: Monitor devices, view analytic	|
| A9 	| TCP/8080 - Open | Management  page: Add devices, manage permissions	|


## Mobile

IoT devices are usually controlled using mobile apps that expose the functionality of the IoT device and operational details of the machine, as well as revealing sensitive data. 
Information gathering covers the answers to the following questions: Which devices are controlled by the mobile app? Which control command is sent to the device over which protocol (BLE, WiFi)? What are the main functionalities of the mobile app (add/remove device/user, switch on/off)? What are the other valuable features of the mobile app (activity log, update firmware)? How do certain actions work (action X works with a one-time-key/PIN code)? Summarize what functionality provides what? What other activities are triggered automatically immediately after an action is started (log generated after switching on/off)? What are the types of users with access rights?

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
