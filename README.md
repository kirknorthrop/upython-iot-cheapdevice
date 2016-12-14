# Micro Python, the Internet of Things and the £3 device

Please find the slides for the above talks, as well as some details on how to install Micro Python on a ESP8266 chip.

I originally gave this talk at PyCon UK 2016. If you'd like to see me giving this talk, please see here: (https://www.youtube.com/watch?v=3ood9xagDCk)

I gave a revised version, focussing more on the ESP8266 chip at Oxford Python Meetup in December 2016.

Both sets of slides are available above.

Slides Content &copy; Kirk Northrop 2016, except images which remain the copyright of their original owners.

## How to use Micro Python on an ESP8266 chip

Some of this may be complicated/not very clear/etc. If you are reading this and it doesn't make sense, please feel free to raise an issue or get in touch and I will try and write something clearer!

Firstly, you'll need an FTDI adapter. You want to wire it up as follows, and the FTDI must be 3V3 (3.3v DC).
Something like [this] (https://www.aliexpress.com/item/FT232RL-FT232-FTDI-USB-to-TTL-3-3V-5-5V-Serial-Adapter-Module-Download-Cable-for/32596090563.html?spm=2114.01010208.3.1.bjNvUW&ws_ab_test=searchweb201556_7,searchweb201602_1_10065_10068_112_10069_110_111_418_10017_109_108_10060_10061_10062_10057_10056_10055_10037_10054_301_10033_10059_10032_10058_10073_10071_10070_10052_10053_10050_10051,searchweb201603_4&btsid=f995096e-3113-4237-b95a-fa1f6889451b) is ideal.

Bear in mind that you may have to supply the 3V3 from elsewhere, as the ESP8266 can spike power quite a lot, and the USB port/FTDI's power rails may not (read: won't) be able to support this.

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

![ESP8266 Pinout](http://playground.boxtec.ch/lib/exe/fetch.php/wireless/esp8266-pinout_etch_copper_top.png)

You probably want to use the awesome [esptool] (https://github.com/themadinventor/esptool)

You want to first off erase the flash. Bear in mind that `/dev/tty.usbserial-A50285BI` will vary depending on your system, the FTDI adapter you are using and so forth. On Windows then you're on your own I'm afraid.
`esptool.py --port /dev/tty.usbserial-A50285BI erase_flash`

Then you need to download the Micro Python firmware for the ESP8266 from (http://micropython.org/download/#esp8266)

Then you can flash the firmware - obviously change the port and the filename...
`esptool.py --port /dev/tty.usbserial-A50285BI --baud 57600 write_flash --flash_size=8m -fm dio 0 esp8266-20160710-v1.8.2.bin`

You'll then want to connect to the ESP8266 via the USB (after remembering to change GPIO0 to 3V3 and cycling the power of the chip).

There are two methods I will describe for this, the horrible old one and the less horrible newer one.

### Horrible old one

[Coolterm](http://freeware.the-meiers.org/#CoolTerm) is good for connecting to a USB Serial device.

Currently due to the fact that the ESP8266 Micro Python came from a Kickstarter, WebREPL is not available by default. Therefore we need to do the following.

First, connect via USB/Serial and type the following
`import webrepl; webrepl.start()`

Set a password - and the ESP8266 will reboot.

Then connect via USB/Serial again and run that command again.

You then need to connect using WebREPL, which can be found here: (http://micropython.org/webrepl/)

Then you can:
- download boot.py
- uncomment the WebREPL lines
- put it back again

Then WebREPL will start on each boot.

You can then use my [IoTWifi project](https://github.com/kirknorthrop/IoTWiFi).

### Less horrible newer one

Download a lovely ESP8266 fork of the micro:bit mu editor from here: (https://github.com/eduvik/mu/tree/feature/multi-board)

Press either files or REPL, and you will have access to those things as required

If you really want to use WebREPL, you can download boot.py, uncomment the WebREPL lines and save it back to the device.

## Useful Links

[Micro Python docs](https://docs.micropython.org/en/latest/esp8266/esp8266/tutorial/intro.html)

[ESP8266 Binary downloads](https://micropython.org/download#esp8266)

[ESP8266 fork of Mu Editor](https://github.com/eduvik/mu/tree/feature/multi-board)

[ESP8266 Pinout](http://playground.boxtec.ch/lib/exe/fetch.php/wireless/esp8266-pinout_etch_copper_top.png)

[ESP8266 Micro Python drivers for various things](https://hackaday.io/project/11660-various-micropython-libraries-and-drivers)

[ESP8266 SSD1306 OLED driver](https://github.com/micropython/micropython/blob/master/drivers/display/ssd1306.py)

[My IoT WiFi helper](https://github.com/kirknorthrop/IoTWiFi)
