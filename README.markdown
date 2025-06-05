# NodeMCU Evil Twin WiFi Captive Portal

## Overview

This project implements an "Evil Twin" WiFi access point using a NodeMCU (ESP8266) to create a rogue WiFi network that mimics a legitimate one. It sets up a captive portal to capture user credentials by presenting a fake firmware update page, tricking users into entering their WiFi passwords. The captured passwords are stored in the ESP8266's EEPROM and can be viewed or cleared via a web interface. Additionally, the SSID of the rogue access point can be changed dynamically.

**Note**: This code is provided for educational purposes only. Unauthorized use of this code to harm networks or steal credentials is illegal and unethical. Use responsibly and only in controlled environments with explicit permission.
![](https://galaxy.ai/_next/image?url=https%3A%2F%2Fimg.youtube.com%2Fvi%2FVeMaF27B0nA%2Fmaxresdefault.jpg&w=1920&q=75)
## Features

- Creates a WiFi access point with a customizable SSID (default: "Free WiFi").
- Hosts a captive portal with a fake firmware update page to capture WiFi passwords.
- Stores captured passwords in EEPROM for persistent storage.
- Provides a web interface to:
  - View captured passwords.
  - Clear stored passwords.
  - Change the SSID of the access point.
- Blinks the built-in LED when a password is submitted.
- DNS spoofing to redirect all DNS requests to the captive portal.
- Persistent SSID storage in EEPROM, with a fallback to the default SSID.

## Requirements

- **Hardware**:
  - NodeMCU or any ESP8266-based board.
- **Software**:
  - Arduino IDE with ESP8266 board support.
  - Libraries:
    - `ESP8266WiFi`
    - `DNSServer`
    - `ESP8266WebServer`
    - `EEPROM`

## Installation

1. **Set up Arduino IDE**:

   - Install the Arduino IDE.
   - Add ESP8266 board support by following the instructions here.
   - Install the required libraries via the Arduino Library Manager.

2. **Upload the Code**:

   - Copy the provided code into the Arduino IDE.
   - Connect your NodeMCU to your computer via USB.
   - Select the appropriate board (e.g., `NodeMCU 1.0 (ESP-12E Module)`) and port in the Arduino IDE.
   - Upload the code to the NodeMCU.

3. **Configuration**:

   - The default SSID is `"Free WiFi"`. You can change it via the web interface at `/ssid` or modify the `SSID_NAME` constant in the code.
   - The access point uses the IP address `172.0.0.1` as the gateway.

## Usage

1. **Power on the NodeMCU**:

   - Once powered, the NodeMCU creates a WiFi access point with the configured SSID (default: "Free WiFi").
   - The built-in LED will remain on until a password is submitted.

2. **Connect to the Access Point**:

   - On a device, connect to the WiFi network created by the NodeMCU.
   - Most devices will automatically open a captive portal. If not, navigate to `http://172.0.0.1` in a browser.

3. **Interact with the Captive Portal**:

   - The main page (`/`) displays a fake firmware update prompt asking for a WiFi password.
   - Enter a password and submit it. The password is stored in EEPROM, and the built-in LED blinks 5 times.
   - Navigate to `/pass` to view all captured passwords.
   - Navigate to `/ssid` to change the SSID of the access point (reconnect to the new SSID after submission).
   - Navigate to `/clear` to reset the stored passwords.

4. **Serial Monitor**:

   - Open the Serial Monitor in the Arduino IDE (baud rate: 115200) to view the current SSID and other debug information.

## File Structure

- **Main Sketch**: Contains the core logic for the WiFi access point, captive portal, and EEPROM handling.
- **Web Interface**:
  - `/`: Main page with the fake firmware update form.
  - `/post`: Handles password submissions.
  - `/pass`: Displays captured passwords.
  - `/ssid`: Form to change the SSID.
  - `/postSSID`: Handles SSID changes.
  - `/clear`: Clears stored passwords.

## Technical Details

- **EEPROM Usage**:
  - Stores the SSID starting at address `0`.
  - Stores passwords starting at address `30`.
  - Uses address `20` to check if the ESP is running for the first time.
- **DNS Spoofing**:
  - Redirects all DNS requests to the gateway IP (`172.0.0.1`), ensuring the captive portal is displayed for any website access attempt.
- **Web Server**:
  - Runs on port `80` using the `ESP8266WebServer` library.
  - Serves HTML pages with a simple CSS-styled interface.
- **LED Feedback**:
  - The built-in LED blinks 5 times (on/off) when a password is submitted.

## Security and Ethical Considerations

- This code is designed to demonstrate vulnerabilities in public WiFi networks and should only be used for educational purposes or in environments where you have explicit permission to test.
- Deploying this code in public or unauthorized networks is illegal and can result in severe consequences.
- Always obtain consent from network owners and users before performing security testing.

## Troubleshooting

- **Cannot connect to the access point**:
  - Ensure the NodeMCU is powered and the code is uploaded correctly.
  - Verify the SSID in the Serial Monitor and connect to it.
- **Captive portal not appearing**:
  - Manually navigate to `http://172.0.0.1` in your browser.
  - Ensure DNS spoofing is working by checking if all website requests redirect to the portal.
- **Passwords not saving**:
  - Check the Serial Monitor for EEPROM-related errors.
  - Ensure the EEPROM is initialized with `EEPROM.begin(512)`.

## License

© All rights reserved by Ishinder Singh
