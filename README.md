

# A 4 axis ESP32 CNC controller for daisy chained SPI stepper drivers.

![](https://github.com/bdring/4_Axis_SPI_CNC/blob/master/docs/images/20190731_165601.jpg)

### Trinamic Drivers

[Trinamic SPI stepper motor drivers](https://www.trinamic.com/products/integrated-circuits/) are great. They have dozens of features not found on traditional small stepper drivers. You can't control all the features via configuration pins, like the MS1, MS2 and MS3 traditionally used to select microstepping. They use SPI to setup and control the features. 

**Note:** Some of these drivers are sold in a pre-configured mode. They are often call "Stand Alone" versions of the drivers. This project is not compatible with those. It only works with SPI controlled drivers.

### About SPI Daisy Chaining

Typically each SPI device requires a SS (slave select) signal. With multiple drivers, this can cause problems with I/O pin count limited controllers, like the ESP32.

Most SPI devices can be used in a daisy chain mode (see image). As bits are received into a register via the MOSI pin,  bits are shifted out of the register of the MISO pin. Trinamic drivers use a a 40 bit wide register. To write to all three drivers shown below, you would send (3) sets of 40 bits. The first set of 40 bits would end up in the last driver. The SS line is pulled low once all drivers have their data. To write to only one driver, dummy data would be sent to the non targeted drivers. 

![](https://upload.wikimedia.org/wikipedia/commons/thumb/9/97/SPI_three_slaves_daisy_chained.svg/700px-SPI_three_slaves_daisy_chained.svg.png)

There is a very comprehensive and easy to use library, [TMCStepper](https://github.com/teemuatlut/TMCStepper), for SPI control, but it does not support daisy chaining. I created a fork that has a basic ability to write to any driver in an SPI daisy chain. It does not support reading data yet.

### Notes about the controller

- The SD card and drivers share the same SPI data lines. There is one SS for the SD and one SS for all of the drivers. 
- If you use less than (4) drivers, you need to jumper MOSI to MISO on the missing drivers to close the daisy chain loop.
- You can connect the DIAG1 pin of the drivers to the limit switches using jumpers JP1-JP4. Be sure to remove only one type of limit switch at a time.
- Source files
  - [Schematic (PDF)](https://github.com/bdring/4_Axis_SPI_CNC/blob/master/docs/1p3/SPI_4Axis_V1p3_Schm.pdf)
  - [Source Files (DipTrace)](https://github.com/bdring/4_Axis_SPI_CNC/tree/master/source/1p3/DipTrace)
  - [Gerbers](https://github.com/bdring/4_Axis_SPI_CNC/tree/master/source/1p3/Gerber)
  
### Plug In Modules

 - ESP32: The PCB uses ESP32 Dev Modules. It uses the 2 x 19 pin types with pin widths of either 0.9" or 1.0". 
 - Motor Drivers: It uses stepstick style stepper motor drivers with Trinamic SPI drivers. It has extra large capacitors.

### Firmware

- Use [Grbl_ESP32](https://github.com/bdring/Grbl_Esp32) with [spi_daisy_4axis.h](https://github.com/bdring/Grbl_Esp32/blob/master/Grbl_Esp32/Machines/spi_daisy_4axis.h) machine file


### I/O Expanders

Note: Obviously, external I/O expanders can be used increase I/O pin count. This project is focused on using only the I/O on the ESP32.

### Revision History

- **V1p2**
  - Added bypass jumper for 4th axis. Install jumper if 4th axis is not used. Normally the daisy chained SPI has to go through that axis.
  - Added 10uF optional capacitor to help with ESP32 modules that have trouble entering bootloader mode. This is not installed by default.
  - Added extra ground connection that TMC5160 drivers need. It is N/C on many other TMC drivers.

### Donation

This project represents a lot of work. Please consider a safe, secure and highly appreciated donation via the PayPal link below.

[![](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=TKNJ9Z775VXB2)
