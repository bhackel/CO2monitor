## CO2 monitor

This is a DIY CO2 monitor based on the [SenseAir
S8](https://senseair.com/products/size-counts/s8-lp/) sensor, an
arduino, and a 16x2 LCD display.

The goal is assess air quality, particularly in classrooms and with
concern about COVID-19. The main source of my understanding was [LibreCO2](https://github.com/danielbernalb/LibreCO2).

Here are the parts I used:

- [SenseAir S8](https://senseair.com/products/size-counts/s8-lp/) from
  [Amazon](https://amzn.to/3AyzQMa)
- [Arduino Nano Every](https://store.arduino.cc/products/arduino-nano-every)
  or [Arduino Nano](https://store.arduino.cc/products/arduino-nano)
- 16x2 display with I2C chip from [Amazon](https://amzn.to/3AwG3Z7)
- plastic box from [Amazon](https://amzn.to/3R24kxb)

Also some wires, include female-female jumpers, and solder and a
soldering iron (just to get wires onto the SenseAir S8), and some
small screws, as well as some _very_ small screws.

If you're using an Arduino Nano Every, I found that, in the Arduino
IDE, you need to first use the Board Manager to install the drivers
for the megaAVR boards, and then when you select Nano Every, use
`Registers emulation: None (ATMEGA4809)`.

My code for interacting with the SenseAir S8 sensor is based on the
example code in the [S8_UART
library](https://github.com/jcomas/S8_UART).

My code for writing on the LCD (including making the custom character
for the subscript 2) was based on the example code with the
older [LiquidCrystal_I2C
library](https://github.com/johnrickman/LiquidCrystal_I2C) (which is
not compatible with the Arduino Nano Every).

## Libraries

- [S8_UART](https://www.arduino.cc/reference/en/libraries/s8_uart/)
  ([github)(https://github.com/jcomas/S8_UART))
- [LCD_I2C](https://www.arduino.cc/reference/en/libraries/lcd_i2c/)
  ([github)(https://github.com/blackhack/LCD_I2C))

## Connections

- The 16x2 LCD with I2C has a 4-pin connector:

  - 16x2 GND -> Arduino Nano GND (black)
  - 16x2 VCC -> Arduino Nano VIN (red)
  - 16x2 SDA -> Arduino Nano A4 (green)
  - 16x2 SCL -> Arduino Nano A5 (yellow)

- The SenseAir S8 needs soldering (needs confirmation)

  - 4-pin side, pin 1 (G+) -> Arduino Nano 5V (red)
  - 4-pin side, pin 2 (G0) -> Arduino Nano GND (black)
  - 5-pin side, pin 2 -> Arduino Nano D7 (green)
  - 5-pin side, pin 3 -> Arduino Nano D6 (yellow)

## License

This work released under the [MIT License](LICENSE.md).
