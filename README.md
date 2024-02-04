# RPi Simplex Clock Controller Hat


# Introduction

This project is a Simplex Synchronous clock controller hat for controlling clocks with a 24VAC motor and correction solenoid.

![alt_text](https://raw.githubusercontent.com/hharte/rpi-simplex-clock-hat/main/images/rpi-simplex-clock-hat.png "image_tooltip")

# Connectors

Terminal block J1


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


Power Input Barrel Jack J2


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
   <td>[GPS] 5V DC Power used for super caps
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



# Jumper

When installed, JP1 provides 5VDC to the Raspberry Pi.  This hat can supply 5VDC at 1200mA, which is enough for a Raspberry Pi Zero 2W.
