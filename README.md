# Micro Python, the Internet of Things and the £3 device

Please find the slides for the above presentation, as well as some details on how to install Micro Python on a ESP8266 chip.

Slides Content &copy; Kirk Northrop 2016, except images which remain the copyright of their original owners.

# How to use Micro Python on an ESP8266 chip

Some of this may be complicated/not very clear/etc. If you are reading this and it doesn't make sense, please feel free to raise an issue or get in touch and I will try and write something clearer!

Firstly, you'll need an FTDI adapter. You want to wire it up as follows, and the FTDI must be 3V3 (3.3v DC).
Something like [this] (https://www.aliexpress.com/item/FT232RL-FT232-FTDI-USB-to-TTL-3-3V-5-5V-Serial-Adapter-Module-Download-Cable-for/32596090563.html?spm=2114.01010208.3.1.bjNvUW&ws_ab_test=searchweb201556_7,searchweb201602_1_10065_10068_112_10069_110_111_418_10017_109_108_10060_10061_10062_10057_10056_10055_10037_10054_301_10033_10059_10032_10058_10073_10071_10070_10052_10053_10050_10051,searchweb201603_4&btsid=f995096e-3113-4237-b95a-fa1f6889451b) is ideal.

Bear in mind that you may have to supply the 3V3 from elsewhere, as the ESP8266 can spike power quite a lot, and the USB port/FTDI's power rails may not be able to support this.

However, even if you supply the 3V3 from elsewhere, the FTDI adapter must still be 3.3v for signalling, or you will blow the ESP8266. (Hence you can in theory use an Arduino to do the flashing, but only if you are confident in splitting voltages)

ESP8266 | FTDI
--------|------
CH_PD | 3V3
VCC | 3V3
GND | GND
GPIO15 (if broken out) | GND
GPIO0 | GND for write, 3V3 for run
TX | RX
RX | TX

You probably want to use the awesome [esptool] (https://github.com/themadinventor/esptool)

You want to first off erase the flash. Bear in mind that `/dev/tty.usbserial-A50285BI` will vary depending on your system, the FTDI adapter you are using and so forth. On Windows then you're on your own I'm afraid.
`esptool.py --port /dev/tty.usbserial-A50285BI erase_flash`

Then you need to download the Micro Python firmware for the ESP8266 from (http://micropython.org/download/#esp8266)

Then you can flash 
`esptool.py --port /dev/tty.usbserial-A50285BI --baud 57600 write_flash --flash_size=8m -fm dio 0 esp8266-20160710-v1.8.2.bin`


coolterm   http://freeware.the-meiers.org/#CoolTerm


Currently… WebREPL not available. Need the kickstarter version for that. Check before PyCon…

http://forum.micropython.org/viewtopic.php?f=16&t=2140

import webrepl; webrepl.start()

Set a password - it will reboot. 

Then USB repl again and run that command again

Download boot.py
uncomment the webrepl lines
put it back again

Then webrepl will start on each boot.









Drivers https://hackaday.io/project/11660/logs/sort/oldest
