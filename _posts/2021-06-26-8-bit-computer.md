---
layout: post
title:  "Ben Eater 8-bit computer project"
date:   2021-06-26 00:51:00 +0200
categories: hardware architecture
tags: project
---
{% include math.html %}
<!--more-->

# Table of Contents
- [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
  - [Clock module](#clock-module)
    - [Power supply](#power-supply)
    - [Basic circuit](#basic-circuit)
      - [Sample closed circuit](#sample-closed-circuit)
    - [Common mistakes](#common-mistakes)
    - [Astable 555 timer](#astable-555-timer)

## Introduction
Ben Eater is an online educator on computer-related topics from which I'm following his 8-bit computer project. [https://eater.net/8bit](https://eater.net/8bit). The computer is composed of different modules, which are built on breadboards. The modules are the clock module, registers and ALU (arithmetic and logic unit) module, RAM (random access memory) and program counter module, and output and control logic module.

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
* We are using the 555 timer IC (integrated circuit (chip))
* Our clock is adjustable-speed (from less than 1Hz to a few hundred Hz).
![Hertz]({{ site.url }}/images/8bit/1hz.jpg)
   *  One Hertz (Hz) is defined as one cycle per second
* The clock can also be put into a manual mode where you push a button to advance each clock cycle. (Useful for debugging)


Source: [https://eater.net/8bit/clock](https://eater.net/8bit/clock)

### Power supply
![Hertz 404]({{ site.url }}/images/8bit/clock/intro.PNG)
We will be using a 5V (volt) power source, which Ben has crafted by cutting off the wires of a cellphone charger.
* Voltage is like the "pump" force to push electrones through a circuit.
* Increasing the voltage will also increase the current, but they are different measurements of different things.
* Current (measured in Amperes/amps/A) is the amount of "electric charge" flowing at a given point.
* Power is defined as energy per unit of time, (rate at which energy is being transfered). Power (P) is also:
  * \\(P = VI\\)
  * Where V is volts (i.e. "pump force") and I is amps ("i.e. quantity").
* Safety rules before using anything related to electricity:
  * Dont plug a lot of stuff into a single extension cord
  * Keep eletrcical things far away from water
  * Never connect LEDs directly to  5 volt source without a current limiting resistor or you will damage the LED.
  * Most 74LSxx logic chips have internal resistors that let you connect LEDS directly to the outputs without a resistor. However, the 555 timer, 74189 RAM, 28C16 EEPROM and 74HC595 do not.
  * For most reliable results you may want to connect a \\(220\Omega\\) resistor in series with every LED anyway. 
    * The ohm  \\(\Omega\\) is defined as an electrical resistance between two points of a conductor when a constant potential difference of one volt, applied to these points, produces in the conductor a current of one ampere.
    * \\(\Omega=V/A\\)

### Basic circuit
![Breadboard]({{ site.url }}/images/8bit/clock/breadboard.PNG)
* The breadboard is composed of metalical strips that can be linked together with jump wires and electrical components.
* The A-J holes constitute columns (each) and the numbers constitute rows.
  * ![Breadboard]({{ site.url }}/images/8bit/clock/bb1.PNG)
  * A1, B1, C1, D1 and E1 are all connected, and not connected to any other wholes in the breadboard.
  * The individual power buses are not connected to each other (only the column itself).
* The + - vertical strips are called buses or rails and are used to deliver power.
  * The red (+) line will connect to the positive side of the battery
  * The black or blue (-) line will connect to the negative side of the battery
  * The busline may not connect the entire length of the board, unbroken lines show that the bus indeed connects the entire length
* The middle space of the breadboard is designed to fit the integrated circuits (IC) (chips) that come with dual in-line package (DIP), meaning they come in two columns of pins of which each individual pin is connected to its own row.

#### Sample closed circuit
![Breadboard]({{ site.url }}/images/8bit/clock/bb2.PNG)
* It doesnt matter whether you put the resistor in series with the led before or after, as in both cases it will drop the "intensity" (i'm still not familiar with the exact thing that gets decreased, at least i know it won't fry it).
* The long leg is the positive side of the LED and it's called anode, and the shorter leg is for the negative side and it's called cathode.
  * Plugging it backwards wont break the led, but it will block the current flow of the circuit.

### Common mistakes
* Setting the LD legs in different rows
* Lose wires not pushed dep into the wholes
* Disrespecting the polarity of the components
  * LEDs have polarity: long leg +, short leg -
  * Resistors don't have polarity
* Jumper wires:
  * Long flexible wires are convinient for simple circuits but will spagetti-like mess up complex circuits
  * Pre-cut jump wires have predefined lengths which limit customization
  * hookup wire and wire strippers to make your own jumper wires:
    * Make sure to buy solid-core wire and not stranded wire as the latter is made up of multiple individual strands which makes the ends much harder to push into a breadboard
    * As a last resort stranded wired if cannot be solded can be twisted to increase its tightness.
    * To leave enough space for bending the wires and plug them into the board leave a room of 3 more wholes.
* Integrated circuits:
  * Not connecting an integrated circuit in the middle of the board as that will connect both ends of the IC (making a shortircuit)

### Astable 555 timer


