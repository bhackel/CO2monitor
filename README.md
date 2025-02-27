## The Idea

This is the first project where I took the time to think through an ideal version of a CO2 monitor, then I broke it down into small pieces and will be attempting to execute on it

Full End-To-End project
 - IOS App that can sync data from the device
 - Device that monitors CO2 levels, can be attached to person

Software requirements:
- IOS App
	- Want to build in swift
	- Needs to connect to the device over bluetooth, then read some stored data, then clear the stored data on the device, and store it in  app
	- Display a graph that shows CO2 exposure over the day
	- Export button to download CSV with "time: ppm" values
- Microcontroller code
	- Possibly translate it into Rust
	- data should be collected every 30? seconds
	- Poll the Senseair using raw Serial comms
	- First develop on arduino 33 iot
	- Then create PCB design with the components
	- This must happen before 3D print can be done

Hardware requirements:
- Option 1: Wrist
	- 3D print a decently big rectangle that attaches to the wrist using Velcro
	- Power using a wire to a battery bank (need 5V), or integrate battery somehow
	- This seems like the ultimate option, copy a Fitbit in design. highest difficulty
- Option 2: Belt hook
	- 3D print a box that clips onto the belt
	- Power using a wire to a battery bank that is in pocket
	- This is where I should start

Procedure
- We already have a working CO2 monitor
- #### ARDUINO
- Add Bluetooth functionality so that I can connect to it with my laptop and read the current CO2 value
- Add Bluetooth functionality so that I can connect to it, read up to the past 100 values, then these values are cleared from a storage in memory
- #### APP
- ~~Create a swift Hello World app and send it to my phone~~ Done 12/31/23
- Add Bluetooth functionality to the swift app so that it can connect to the Arduino and read the current CO2 value to the screen
- Have it continually read the CO2 value and put it on the screen
- Have it read the past 100 values
- Create a storage for storing CO2 values over time
- Create a graph that shows these values
- Create a table that shows these values
- Create a button that allows to share a CSV with these values
- #### PCB
- Design a PCB that integrates all the components nicely
- Make sure to figure out how to get that microcontroller into there
- Somehow get the power connected with a connector
- I am unfamiliar in this part
- #### 3D Printing
- Create a box that encases the PCB and has poles or something to mount it on
- Don't be lazy and use zipties, make this look professional with screw mounts

## Updated to work on Arduino Nano 33 IoT

This fork uses the built-in serial connections on the arduino instead of relying on the SoftwareSerial package, as this package does not work on the Nano 33 IoT.

These are the wiring changes:
- 5-pin side, pin 2 (UART RX) → **Arduino TX** (green)

- 5-pin side, pin 3 (UART TX) → **Arduino RX** (yellow)

Additionally, because the Nano 33 IoT runs on 3.3V while the S8 runs on 5V, you will need to power the Arduino using USB power. Then, to power the S8 sensor, you need to bridge the VUSB contacts on the bottom of the Arduino, then connect the sensor to the VUSB pin. Note that this pin directly uses the USB power and is not regulated to 5V, so an unstable power source might kill the sensor.

## Original Docs: CO<sub>2</sub> monitor based on an Arduino and a SenseAir S8 sensor

This is a low-cost, do-it-yourself CO<sub>2</sub> monitor based on the [SenseAir
S8](https://senseair.com/products/size-counts/s8-lp/) sensor, an
[Arduino Nano
Every](https://store.arduino.cc/products/arduino-nano-every), and a
16x2 LCD display.

The goal is assess air quality, particularly in classrooms and meeting
rooms and with concern about COVID-19. The main source of my
understanding was
[LibreCO2](https://github.com/danielbernalb/LibreCO2). Also see
[this useful
post](https://emariete.com/en/meter-co2-esp8266-nodemcu-sensor-senseair-s8/)
by [Mariete](https://emariete.com/en/).

I did a limited comparison between two of these SenseAir S8-based monitors and
the more expensive [Aranet4](https://aranet.com/products/aranet4/);
[see the results](https://karlduino.org/CO2monitor/docs/Comparison/comparison.html).

[![CO2 monitor tied to a wooden post with a pink cord](https://karlduino.org/CO2monitor/docs/pics/co2monitor_sm.jpg)](https://karlduino.org/CO2monitor/docs/pics/co2monitor.jpg)

### Parts

The cost of the main components is about US$ 60. (Most of the cost is
the sensor, which is about US$ 40. If you're willing to wait a month
for shipping, you can get it much cheaper
[via AliExpress](https://s.click.aliexpress.com/e/_9GsnY9)).

- [SenseAir S8](https://senseair.com/products/size-counts/s8-lp/)
  [![shopping cart icon](docs/pics/shopping-cart.png)](https://amzn.to/3AyzQMa)

- [Arduino Nano Every](https://store.arduino.cc/products/arduino-nano-every)

- 16x2 display with I2C chip
  [![shopping cart icon](docs/pics/shopping-cart.png)](https://amzn.to/3AwG3Z7)

- clear plastic box
  [![shopping cart icon](docs/pics/shopping-cart.png)](https://amzn.to/3R24kxb)

To connect the components, you will need 8 female-to-female jumper
wires, solder, a soldering iron, 2 2-pin male headers for the SenseAir
S8 sensor, and maybe also 2 16-pin male headers for the Arduino (if
you got one without headers installed).

To attach components to the box, you'll need some
small screws (I used 1/2" 4/40 screws for the LCD display), as well as
some _very_ small screws (I used 11/32" 0/80 screws for the Arduino).
I get them from [McMaster-Carr](https://mcmaster.com).


### Code

You'll find the code on [Github](https://github.com/karlduino/CO2monitor).

The first time using the Arduino Nano Every with the Arduino
IDE, you may need to use the Board Manager to install the drivers
for the megaAVR boards. Then when you select the Nano Every as your
board, select `Registers emulation: None (ATMEGA4809)`.

My code for interacting with the SenseAir S8 sensor is based on the
example code in the [S8_UART
library](https://github.com/jcomas/S8_UART).

My code for writing on the LCD (including making custom characters)
was based on the example code with the older [LiquidCrystal_I2C
library](https://github.com/johnrickman/LiquidCrystal_I2C) (which is
not compatible with the Arduino Nano Every).

### Libraries

The code uses the following libraries to interact with the S8 sensor
and the LCD display, respectively.

- [S8_UART](https://www.arduino.cc/reference/en/libraries/s8_uart/)
  [![github](https://kbroman.org/icons16/github-icon.png)](https://github.com/jcomas/S8_UART)

- [LCD_I2C](https://www.arduino.cc/reference/en/libraries/lcd_i2c/)
  [![github](https://kbroman.org/icons16/github-icon.png)](https://github.com/blackhack/LCD_I2C)

In the Arduino IDE, install these libraries with the
Library Manager.

### Assembly instructions

See the
[instructions document](https://karlduino.org/CO2monitor/docs/instructions.html),
which attempts to explain the full process:

- Prepare the box (drilling holes to mount components and cutting
  holes for the sensor and the micro-USB cable)

- Solder headers onto the main components

- Mount the components in the box

- Make connections (see below)

- Load the software

- Adjust the contrast on the LCD display


### Connections

The 16x2 LCD with I2C has a 4-pin connector:

  - 16x2 GND → Arduino GND (black)

  - 16x2 VCC → Arduino VIN (red)

  - 16x2 SDA → Arduino A4 (green)

  - 16x2 SCL → Arduino A5 (yellow)

The SenseAir S8 needs soldering; you could use two pairs of header
pins. See page 3 of
[the
specs](https://rmtplusstoragesenseair.blob.core.windows.net/docs/publicerat/PSP126.pdf)
for pin information, which is not printed on the sensor.

- 4-pin side, pin 1 (G+) → Arduino 5V (red)

- 4-pin side, pin 2 (G0) → Arduino GND (black)

- 5-pin side, pin 2 (UART RX) → Arduino D10 (green)

- 5-pin side, pin 3 (UART TX) → Arduino D11 (yellow)


### License

This work released under the [MIT License](LICENSE.md).
