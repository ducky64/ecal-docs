# E-ink calendar display

Fully open-source:
- Hardware design
  - Circuit (as HDL): https://github.com/BerkeleyHCI/PolymorphicBlocks/blob/master/examples/test_iot_display.py
    - A ESP32-S3-based board using a low-quiescent TSP54202 buck converter with a 24-pin EInk display.
    - Some fixes needed to reduce quiescent current draw to ~100uA: remove the protection 3.3v Zener diode (draws excessive current at operating), increase impedance of buck converter feedback resistors
    - Future versions might minimize quiescent current draw with an external RTC that gates the buck converter, or even gates a LDO since so little time is spent active
  - Layout: https://github.com/BerkeleyHCI/PolymorphicBlocks/tree/master/examples/IotDisplay
- Software
  - Server (Python + Flask + SVG template): https://github.com/ducky64/eink-svg-server
    - Offloads calendar rendering to a server which can run a proper iCal decoder
    - Returns a PNG image and time-to-next-update
  - Firmware (Arduino + PlatformIO build): https://github.com/ducky64/edg-pcbs/tree/main/IoTDisplay
    - Fetches a PNG from an external server (as base64 as part of a JSON structure) and draws it to the screen
    - Deep sleep to reduce power draw
