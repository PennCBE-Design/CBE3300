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

### Filter assembly

1. Procure a media housing:
    - a plastic cup with the bottom cut away OR 
    - a 3D printed vessel with two open ends
1. Apply hot glue to the base of the media housing.
1. Place the fine mesh over the hot glue to secure it to the media housing.
1. Trim the edges after the glue cools.
1. Fill the media housing with activated carbon or other filtration media.
1. Repeat mesh attachment on the top of the media housing.
1. Ensure that the media housing can fit over the water reservoir and hold circulating liquid above. If not, design a fixture for the media housing.

### Flow cell

1. Slice and print the 3D files for the flow cell. No special settings are necessary.
    - Ensure the parts are oriented appropriately for printing (avoid unsupported areas, etc).
1. Cut two clear lenses from a plastic cup or sheet stock.
1. Insert one lens into one flow cell viewport recess. Ensure that the lens is flush with the flat surface.
1. Apply hot glue to the interior edge of the recess.
1. Insert the viewport plug into the recess and hold until cool.
1. Repeat for the opposite viewport.
1. Confirm the system seal by plugging the base with hot glue or electrical tape and filling the volume with liquid.
1. Allow the system to sit for 1-5 minutes. Note the location of any leaks.
    - If leaks occur, remove the affected plugs. Repeat the process as needed until the system is leak-tight.

### Circulation Assembly

1. Slice and print the 3D files for the case. No special settings are necessary.
    - Ensure the parts are oriented appropriately for printing (avoid unsupported areas, etc).
1. Attach the Arduino to the bottom case shell and secure with pins.
    - It may be necessary to use pliers to insert the pins.
    - It may be necessary to increase or reduce the pin diameter with tape, sanding, or other means.
1. Fit the pump to the front plate and secure with screws and nuts.
1. Fit the prototyping board to the front plate and secure with glue, screws, wire loops, or pins.
1. Fit the back plate and secure the potentiometer bases, passing the knobs through the plate.
1. Fit and fix the barrel jack adapter to the back plate. Connect the 12 V power leads to the prototyping board.
1. Attach the top of the case to the unit.
1. Connect the flow cell to the pump using large diameter silicone tubing.
1. Fit the flow cell mounting brackets and situate the flow cell, photosensor, and LED. Secure the outer bracket component.

![Image of final assembly step 1 for flow cell.](img/PXL_20240715_234513860.jpg){width=50%}

## Firmware

1. Open `project-water-purifier/program/program.ino` in your editor.
1. If alternative program values are required, set the `readVals();` parameters in `loop()` at the bottom of the program.
    - Default number of readings before check: `int n = 10`
    - Default number of milliseconds to wait between each reading: `int d = 1000`
    - To set to 1 reading with 0.1 second delay: `readVals(1, 100);`
1. Connect the Arduino to the computer and identify its serial port.
1. Upload `project-water-purifier/program/program.ino` to the Arduino.

## Operation

1. Prepare a test solution reservoir using approximately 50 \textmu L (1 drop) of red food dye in 30 mL of water.
1. Situate the filter assembly over the reservoir.
1. Secure the circulation assembly near the reservoir and route tubing to the flow cell inlet and from the pump outlet such that all liquid recirculates in the reservoir.
1. Power on the device.
1. The diaphragm or peristaltic pumps do not need priming. The device will run for a period of time, sample, and check whether to continue operating.
1. If necessary, adjust the pump speed or concentration threshold using the respective potentiometer.
1. At any point, the device will output the flow cell reading for observation.
1. Once the flow cell value reaches the threshold, pump operation will cease.

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

## The pump is moving water the wrong way.

If the motor is going the wrong direction with a peristaltic pump, reverse the polarity of the leads. With a diaphram pump, the device flows water only in one direction.

## Water is coming out of the flow cell sides.

If the viewports on the flow cell are leaking, remove the element altogether, and reapply. It may be easier to first secure the lens, then attach the plug -- possibly by applying glue to the plug before seating, rather than applying more glue to the recess.

## There are bubbles in the water coming out of the pump.

This may be due to leaks in the flow cell. Small air gaps may be okay. Everything leaks under the right conditions. As long as it does not interfere with measurement, a small leak that allows air into the flow cell during operation (but not water out) is acceptable for short-term operation.

## The analyte is not being filtered from the water.

There may be difficulty matching adsorbent to analyte and timescale of removal. For example, using natural substances such as spices with commercial filters and a small number of passes produced limited results. Changing to exclusively activated carbon and chemical indicator (food dye) resolved the color detection issue. However, carbon sediment mixed with the water in the process of filtration. The activated carbon should be rinsed before use, and mechanical filtration by nonwoven materials or porous media may be necessary for complete removal of suspended solids. Note that activated carbon may react with content in regular tap water to _produce_ precipitate. For this reason, the demonstration of activated carbon filtering dye is best performed with DI or distilled water.

# Alternative embodiments and extensions

## Prototype Iterations

### Photosensor options and basic circuit

An initial photosensor circuit was constructed using the same principle as the air quality monitor proof of concept. A phototransistor or photoresistor can be used to convert the light signal in response to liquid analyte concentration.

![Breadboard photosensor prototype](img/PXL_20240610_121450147.jpg){width=50%}

### Alginate bead synthesis

Alginate bead synthesis was performed in a variety of permutations. It was ultimately deemed too fragile to proceed in the given timeframe.

![](img/PXL_20240617_180910802.jpg){width=25%}

![](img/PXL_20240617_183019151.jpg){width=25%}

![](img/PXL_20240617_183025425.jpg){width=25%}

![](img/PXL_20240617_183507212.jpg){width=25%}

![](img/PXL_20240617_183510112.jpg){width=25%}

![](img/PXL_20240617_183759525.jpg){width=25%}

![](img/PXL_20240617_184203431.jpg){width=25%}

### Conductivity and Total Dissolved Solids (TDS)

![Prototype using TDS probe](img/PXL_20240617_153021265.jpg){width=50%}

![Prototype using TDS probe. Image 2.](img/PXL_20240617_153056036.jpg){width=50%}

# References

## Background and educational

1. https://www.ce.washington.edu/news/article/2023-02-01/research-splash-color
    - case study colored dye estuary water
1. https://pubs.acs.org/doi/pdf/10.1021/es60058a005
    - waste/water filtration concepts (legacy publication)
1. Detecting contaminants - state-of-the-art
    - https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7126548/
1. https://www.jpl.nasa.gov/edu/learn/project/make-a-water-filter/
1. https://edu.rsc.org/experiments/carbon-filtration-and-activated-charcoal/412.article
1. https://www.sciencebuddies.org/science-fair-projects/project-ideas/Chem_p108/chemistry/which-filtration-material-leads-to-the-best-drinking-water


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

### Activated carbon effectiveness

1. https://www.freshwatersystems.com/blogs/blog/activated-carbon-filters-101
1. https://www.youtube.com/watch?v=wdfZ9r8p7A0
    - activated carbon remove titration indicator
1. https://www.youtube.com/watch?v=ryu7fH-mxTk
    - activated carbon remove grape juice color
1. https://www.youtube.com/watch?v=L6ZWkO6tPTE
    - removing color from oil
1. https://www.reddit.com/r/askscience/comments/4sk8sb/practical_filtration_method_to_remove_lead_from/
    - activated carbon filtering lead
1. https://www.youtube.com/watch?v=Vqj4VAeR0bc
    - activated carbon static color removal
1. https://www.youtube.com/watch?v=son8yyD-T_E
    - Verify activated carbon vs charcoal -- activated carbon shows bubbles


### DIY photosensor capability

1. https://www.che.utah.edu/teaching_module/spectrophotometer/


### Cuvette flow cell

1. https://www.biocompare.com/Bench-Tips/175408-Selecting-the-Right-Volume-to-Measure-Your-Samples/#:~:text=Cuvettes,will%20work%20in%20most%20spectrophotometers.
    - sizing cuvette
1. https://www.agilent.com/cs/library/technicaloverviews/public/te-cary-3500-uv-vis-variable-path-length-cell-holder-5994-5781en-agilent.pdf
    - cuvette design
1. https://icuvets.com/en/understanding-cuvette-volume-material-path-length-etc/
    - cuvette design


### Dyes

1. https://pubs.acs.org/doi/10.1021/ie9012437
    - methylene blue dye uv degradation (LEDs)
1. https://www.researchgate.net/figure/Organic-dyes-that-are-used-for-degradation-under-UV-light_fig10_314108580
    - Organic dyes (UV degradable)
1. https://pubs.rsc.org/en/content/articlehtml/2022/ra/d2ra05779d
    - Investigating degradation without photocatalysts
1. https://www.nutraceuticalsworld.com/issues/2017-03/view_breaking-news/basf-launches-10-beta-carotene-colorant/
    - natural orange coloring

## Open-source projects and other embodiments

1. https://hackaday.com/2023/03/20/remote-water-quality-monitoring/
    - https://www.instructables.com/Arduino-Water-Quality-Monitoring-System/
    - https://www.instructables.com/member/RowlesGroupResearch/
    - https://www.rowlesresearch.com/work/research
1. https://hackaday.com/2015/08/27/hackaday-prize-semifinalist-water-quality-monitoring/
1. https://hackaday.io/project/6835-c4derpillar-open-ce-cd
1. https://hackaday.com/2014/10/17/open-source-water-quality-tester/
    - https://www.academia.edu/8319858/Open-source_mobile_water_quality_testing_platform
    - https://hal.science/hal-02119690/document
1. https://www.pottersforpeace.org/ceramic-water-filter-project
1. https://makezine.com/article/education/open-sourcing-plans-low-tech-high-thinking-water-filters/
1. https://unfold.be/pages/open-source-waterfilter.html
1. https://faq.hydroviv.com/en-US
1. https://jerrycanfilter.com/opensource/
1. https://www.youtube.com/watch?v=3uzXeCnzf0c
    - diy water filter (sediment, etc)
1. https://www.youtube.com/watch?v=y0OsnJ0Uz5Q
    - biosand filter
1. https://news.mit.edu/2021/filters-sapwood-purify-water-0325
    - bio wood filter
    - http://www.xylemwaterfilter.org/science-projects/


## Commercial products

1. https://www.sawyer.com/products/mini-water-filtration-system
1. https://www.usaberkeyfilters.com/products/travel-berkey-system-stainless-bundle-water-filter/
1. https://www.bigberkeywaterfilters.com/blog/berkey-water-filters-and-tds-readings
1. Home filter teardown
    - https://www.youtube.com/watch?v=8NgdBHzHZTo

## Project extensions

Atlas Scientific on Digikey:  
https://www.digikey.com/en/products/filter/sensor-kits/662?_gl=1%2At52e2s%2A_up%2AMQ..&gclid=CjwKCAjwg8qzBhAoEiwAWagLrHohlK2_Z5QmxKYwpSW47ihrBR-TklAMvOUN4U5xaPaEU2wLhoMzdRoC8WEQAvD_BwE&s=N4IgjCBcoLQBxVAYygMwIYBsDOBTANCAPZQDaIALGAKwBMIh1ADGAGwgC6Avj0A


### Level sensing

1. https://www.dfrobot.com/product-1863.html
    - Industrial sensor, 4-20 mA output 

### Oxidation-reduction potential (ORP) and chlorine content

1. https://forum.arduino.cc/t/chlorine/52113/5
    - Arduino forum discussion on measuring chlorine content
1. https://basinpooldesigns.com/wp-content/uploads/2018/10/Chlorine-PPM-to-ORP-conversion-chart.pdf
    - Relating ORP to chlorine content
1. https://atlas-scientific.com/blog/orp-measurement-for-chlorine/
    - oxidation-reduction potential (ORP) sensor
1. https://www.homedepot.com/p/Waterproof-Bluetooth-Pool-ORP-and-Temperature-Tester-ORP650B/305750913
    - commercial orp sensor
- https://www.dfrobot.com/product-1071.html
    - analog Arduino-compatible sensor
1. https://www.us.endress.com/en/field-instruments-overview/liquid-analysis-product-overview/free-chlorine-digital-sensor-ccs51d?t.tabId=product-overview
1. https://sbcontrol.com/probes-selection/ppm-sensors/
    - Commercial chlorine sensor
1. https://sea.omega.com/ph/pptst/CLDTX_FCLTX.html
    - Chlorine, 4-20 mA output
1. https://sensorex.com/product/fcl-amperometric-free-chlorine-sensor/
    - amperometric, 4-20 mA output, free chlorine sensor
1. https://wiki.seeedstudio.com/Grove-ORP-Sensor-kit/
1. https://www.vernier.com/product/orp-sensor/?wcpbc-manual-country=US


### pH

1. https://atlas-scientific.com/ph
1. https://atlas-scientific.com/embedded-solutions/surveryor-analog-ph-sensor-meter/
1. https://www.dfrobot.com/product-1025.html


### Conductivity

1. https://atlas-scientific.com/embedded-solutions/ezo-conductivity-circuit/
1. https://atlas-scientific.com/kits/conductivity-k-1-0-kit/
