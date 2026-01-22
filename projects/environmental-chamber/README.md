---
title: "Project: Environment-Controlled Horticulture Chamber"
toc: true
toc-depth: 2
numbersections: true
colorlinks: true
abstract-title: Summary
abstract: |
  Horticulture and HVAC often demand reasonably stringent control of environmental conditions like temperature and humidity.

  This environment-controlled enclosure incorporates heating strips on the chamber interior for temperature control, coupled with an air-pump powered water bubbler for humidity, and an adaptive daylight LED lamp compensating for insufficient light. The bubbler delivers humid air, while a combination of the strip heaters and heating of the water (optional) can elevate the internal temperature of the enclosure to a desired set point.
  
  Builders will gain experience with soldering and electronics assembly, CAD, 3D printing, firmware programming, gas plumbing, power electronics, and hardware-based user-responsive I/O.

  Further extensions can be for automated watering/nutrient delivery and soil humidity/pH monitoring.

  Approximate time: 35 person-blocks.
  (1 block = 1.5 hrs)

header-includes:
#   - \usepackage{draftwatermark}
  - \usepackage{xurl}
---

# Environment-controlled horticulture chamber: Background

## Premise

> This device allows precise control of conditions relevant for plant growth inside an enclosed space, preferably large enough to contain (and sustain!) a plant that would otherwise have trouble thriving in the relatively dry and temperate climate of the northeastern USA (_e.g._ peace lilies, ferns, snake plants).

> Students will build an enclosure that keeps a plant alive and in optimal condition, regardless of location indoors. PID control will be implemented to elevate the humidity and/or temperature set points above ambient conditions, while a calibrated photo-sensor will feed input to a dimmable LED module, to compensate for lighting insufficiency and/or to set up arbitrary illumination scheduling.

> Ideally, students can set up a side-by-side trial with two seedlings of a plant of their choice, one with and one without the enclosure, and compare growth metrics over the course of several weeks.

**Commercial reference:** [Darwin Chambers](https://www.fishersci.com/shop/products/chamber-plant-grow-68-cu-foot/501251778?srsltid=AfmBOooTmNCHZqzkCOdN6UrQs9Lzce82dcCGp5qe8oUM6U17uDQcRz1mJes)

## Chemical Engineering Principles

- Fundamentals (mass and energy balances, composition)
- Process control
- Transport (mass transfer)

## Basic References

- Example of humidity/temp controlled enclosure
- PWM pump and LED control

\newpage

# Prototype project devices

## Hands-on Skills

- Electrical: wiring, soldering, power
- Electronics: hardware, power electronics, signal processing, dynamic I/O
- Programming: Python or Arduino IDE
- Fabrication: CAD or crafting, plumbing
- Basic optics

## Final prototype

![Image of final prototype device](img/PXL_20240715_235052833~3.jpg)

- Humidity/temperature sensor
- PWM-driven diaphragm air pump
- PWM-dimmable LED lamp

\newpage

# Components

## Hardware and electronics

1. Microcontroller unit (MCU): Adafruit Trinket M0 or Arduino Uno R3
1. Temperature/humidity sensor (_e.g._ SHT31D)
1. MCP2221 Breakout board (for sensor comm to control computer)
1. MicroUSB or USBC Cables (computer to MCU/breakout boards)
1. Barrel jack splitter (to +/- terminals or leads)
1. MCU encasement
    - Either CAD-drafted or self-designed
1. Dimmable multicolor LED lamp (https://www.adafruit.com/product/2861)
1. Power supply for lamp (5V and at least 2A)
1. 2x 10K potentiometer (variable resistor)
1. 2x 220 Ohm resistor
    - 1 for LED
    - 1 for motor control
1. 1x 10K Ohm resistor
    - for photosensor
1. 1x 1N4001 diode
1. 1x PN2222 transistor
1. 1x photoresistor or phototransistor (for ambient light sensing)
1. Air pump. Use a diaphragm pump or peristaltic pump, which are self-priming.
    - Diaphragm pump
        - Small sibling (5V powered): https://www.adafruit.com/product/4700
        - Big sibling (12V): https://www.amazon.com/brushless-KVP04-negative-pressure-diaphragm/dp/B096KFKR2J
    - Peristaltic pump (12V)
        - https://www.adafruit.com/product/1150
1. Power supplies for the above pumps
1. Adafruit protoboard or breadboard (4 x 6 cm)

## Tools and equipment

1. Cut/strip/crimp tool
1. Screwdriver
1. Power drill
1. Multimeter
1. Glue gun
1. Balance
1. Rubber stopper
1. Cap/stopper-sealable container (e.g., vacuum flask)
1. Aquarium air stones/bubblers
1. Soldering setup

## Consumables

1. Hook-up wire
1. Terminal connectors
1. Push-fit gas tubing fittings (single and cluster) ([Example here](https://www.sp-spareparts.com/en/p/qsq-6-4-festo))
1. Nylon/silicone tubing
1. Transparent, preferably somewhat airtight plastic container/tub large enough to hold plant of interest
1. Electrical tape

## Software

### Computer

- VS Code (1.91.0)

### Firmware

Onboard the Trinket:
- ```code.py```: RH bubbler/lamp control code
- ```boot.py```: code for importing a USB communications device class (CDC), to open a second "virtual" COM port that listens for incoming commands

On control computer (**update to asyncio implementations**):
- ```HTsense.py```: Testing RH/T sensor outputs
- ```RHcontrol.py```: implementation of RH control module

### Libraries/Packages

On control computer: 
- CircuitPython + dedicated SHT31D sensor package (for T/RH sensor readout)
- Adafruit Blinka (for MCP2221A USB comm breakout board)
- simple-pid (Python PID control library) 
- asyncio (asynchronous I/O communication)

### Extensions

VS Code
1. Serial Monitor (v0.11.0)
1. Arduino (v0.6.0)

# Procedure

## Computer setup

1. Install VS Code or Arduino IDE, or download zipped bundles (for use without admin privileges)
1. Install the libraries and extensions for Arduino and VS Code

## CAD

1. Create a microelectronics case, or use existing container on hand
1. Create a wall/attachment to hold electronics

## Hardware

### General wiring

Five core connections/circuits comprise the enclosure's functionality. The core circuits are as follows:

1. RH/T sensor circuit
1. Pump (motor) circuit
1. LED circuit
1. Photosensor circuit
1. Potentiometer circuit

![Pump circuit schematic. Connect the motor leads to a 12 V power supply, rather than Arduino 5V out. Source: Adafruit. https://learn.adafruit.com/adafruit-arduino-lesson-13-dc-motors/overview](img/image.png)

![LED circuit. Source: Adafruit. https://learn.adafruit.com/adafruit-arduino-lesson-2-leds/overview](img/image-2.png)

![Photosensor circuit. Source: Adafruit. https://learn.adafruit.com/photocells/overview](img/image-1.png)

![Potentiometer circuit. Source: Arduino. https://docs.arduino.cc/learn/electronics/potentiometer-basics/](img/image-3.png)

Random resources (here for now):
Trinket: https://learn.adafruit.com/adafruit-trinket-m0-circuitpython-arduino

I2C intro: https://learn.adafruit.com/working-with-i2c-devices?view=all 

I2C Breakout comms for data logging/readout: https://learn.adafruit.com/jupyter-on-any-computer-with-circuitpython-libraries-and-mcp2221 

MCP2221 on any computer: https://learn.adafruit.com/circuitpython-libraries-on-any-computer-with-mcp2221?view=all

### Prototyping board circuits

1. Designate one section at the top of the board for 12 V power and one section at the bottom of the board for 5 V power.
    - There will be one power line in (12 V). This is fed separately to the pump and Arduino, which regulates the supply to 5 V for the onboard electronics.
1. Solder rails on either side of the 5 V section. These will be used to power the LED, photosensor, and two potentiometers.
1. Solder a horizontal path between the rails for each of the LED and photosensor circuits, including long power leads to accommodate passing through the case to each sensor and a long interface lead to connect to the Arduino.
1. In the 12 V section, fit the transistor, diode, and resistor. Solder in place.
1. Solder long leads for the pump terminals and crimp each loose end with a female quick disconnect.
1. Solder a long lead to connect the resistor and center transistor leg to the Arduino.
1. Solder leads to connect the barrel jack adapter to the 12 V section of the board.
1. Solder a connection between the 12 V ground and 5 V ground.
1. Solder a long lead to connect the 12 V in to the Arduino Vin pin.
1. Solder leads from the bottom of the 5 V rails to connect to the Arduino 5 V and ground pins.
1. Solder leads from the top of the 5 V rails to each of the potentiometers.
1. Solder a long lead from each potentiometer output to connect to the Arduino analog pins.

![Image of prototyping board without potentiometer wiring.](img/PXL_20240714_135124180~3.jpg){width=50%}

![Image of prototyping board with potentiometer wiring.](img/PXL_20240715_233906021.MP.jpg){width=50%}

![Image of prototyping board connected to Arduino pins.](img/PXL_20240715_234010170.jpg)

![Image of potentiometers mounted to case.](img/PXL_20240715_233950152.jpg){width=50%}

### Photocompensator circuit (LED + Photosensor)



### Humidity enclosure and control circuit

Onboard the Trinket M0 there are a total of five digital general-purpose I/O (GPIO) pins.
We will use them as follows:

Pin 0 (A2): PWM input to pump
Pin 1 (A0): 10-bit DAC analog output (?)
Pin 2 (A1): Analog input for photoresistor
Pin 3 (A3): PWM output for LED lamp (24 LEDs x 60 mA max = ~1.5 A power supply. 2 A for good measure.)
Pin 4: Free pin!

We only need one output pin for PWM inputs to the pump.

### Enclosure construction

[Under construction]

### Flow cell

[Under construction]

### Circulation Assembly

[Under construction]

## Firmware

[Stay tuned]

## Operation

[TBD]

# Output

Example output is included in the folder: `project-water-purifier/output` and illustrated in the final prototype image.

# Troubleshooting

## Nothing is working. There's smoke coming out of my device.

At no point should the pump be powered from the Arduino or controlled without the diode. This risks excess current through the Arduino onboard voltage regulator, which will damage the device.

## My motor speed isn't changing with PWM.

The line used to switch the transistor should have a resistor. It should be relatively small (<1K), or else the circuit may not function appropriately.

Example:  

- https://forum.arduino.cc/t/why-does-my-pump-not-stop-pumping/1134698/5
- https://forum.arduino.cc/t/npn-transistor-is-not-able-to-power-motor-with-external-5v-power-supply/1046537/7
- https://electronics.stackexchange.com/questions/346606/12v-volts-being-reduced-through-a-pn2222

Further, to use PWM speed control, the pump must be able to operate with an Arduino-compatible PWM frequency.

Example:  

- https://forum.arduino.cc/t/arduino-dc-variable-speed-water-pump-speed-regulation/1207663/6

If the transistor is not modulating the speed of the motor based on the respective potentiometer, check the input and output lines, and confirm that the grounds are connected (such that a complete circuit can be formed through the Arduino).

Example:   

- https://www.reddit.com/r/arduino/comments/17p0wsl/arduino_uno_slows_down_and_stops_working_after/


# Alternative embodiments and extensions

## Prototype Iterations

### Photosensor options and basic circuit

An initial photosensor circuit was constructed using the same principle as the air quality monitor proof of concept. A phototransistor or photoresistor can be used to convert the light signal in response to liquid analyte concentration.

![Breadboard photosensor prototype](img/PXL_20240610_121450147.jpg){width=50%}


# References

## Background and educational

1. From a plant physiology perspective: https://www.appstate.edu/~neufeldhs/pltphys/waterpotential.htm
2. Humidity standardization and proper metrology: https://www.teos-10.org/pubs/ICPWS2013_WS_TechnicalReport_Humidity_20150211primo.pdf

## Datasheets

1. Transistors
    - https://users.ece.utexas.edu/%7Evalvano/Datasheets/PN2222-D.pdf
    - https://www.mouser.com/datasheet/2/149/PN2222A-371983.pdf
    - https://pdf1.alldatasheet.com/datasheet-pdf/view/5107/MOTOROLA/MPS2222A.html

## Arduino

1. https://docs.arduino.cc/learn/microcontrollers/analog-output/
    - Arduino docs on PWM

## Electronics

1. https://learn.adafruit.com/adafruit-arduino-lesson-13-dc-motors/arduino-code
    - Pump/motor circuit
1. https://www.escomponents.com/resistor-value-tablesguides2
    - Resistor number codes
1. https://electronics.stackexchange.com/questions/346606/12v-volts-being-reduced-through-a-pn2222
    - Use a small resistor with PN2222
1. https://forum.arduino.cc/t/can-i-use-a-12vdc-3a-power-supply-for-vin/598654/5
    - arduino voltage regulator with 12 V input
1. https://forum.arduino.cc/t/12v-power-supply-motor-and-arduino-uno/974856/3
    - power arduino with 12 V and power other device
    - power as few devices through arduino as possible
1. https://forum.arduino.cc/t/powering-an-arduino-nano-with-a-12v-battery/477312/9
    - power arduino from 12 v
1. https://forum.arduino.cc/t/replace-voltage-regulator-on-arduino-mega/402325/3
    - can you replace the voltage regulator? probably not


## Project-specific

### DIY photosensor capability

1. https://www.che.utah.edu/teaching_module/spectrophotometer/


## Commercial products

1. https://www.sawyer.com/products/mini-water-filtration-system
1. https://www.usaberkeyfilters.com/products/travel-berkey-system-stainless-bundle-water-filter/
1. https://www.bigberkeywaterfilters.com/blog/berkey-water-filters-and-tds-readings
1. Home filter teardown
    - https://www.youtube.com/watch?v=8NgdBHzHZTo

## Project extensions

Atlas Scientific on Digikey:  
https://www.digikey.com/en/products/filter/sensor-kits/662?_gl=1%2At52e2s%2A_up%2AMQ..&gclid=CjwKCAjwg8qzBhAoEiwAWagLrHohlK2_Z5QmxKYwpSW47ihrBR-TklAMvOUN4U5xaPaEU2wLhoMzdRoC8WEQAvD_BwE&s=N4IgjCBcoLQBxVAYygMwIYBsDOBTANCAPZQDaIALGAKwBMIh1ADGAGwgC6Avj0A


### Water level sensing (if doing auto-irrigation)

1. https://www.dfrobot.com/product-1863.html
    - Industrial sensor, 4-20 mA output 

### pH monitoring

1. https://atlas-scientific.com/ph
1. https://atlas-scientific.com/embedded-solutions/surveryor-analog-ph-sensor-meter/
1. https://www.dfrobot.com/product-1025.html
