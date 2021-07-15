---
layout: post
title:  "Ben Eater 8-bit computer project"
date:   2021-06-26 00:51:00 +0200
categories: hardware
tags: project
---
{% include math.html %}
<!--more-->

# Table of Contents
- [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
    - [Power supply](#power-supply)
    - [Breadboards](#breadboards)
      - [Sample closed circuit](#sample-closed-circuit)
      - [Common mistakes](#common-mistakes)
  - [Clock module](#clock-module)
    - [Astable 555 timer](#astable-555-timer)
      - [Step response](#step-response)
    - [Speed](#speed)
    - [Noise](#noise)

## Introduction
Ben Eater is an online educator on computer-related topics from which I'm following his 8-bit computer project. [https://eater.net/8bit](https://eater.net/8bit). The computer is composed of different modules, which are built on breadboards. The modules are the clock module, registers and ALU (arithmetic and logic unit) module, RAM (random access memory) and program counter module, and output and control logic module.

To refresh circuit analysis basics check [this post]({{ site.url }}/hardware/2021/06/26/8-EE-cheatsheet.html).

### Power supply
We will be using a 5V (volt) power source, which Ben has crafted by cutting off the wires of a cellphone charger.
* Safety rules before using anything related to electricity:
  * Dont plug a lot of stuff into a single extension cord
  * Keep eletrcical things far away from water
  * Never connect LEDs directly to  5 volt source without a current limiting resistor or you will damage the LED.
  * Most 74LSxx logic chips have internal resistors that let you connect LEDS directly to the outputs without a resistor. However, the 555 timer, 74189 RAM, 28C16 EEPROM and 74HC595 do not.
  * For most reliable results you may want to connect a \\(220\Omega\\) resistor in series with every LED.

### Breadboards
![Breadboard]({{ site.url }}/images/8bit/clock/breadboard.PNG)
* The breadboard is composed of metalical strips that can be linked together with jump wires and electrical components.
* The A-J holes constitute columns (each) and the numbers constitute rows.
  * ![Breadboard]({{ site.url }}/images/8bit/clock/bb1.PNG)
  * A1, B1, C1, D1 and E1 are all connected, and not connected to any other wholes in the breadboard.
  * The individual power buses are not connected to each other (only the column itself).
* The + - vertical strips are called buses or rails and are used to deliver power.
  * The red (+) line will connect to the positive terminal of the battery
  * The black or blue (-) line will connect to the negative terminal of the battery
  * The busline may not connect the entire length of the board, unbroken lines show that the bus indeed connects the entire length
* The middle space of the breadboard is designed to fit the integrated circuits (IC) (chips) that come with dual in-line package (DIP), meaning they come in two columns of pins of which each individual pin is connected to its own row.

#### Sample closed circuit
![Breadboard]({{ site.url }}/images/8bit/clock/bb2.PNG)
* The long leg is the positive side of the LED and it's called anode, and the shorter leg is for the negative side and it's called cathode.
  * Plugging it backwards wont break the led, but it will block the current flow of the circuit.

#### Common mistakes
* Setting the LD legs in different rows
* Loose wires not pushed deep into the wholes
* Disrespecting the polarity of the components
  * LEDs have polarity: long leg +, short leg -
  * Resistors don't have polarity
* Jumper wires:
  * Long flexible wires are convinient for simple circuits but will spagetti-like mess up complex circuits
  * Pre-cut jump wires have predefined lengths which limit customization
  * hookup wire and wire strippers to make your own jumper wires:
    * Make sure to buy solid-core wire and not stranded wire as the latter is made up of multiple individual strands which makes the ends much harder to push into a breadboard
    * As a last resort if stranded wired cannot be solded it can be twisted to increase its tightness.
    * To leave enough space for bending the wires and plug them into the board leave a room of 3 more wholes.
* Integrated circuits:
  * Not connecting an integrated circuit in the middle of the board as that will connect both ends of the IC (making a shortircuit)

##  Clock module
The clock coordinates everything, it sets the timing of everything.
* Components included in the kit:
  * 1 Breadboard
    * Great for experiencing circuits, not intended for permanent ones.
    * In addition, for the project you'll need
    * jumper wires
    * wire strippers
    * wire cutters
    * needle nose pliers
  * 1 Jumper wire kit
    * With 140 pre-cut wires
  * 1 Power plug
  * 1 Power plug adapter
  * 3 555 IC timers
  * 1 74LS04 hex inverter
  * 1 74LS08 quad AND gate
  * 1 74LS32 quad OR gate
  * 1 Momentary pushbutton
  * 1 \\(1M\Omega\\) potentiometer
  * 1 slide switch
  * 5 \\(220\Omega\\) resistors (red-red-brown)
  * 10 \\(1k\Omega\\) resistors (brown-black-red)
  * 5 \\(1M\Omega\\) resistors (brown-black-green)
  * 5 Yellow LEDs
  * 5 Blue LEDs
  * 5 \\(0.01\mu F\\) capacitors
    * The microfarad (symbolized µF) is a unit of capacitance.
  * 5 \\(0.1\mu F\\) capacitors
  * 5 \\(1\mu F\\) capacitors
  * 5 \\(10\mu F\\) capacitors
* We are using the 555 timer IC (integrated circuit (chip) below)
![404]({{ site.url }}/images/8bit/clock/555.PNG)

* Our clock is adjustable-speed (from less than 1Hz to a few hundred Hz).
![Hertz]({{ site.url }}/images/8bit/clock/1hz.jpg)
   *  One Hertz (Hz) is defined as one cycle per second
* The clock can also be put into a manual mode where you push a button to advance each clock cycle. (Useful for debugging)
* At this point I'm assuming that the goal of the clock module is to provide an output voltage terminal that alternates between high and low voltage with a frequency between 1-100 Herz. Then use one clock cycle as a unit of time to determine operation slots.
  * Slots could be used to synchronise operations (i.e. to ensure that the BUS is only being used by 1 party at a time)

### Astable 555 timer
![404]({{ site.url }}/images/8bit/clock/intro.PNG)
* We control the timing with 2 resistors and a capacitor
  * The resistor connected from \\(V_{cc}\\) to pin 7 is of \\(1k\Omega\\)
  * The resistor from pin 7 to pin 6 has \\(100k\Omega\\).
  * The capacitor from pin 2 to ground is \\(1\mu F\\) (whose negative terminal is connected to ground)
* The breadboard rails are +5V and 0V (ground) although the voltage range of the 555 timer is typically 4.5 to 16 volts.
  * The output of the 555 timer is around 3.3V and at least 2.75V
* The LED is connected to the output pin (3) and with a \\(220\Omega\\) as the current comming out of the output pin might burn it. The LED should alternate between on and off.
* This is the schematic of the circuit (including the inside of the 555 timer):
![404]({{ site.url }}/images/8bit/clock/555_circuit.PNG)
  * The [opamps]({{ site.url }}/hardware/2021/06/26/8-EE-cheatsheet.html#op-amp-circuits) (triangles with +-) are in a comparator setting
![404]({{ site.url }}/images/8bit/clock/opamp105.gif)
    * The comparator output signal specifies whether the voltage input (\\(v_{in}\\)) is above (\\(v_{out}=1\\)) or below (\\(v_{out}=0\\)) the reference voltage (\\(v_{ref}\\)).
  * The 3 resistors inside the 555 timer are probably \\(5k\Omega\\) each and they form a [voltage divider circuit]({{ site.url }}/hardware/2021/06/26/8-EE-cheatsheet.html#voltage-divider-circuits)
    * Therefore with a 5V source the voltage drop of the bottom resistor is \\(5V\cdot\frac{5k\Omega}{15\Omega}\approx 1.67V\\) and the voltage drop of the 2 bottom resistors combined is \\(5V\cdot\frac{10k\Omega}{15\Omega}\approx 3.33V\\) (recall that all voltage values are given in relation to ground).
      * Therefore the reference voltage of the top comparator is 2.33V and the input voltage of the bottom comparator is 1.67V
  * You can also see that the output of the comparators feed the R and S terminals of an SR latch
![404]({{ site.url }}/images/8bit/clock/sr.PNG)

#### Step response
* Assume the initial voltage of the [capacitor]({{ site.url}}/hardware/2021/06/26/8-EE-cheatsheet.html#capacitor-c) is 0V and that the circuit has been powered off for a while
* The bottom comprator has \\(v_{in} = 1.67V\\) and \\(v_{ref} = 0V\\) as the capacitor had 0 energy stored. Since \\(v_{in} \gt v_{ref} \implies v_{out} = High \implies S = 1\\)
* The top comprator has \\(v_{in} = 0V\\) and \\(v_{ref} = 3.33V\\). Since \\(v_{in} \lt v_{ref} \implies v_{out} = Low \implies R = 0\\)
  * From the truth table we see that S = 1 and R = 0 yield a high Q.
  * \\(\overline{Q}\\) will be low and the transistor connected to pin 7 will not conduct.
* The capacitor starts increasing its voltage and at the beginning it's low enough such that both comparators mantain the previous output but as soon as the capacitor reaches a voltage higher than 1.67 the bottom capacitor has \\(v_{in} \lt v_{ref} \implies v_{out} = Low \implies S = 0\\)
  * While the capacitor has a voltage between 1.68 and 3.32 the top capacitor will keep \\(v_{in} \lt v_{ref} \implies v_{out} = Low \implies R = 0\\)
    * SR = 00 means that Q which mantain whichever state it had, which in this is case was high and therefore \\(\overline{Q}\\) will be kept low and the transistor connected to pin 7 wont flow yet.
  * As the capacitor surpases 3.34 the top comparator will have \\(v_{in} \gt v_{ref} \implies v_{out} = High \implies R = 1\\)
    * SR = 01 means Q is low and \\(\overline{Q}\\) is high. The LED is now off.
    * Since \\(\overline{Q}\\) is high the transistor will be connected to the branch emerging after the \\(1k\Omega\\) resistor, which since connects directly to ground it has zero resistance and all the current the previously flowed down pins 6, 2 and the capacitor won't flow there as all of it will be discharged in pin 7
  * Similarly the capacitor is also connected to the low resistance (although it has to go through a \\(100k\Omega)\\) resistor, recall that the inputs of an opamp have a very large resistance (not explicitly stated in the circuit), therefore the capacitor will also discharge via pin 7, and it's voltage will start to drop.
    * The top comparator will be the first one to switch from hight to low output, which would leave SR = 00, and mantain the current status quo.
    * As the voltage of the capacitor drops below 1.66 volts the bottom comprator will switch from low to high output, which leaves SR = 10, which sets Q high (LED is back on) and \\(\overline{Q}\\) is low, which disconnects the discharger and everything is set back to the starting point (except that the capacitor might not have been fully discharged before it starts charging againg)

### Speed
* The flashing frequency of the led is determined by Q which is determined by how quickly the capcitor charges and discharges
  * This is determined by how much current flows into the capacitor
  * The more resistance in the resistors, the less current flowing into the capacitor and the slower it's going to charge (and also the slower it's going to discharge as fewer current will be travelling back to the discharger)
  * The larger the capacitance the longer it takes to charge/discharge. From here it follows that the bigger R * C the slower the clock
* The reason Ben has chosen 1k and 100k resistors is because charging will go through 101k resistance and discharging through 100k resistance making the high and low Q values of almost identical time.
  * The charge time (Q output high) of the 555 timer given by its data sheet is: \\(t_1=0.693(R_a + R_b)C\\) with \\(R_a\\) being our 1k resistor and \\(R_b\\) the 100k one.
  * Discharge time (Q output low): \\(t_2 = 0.693 \cdot R_b \cdot C\\)
  * The total period is \\(T = t_1 + t_2 = 0.693(R_a + 2R_b) C\\) which in our case is 0.139 s
  * The frequency (of oscillation) is \\(\frac{1}{T}=\frac{1}{0.139}=7.19\\) which means that the LED is flashing about 7 times per second
* We can replace the 100k resistor with a 1k resistor and a potentiometer in series (so when the potentiometer has 0 resistance there's at least a 1k ressitor between pin 6 and 7) to manually adjust the speed of the clock


### Noise
* Adding a \\(.01\mu F\\) capacitor from ground to pin 5 is recommended by the 555 timer datasheet as it clears the noise from low to high
![404]({{ site.url }}/images/8bit/clock/noise.PNG)
* Our power supply (typically with curled wires) may act "not perfect" (i.e. as an inductor sometimes) and sometimes components (specially transistors) might get more volts than they actually desire at times.
![404]({{ site.url }}/images/8bit/clock/noise2.PNG)
  * Adding another \\(.01\mu F\\) capacitor parallel to the power supply (i.e each leg on a + and - hole respectively in the breadboard) may help reduce dangerous voltage jumps.
  * Ideally you'd want to have this capacitor right next to the power pins of the chip but there aint that much space in a breadboard.
* Low voltage on pin number 4 triggers a SR = 01 that overwrites whatever is going on with the comparators and resets (turns off) the SR latch. Therefore the datasheet recomends to hookup pin 4 directly to 5V power supply

When we add all the noise and manual speed adjustments we end up with the followin breadboard:

![404]({{ site.url }}/images/8bit/clock/555_final.PNG)
