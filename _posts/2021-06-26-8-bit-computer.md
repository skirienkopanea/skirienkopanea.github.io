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
      - [Schematic](#schematic)
      - [Step response](#step-response)
      - [Speed](#speed)
      - [Noise](#noise)
      - [Tinkercad](#tinkercad)
    - [Monostable 555 timer](#monostable-555-timer)
      - [Schematic](#schematic-1)
      - [Step response](#step-response-1)
      - [Noise](#noise-1)
      - [Tinkercad](#tinkercad-1)
    - [Bistable 555 timer](#bistable-555-timer)
      - [Schematic](#schematic-2)
      - [Tinkercad](#tinkercad-2)

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
  * 5 \\(0.1\mu F\\) capacitors
  * 5 \\(1\mu F\\) capacitors
  * 5 \\(10\mu F\\) capacitors
* We are using the 555 timer IC (integrated circuit (chip) below)
![404]({{ site.url }}/images/8bit/clock/555.png)

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
* It is called astable because it always alternates states

#### Schematic
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

#### Speed
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

#### Noise
* Adding a \\(.01\mu F\\) capacitor from ground to pin 5 is recommended by the 555 timer datasheet as it clears the noise from low to high
![404]({{ site.url }}/images/8bit/clock/noise.PNG)
* Our power supply (typically with curled wires) may act "not perfect" (i.e. as an inductor sometimes) and sometimes components (specially transistors) might get more volts than they actually desire at times.
![404]({{ site.url }}/images/8bit/clock/noise2.PNG)
  * Adding another \\(.01\mu F\\) capacitor parallel to the power supply (i.e each leg on a + and - hole respectively in the breadboard) may help reduce dangerous voltage jumps.
  * Ideally you'd want to have this capacitor right next to the power pins of the chip but there aint that much space in a breadboard.
* Low voltage on pin number 4 triggers a SR = 01 that overwrites whatever is going on with the comparators and resets (turns off) the SR latch. Therefore the datasheet recomends to hookup pin 4 directly to 5V power supply

When we add all the noise and manual speed adjustments we end up with the following breadboard:

![404]({{ site.url }}/images/8bit/clock/555_final.PNG)

#### Tinkercad
* Tinkercad of the [original schematic]({{ site.url }}/hardware/2021/06/26/8-bit-computer.html#schematic)
![404]({{ site.url }}/images/8bit/clock/clock1.png)
[Open tinkercad](https://www.tinkercad.com/things/kRi8HItiSbm-555-timer-p1/)

* Tinkercad of the upgraded circuit
  * \\(1M\Omega\\) Potentiometer in series with Resistor B, which it decreases from \\(100k\Omega\\) to \\(1k\Omega\\) to manually adjust the speed.
  * \\(.01\mu F\\) capacitor parallel to power supply to control voltage spike noise
  * \\(.01\mu F\\) capacitor from ground to pin 5 to control low to high clock signal noise
  * \\(V_{cc}\\) to pin 4 to avoid unvoluntary clock resets
![404]({{ site.url }}/images/8bit/clock/clock2.PNG)
[Open tinkercad](https://www.tinkercad.com/things/0G7nymUncAr-555-timer-p2)

* Ben replica
  * (Not inteded to implement in real life) Replaced the polarized capacitor with a small capacitor as it takes less space on the breadboard (it still has \\(1\mu F\\)).
  * Shifted (aesthitically only) components to take less space.
  * Placed everything on the breadboard
![404]({{ site.url }}/images/8bit/clock/clock3.PNG)
[Open tinkercad](https://www.tinkercad.com/things/4aTt5fsfbwB-555-timer-p3)

### Monostable 555 timer
* We want to be able to manually advance 1 clock cycle
* Although a simple pushbutton seems to be enough to create cycles
![404]({{ site.url }}/images/8bit/clock/pushbutton.PNG)
  * In reality the push button mechanics might unvoluntarily create additional clock cycles as the metal bounces additional times, not susceptible to the human eye
![404]({{ site.url }}/images/8bit/clock/bounce.PNG)
* A debouncing circuit deals with this issue
![404]({{ site.url }}/images/8bit/clock/monostable.PNG)
   * The duration of the high signal is determined by the capacitance times resitance of the two components on the right.
* It is called monostable because it only has 1 stable status (low)

#### Schematic
![404]({{ site.url }}/images/8bit/clock/555_circuit2.PNG)
* Like the previous 555 timer circuit, there's a resistor connected to pin 7 (discharge), but here it is also directly connected to pin 6 (threshold) as there is no resitor "b".
* The capacitor (which determines the duration of the signal together with resistor a (the \\(1M\Omega\\) one), is also connected directly to pin 6 and 7
* The output lights a LED in series with a \\(220\Omega\\) resistor.
* Pin 2 is connected to a node that has a \\(1k\Omega\\) resistor directly connected to \\(V_{cc}\\) on the positive terminal and a push button directly connected to ground on the negative terminal
  * I'ts like a [voltage divider]({{ site.url }}/hardware/2021/06/26/8-EE-cheatsheet.html#voltage-divider-circuits) with \\(R_1\\) being 1k resistor and \\(R_2\\) the switch.
    * \\(v_0=v_s\frac{R_2}{R_1+R_2}\\)
    * When \\(R_2\\) is an open circuit it has infinity resistance, which gives \\(v_0=v_s\frac{\frac{R_2}{R_2}}{\frac{R_1}{R_2}+\frac{R_2}{R_2}}=v_s\frac{1}{\frac{1}{\infty}+1}=v_s\\)
    * When \\(R_2\\) is a short circuit then \\(v_0=v_s\frac{0}{R_1+0}=0\\)
  * When the push button is open, the voltage of pin 2 is \\(v_{cc}\\)
    * The bottom comparator sends output 0 = S (which either resets or leaves current state)
  * When the push button is closed, the voltage of pin 2 is 0
    * The bottom comparator sends output 1 = S (which then sets the SR latch and triggers a high output

#### Step response
* Initally the push button is open so the bottom comparator sets S = 0, the LED is off and so is \\(\overline{Q}\\) high, which enables the discharge transistor. The capacitor which has 0V, is not able to charge, therefore the top comparator will mantain R = 0. The current state is kept (led off) until we intervene
* Only after pushing (and release) the button we can triger SR = 10 as we made the voltage of pin 2 to be 0 and hence the bottom comparator is able to set S = 1, which makes Q high and \\(\overline{Q}\\) low.
  * This disables the discharge transistor and allows the capacitor to increase its voltage, the capacitor increases its voltage and when it's above 3.34V it will reset the clock (given that the pusshbutton is not held on, otherwise we encounter SR = 11 which in an SR latch is an illegal configuration (it gives unpredictable output))
  * As long as we dont deliberately hold the push button for too long this implementation should deal with the noise from the push button bounces
  * After the capacitor triggers the reset, \\(\overline{Q}\\) becomes high and enables the discharge again, since there is no resistor between discharge and the capacitor it will bleed out it's voltage very quickly, which immideately sets R back to 0.
    * Therefore it's very unlikely to achieve SR = 11 by doing very fast clicks (or rebounds), it should only possible by deliberately holding the button
![404]({{ site.url }}/images/8bit/clock/monostable2.PNG)

#### Noise
Datasheet recomends:
* \\(.01\mu F\\) capacitor from ground to pin 5
* 5V to pin 4

#### Tinkercad
* Tinkercad of the [original schematic]({{ site.url }}/hardware/2021/06/26/8-bit-computer.html#schematic-1)
![404]({{ site.url }}/images/8bit/clock/clock4.PNG)
[Open tinkercad](https://www.tinkercad.com/things/c5hBciREsFx-555-timer-p5)

* Ben replica (with noise upgrade)
![404]({{ site.url }}/images/8bit/clock/clock5.PNG)
[Open tinkercad](https://www.tinkercad.com/things/cBU2bNvFG8y-555-timer-p6)

### Bistable 555 timer
* We could use a switch to manually alternate between the high and low clock output states
  * However if we just use a mechanical switch we face the same rebound issue
* Since the 555 timer has a built-in SR latch, we can we can use it to set/reset the clock style as it also acts as a debouncer (because it latches the last state).
  * We use a switch that is "break before make"
![404]({{ site.url }}/images/8bit/clock/breakbeforemake.PNG)
    * The bouncing is always between the next step and the break, so any bouncing is not harmful as the desired result has been already latched as we'll show below

![404]({{ site.url }}/images/8bit/clock/clock7.PNG)
* It is called bistable because both high and low output states are latched

#### Schematic
![404]({{ site.url }}/images/8bit/clock/clock6.PNG)
* We connect the threshold pin to the ground and just use the trigger pin (2) to set S = 1 (via the bottom comparator) and the reset pin (4) to directly (skipping the top comparator) set R = 1
  * Since threshold is always below 3.33, R will always be 0 except when deliberately using the reset pin.
  * Basically we need to send a low signal either to pin 2 (because it is inverted) xor pin 4 (because we want the trigger input of the comparator to be lower than 1.67) to set and reset respectively.
  * Since we have a break-before-make switch, we will either:
    * Connect pin 2 to ground (and make it 0V instead of the previous \\(V_{cc}\\)), which sets S = 1
    * Break (neither pin 2 nor 4 are connected to ground): hold state
      * Pin 2 has \\(V_{cc}\\) and therefore S = 0
      * Reset pin is an open circuit and therefore doesn't overwrite the top comparator, who always outputs low, so R = 0
    * Connect pin 4 to ground and set R = 1
* The \\(1k\Omega\\) resistor on pin 4 is to connect 5V to pin 4 as recommended by the 555 datasheet and we use a resistor instead of just a piece of wire as pin 4 is also connected to ground and we do not want a short circuit.
* Not included in the schematic is the \\(.01\mu F\\) capacitor from ground to pin 5

#### Tinkercad
* Tinkercad of the [original schematic]({{ site.url }}/hardware/2021/06/26/8-bit-computer.html#schematic-2)
![404]({{ site.url }}/images/8bit/clock/clock8.PNG)
[Open tinkercad](https://www.tinkercad.com/things/j2aXkFz1LPm-555-timer-p7)

* Ben replica
![404]({{ site.url }}/images/8bit/clock/clock9.PNG)
[Open tinkercad](https://www.tinkercad.com/things/lFTIiefIzFf-555-timer-p8)