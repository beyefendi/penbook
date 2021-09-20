# Threat modeling template

## Web application/GUI attack surface 

### Information gathering

The control server is designed for the medical team (nurses) to operate the infusion pump remotely.

The kiosk app contains a graphical user interface (GUI) that is the front-end of the control server service.

**Interaction:** The service sends commands to the infusion pumps that are transmitted through an encrypted protocol.

**Capabilities:**

- The kiosk is not designed to use the device’s OS as a standard OS user but only the GUI app from the touch screen.

- Vendor techicians' can access the wole device remotly for maintanence purposes.

- GUI app has very limited interactions with the underlying OS, and the input capability is fairly restricted. 

- Patients are not supposed to be a user of the infusion pump due to safety reasons. 

  ![](images/threat-model-1.png)

### Threats

#### 1. Authentication - Weak credentials

- The GUI authenticates users with four-digit PINs that can be predicted. 
- **Impact:** If attackers bypass authentication, they can send commands to the infusion pump on behalf of the accounts’ owners.
- **Category:** Spoofing

#### 2. Authentication - Improper account lockout

- The GUI locks a user out after five consecutive failed login attempts.
- Although it is supposed to be a brute-force protection mechanism, it can cause not to log into the system for a specified period. 
- Since the system works with a single user account, it blocks all users. 
- **Impact:** It might block access to the system and violate the patient safety requirement. Need to find the balance between security, safety, and usability.
- **Category:** Denial of service

#### 3. Authentication - Weak mechanism

- The GUI logs user actions; however, it supports only a single user account for the nurses. 
- **Impact:** It is impossible to distinguish users between them and identify actual users of a specific operation.
- **Category:** Repudiation

#### 4. Supply chain attack

- Critical medical systems frequently have remote support solutions that allow the vendor’s technicians to access the software instantly. 
- Even if remote connection also requires authentication, the credentials might be publicly available or be the default.
- **Impact:** As technician accounts are more privileged on the device, it can be misused to escalate privileges.
- **Category:** Privilege escalation

#### 5. Error messages

- When presented to the user, certain debugging messages or errors might reveal important information about the patients or system internals. 
- Commonly, such devices usually run outdated utilities, so such services are prone to vulnerabilities. 
- **Impact:** Adversaries might be able to decode these messages, identify the underlying technologies, and figure out ways to exploit them.
- **Category:** Information disclosure

#### 6. Physical tampering

- Although the GUI is designed to allow limited inputs, it could get input through an external keyboard.
- Even if most of the keyboard keys have been disabled, the system might still allow key combinations, such as shortcuts, hotkeys, or even accessibility features configured by the OS (closing a window by pressing ALT-F4 on Windows or CTRL-TAB for switching apps). 
- **Impact:** These could allow attackers to bypass the GUI and exit the kiosk app that exposes another attack surface firmware.
- **Category:** Tampering