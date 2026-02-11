# esp32-mmwave
This is a how to on getting the esp+mmwave package set up in home assistant


Things you'll need:

- esp+mmwave package
- usbc cable capable of data transfer
- device capable of flashing esp (homeassistant box or machine capable of serial over usb)
- wall charger or other means of providing power to the esp. I personally use https://a.co/d/0dFnKW11 due to it being low-profile and supporting multiple ports, but you can use almost anything as long as it outputs 5v1a.
- usbc cable for final placement if using a different one than what was used for flashing. I like these because they're low-profile https://a.co/d/096fsXBY but they only come in black.

Pre-Requisites for flashing:

1. Install the ESPHome addon in home assistant via https://esphome.io/guides/getting_started_hassio/
2. Once installed, go to the ESPHome Builder (homeassistant > addons > ESPHome Builder > Open web UI)
3. Click Secrets up above and add your iot network credentials in the following format:  
   wifi_ssid: "networkname"  
   wifi_password: "password"
4. Add an encryption key to secrets for the ESPs too if you don't need different encryption keys for each ESP  
   enc_key: "<44-Character-String>"
5. Add an OTA password for secure OTA updates  
   ota_pass: "<20-character-string>"  
7. Click Save

Flashing: 

1. Click "+ New Device" > Continue > Empty Configuration > and name your device. I usually name mine in the following scheme: mmwave-kitchen to keep them easy to find.
2. Once named, you'll see a new device with your chosen name pop up in the builder screen. Click Edit and paste the following config. Make sure to modify it as there are static IP addresses.
   https://github.com/glocklol/esp32-mmwave/blob/main/mmwave-sample.yaml
3. Click Save.
4. Click the 3 dots on your new device and select Install > Manual Download. Wait for the firmware to build. Once complete, select Factory Format and allow the download.
5. Pop a new tab and head over to https://web.esphome.io in Chrome (it's one of the few browsers that is actually supported for this part)
6. Attach the antenna to the board here, you'll feel it click on. [Antenna Placement](https://github.com/glocklol/esp32-mmwave/blob/6240b2dde48fa7c476f8c093a2e1ead94fc692f7/IMG_2688.jpeg)
7. Stage the ESP for flashing by holding the B (Boot) button on the ESP (if usbc port is facing up and away from you, it's on the top right of the board, it's miniscule. Photo here: [Boot Button](https://github.com/glocklol/esp32-mmwave/blob/db1fb2cc9c98057f367d7c1c87d231a01751715b/IMG_2687.jpeg))
   Hold this button as you plug the ESP into your machine - this will put it in bootloader mode.
9. Go through any system prompts asking if you'd like to allow the device to connect
10. Click "Connect" in the ESP Home Web window.
11. Select the device - usually USB JTAG/ Serial something or other and click Connect
12. Click Install - NOT PREPARE FOR FIRST USE - and load that file we downloaded from ESPHome Builder in homeassistant, click install. Let it do its thing and when it's complete, unplug and re-plug the ESP to power (hitting the reset button will just put it back into bootloader mode. Power must be cut.
13. Try pinging the IP you set for it and that's it.

Getting it into home assistant:

The new esp should automatically show in the found devices in the home assistant integrations section. Click Add, and it may ask you for the encryption key we set up in secrets earlier. 

Now you can go over to Integrations > ESPHome to fiddle with your new device.

Radar Instructions:

The mmwave radar in this package will likely need a little tuning to get presence detection just right. Under the ESP's device page you'll find sliders for "Gate" sensitivities g0-g8 each of which has a Still and a Movement threshold you can set.  The "Engineering Mode" switch will enable live output of what each distance "Gate" detects. Likewise, you can set max still and max moving distance the radar will report back to HA here as well as radar timeout. Keep this above a second or so so the radar can "settle" after movement disappears. 
