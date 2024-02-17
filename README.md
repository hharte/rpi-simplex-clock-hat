# RPi Simplex Clock Controller Hat


# Introduction

This project is a Raspberry Pi-compatible hat for controlling Simplex Synchronous clocks with a 24VAC motor and 24VAC correction solenoid.  This hardware can be used with the <code>[master-clock](https://github.com/hharte/master-clock.git)</code> software to implement a synchronous clock controller.

![alt_text](https://raw.githubusercontent.com/hharte/rpi-simplex-clock-hat/main/images/rpi-simplex-clock-hat.png "image_tooltip")


# Connectors


## Terminal block J1


<table>
  <tr>
   <td><strong><em>Pin</em></strong>
   </td>
   <td><strong><em>Name</em></strong>
   </td>
   <td><strong><em>Purpose</em></strong>
   </td>
  </tr>
  <tr>
   <td>1
   </td>
   <td>24VAC
   </td>
   <td>24VAC 60Hz Input
   </td>
  </tr>
  <tr>
   <td>2
   </td>
   <td>24VAC COM
   </td>
   <td>24VAC Common (Return)
   </td>
  </tr>
  <tr>
   <td>3
   </td>
   <td>SYNC
   </td>
   <td>24VAC Clock Sync Pulse Output
   </td>
  </tr>
  <tr>
   <td>4
   </td>
   <td>RUN
   </td>
   <td>24VAC Clock Motor control (optional)
   </td>
  </tr>
</table>



## Power Input Barrel Jacks J2, J4

Note: populate either J2 or J4.  J4 is intended to give the hat a smaller footprint when installed inside the clock housing.  J2 is intended for use with a full-size Raspberry Pi.


<table>
  <tr>
   <td><strong><em>Pin</em></strong>
   </td>
   <td><strong><em>Name</em></strong>
   </td>
   <td><strong><em>Purpose</em></strong>
   </td>
  </tr>
  <tr>
   <td>Tip
   </td>
   <td>24VAC
   </td>
   <td>24VAC 60Hz Input
   </td>
  </tr>
  <tr>
   <td>Ring
   </td>
   <td>24VAC COM
   </td>
   <td>24VAC Common (Return)
   </td>
  </tr>
</table>


Raspberry Pi J3 Signals


<table>
  <tr>
   <td><strong><em>Pin</em></strong>
   </td>
   <td><strong><em>Raspberry Pi Name</em></strong>
   </td>
   <td><strong><em>Purpose</em></strong>
   </td>
  </tr>
  <tr>
   <td>1
   </td>
   <td>3.3V DC Power
   </td>
   <td>3.3V DC Power
   </td>
  </tr>
  <tr>
   <td>2, 4
   </td>
   <td>5V DC Power
   </td>
   <td>Supplies 5VDC at 1.2A to backpower the RPi.
   </td>
  </tr>
  <tr>
   <td>11
   </td>
   <td>GPIO17
   </td>
   <td>SYNC
   </td>
  </tr>
  <tr>
   <td>13
   </td>
   <td>GPIO27
   </td>
   <td>RUN
   </td>
  </tr>
  <tr>
   <td>27
   </td>
   <td>ID_SDA
   </td>
   <td>ID EEPROM I2C Data
   </td>
  </tr>
  <tr>
   <td>28
   </td>
   <td>ID_SCL
   </td>
   <td>ID EEPROM I2C Clock
   </td>
  </tr>
</table>



# Power Supply

This board contains an AC to DC power supply to power the Raspberry Pi Zero 2 W from the same 24VAC that runs the clock motor.  If you do not wish to use this power supply to power the RPi, the following components may be omitted from your build:


<table>
  <tr>
   <td><strong><em>Reference Designator</em></strong>
   </td>
   <td><strong><em>Component</em></strong>
   </td>
  </tr>
  <tr>
   <td>D1
   </td>
   <td>Bridge rectifier
   </td>
  </tr>
  <tr>
   <td>C1, C2, C3, C4
   </td>
   <td>Capacitors
   </td>
  </tr>
  <tr>
   <td>PS1
   </td>
   <td>SPBW06-5 DC-DC Converter
   </td>
  </tr>
  <tr>
   <td>JP1
   </td>
   <td>Jumper
   </td>
  </tr>
</table>



# Jumper

When installed, JP1 provides 5VDC to the Raspberry Pi.  This hat can supply 5VDC at 1.2A, which is enough for a Raspberry Pi Zero 2W.


# Hat ID EEPROM

The board contains a footprint for a 24C256 or similar serial EEPROM.  This EEPROM connects to the Raspberry Pi’s i2c-0 port, and contains configuration data describing the hat.

The i2c-0 port needs to be enabled in `/boot/config.txt` by manually adding the line:


```
dtparam=i2c_vc=on
```


And uncommenting:


```
dtparam=i2c_arm=on
```


Reboot the Pi and make sure the EEPROM is detected at address 0x50:


```
$ i2cdetect -y 0
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:                         -- -- -- -- -- -- -- --
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
50: 50 -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
70: -- -- -- -- -- -- -- --
```


Use utilities from here: [https://github.com/raspberrypi/hats](https://github.com/raspberrypi/hats)


```
$ git clone https://github.com/raspberrypi/hats
$ cd hats/eepromutils/
$ make
$ ./eepmake rpi-simplex-clock-hat.txt rpi-simplex-clock-hat.eep
$ sudo ./eepflash.sh -w -f=./rpi-simplex-clock-hat.eep -t=24c64 -d=0
This will attempt to talk to an eeprom at i2c address 0x50. Make sure there is an eeprom at this address.
This script comes with ABSOLUTELY no warranty. Continue only if you know what you are doing.
Do you wish to continue? (yes/no): yes
Writing...
0+1 records in
0+1 records out
149 bytes copied, 0.660765 s, 0.2 kB/s
Closing EEPROM Device.
Done.
```


Read EEPROM to a file:


```
sudo ./eepflash.sh -r -t=24c64 -f=readout.eep
```


After programming the EEPROM and rebooting, the hat information should be available in `/proc/device-tree/hat/`


# Software Installation


## Raspberry Pi Zero 2 W

Install Raspberry Pi OS to MicroSD card using Raspberry Pi Imager.  Select Raspberry Pi OS (other) → Raspberry Pi OS (Legacy, 32-bit) Lite.

Change settings to enable WiFi with SSID/Password, set WiFi country, set locale, set username/password.

After booting the Raspberry Pi, ssh in and:


```
$ sudo apt-get install ntpdate
$ sudo ntpdate time.google.com
$ sudo apt-get update; sudo apt-get upgrade
$ sudo apt-get install git gpiod i2c-tools
$ git clone https://github.com/hharte/master-clock.git
```



### Configure Service

Edit `simplex_master_clock.service` and change the following line to the correct path for `simplex_master_clock.py`:

ExecStart=/usr/bin/python3 /home/pi/master-clock/simplex_master_clock.py


### Install Service


```
$ sudo cp simplex_master_clock.service /etc/systemd/system/
$ sudo systemctl enable simplex_master_clock.service
$ sudo systemctl start simplex_master_clock.service
$ sudo systemctl status simplex_master_clock.service
```



# Clock Connectors

The Simplex synchronous clocks typically have a 4-pin female Molex MLX connector.  These connectors, pins, and tools are available from Amazon:

Connectors: [https://www.amazon.com/dp/B07DKZQK5M](https://www.amazon.com/dp/B07DKZQK5M)

Crimper: [https://www.amazon.com/gp/product/B0C8MXNWDK](https://www.amazon.com/gp/product/B0C8MXNWDK)

Pin Extractor: DAP-T292328: [https://www.amazon.com/dp/B0C4NWRQD4](https://www.amazon.com/dp/B0C4NWRQD4)


# Clock Movement Bushings

The clock movements are mounted using pins with spring clips and rubber bushings.  These rubber bushings are get dried out and brittle with age.  If you would like to replace them, the ¼” grommets in this kit work well: [300 Pcs Rubber Grommets Kit](https://www.amazon.com/gp/product/B0BXKXMT55).


# Revision History

Rev 0 → Rev 1



1. Turn SSRs on with logic 1 to GPIO outputs.  This is needed so that the SSRs don't become enabled when the GPIO17 and GPIO27 are set to outputs during boot.  These GPIOs are pulled low on the RPi.
2. Add alternate footprint location (J4) for AC input barrel jack.
3. Add additional mounting holes for Raspberry Pi Zero / Zero 2 W.
4. Correct pinout for the bridge rectifier D1.
