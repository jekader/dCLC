# dCLC - alternative firmware for 303WifiLC01 clock

This repo contains code by @maarten-pennings and @Utyff from this repo:

https://github.com/maarten-pennings/303WIFILC01

It is adapted to use PlatformIO for ease of building.
All kudos go to the original authors, patches welcome!

## Build

* Install Visual Studio Code and the PlatformIO plugin
* Clone the repo and open it using vscode
* Open the PlatformIO menu, select 303WIFILC01 target and click `Build`
* ensure no errors are reported and a SUCCESS status is shown

## Back up original firmware

It might be a good idea to back up the origina

## Flash firmware

WARNING: Some skills required. Read through the whole section before attempting.
The process may damage or brick the clock. Use at your own risk!

* Disconnect the clock from power
* Solder a UART adapter to the RX/TX/GND pads on the back of the clock
* Press and hold the SET button on the back of the clock
* Connect clock power via MicroUSB
* A red LED on the back should blink once - the device is now in BOOT mode ready for flashing
* Connect UART adaper to the PC running PlatformIO
* Optional: back up original firmware using esptool `esptool -p /dev/ttyUSB0 read_flash 0 0x100000 fw.bak`
* in the PlatformIO menu, select 303WIFILC01 target and click `Upload`
* Wait until the process reports SUCCESS - if it fails, check that the serial port is set up correctly and that there is nothing else using it
* The clock should now boot. If it does not - power cycle it

## Configure the clock

The clock can only be configured in a special mode.
* press the SET button immediately after plugging in power. This is tricky as pressing the button too soon will trigger BOOT mode so give it a few tries. The display should read `dCLC`
* Using a smartphone or other device, search for open WiFi networks and connect to `dCLC-NNNNNN`
* access IP 10.10.10.10 in a browser to get the configuration menu
* set up WiFi parameters and other settings as needed
* save the settings
* restart the device from the web page
* once a connection is established, time should be syncronised over NTP
* if something fails - repeat the steps
* time is persisted in the RTC so if the battery is present, time will be kept even when the clock is disconnected from power

## Troubleshooting

If the clock misbehaves, connect it to a computer using the UART adapter used for flashing and use a terminal emulator like `minicom` to review logs.
Normally, the firmware will print its configuration during startup and then show a timestamp every second of operation.
