---
layout: post
title:  "Engineering Circuits basics"
date:   2021-06-26 00:51:00 +0200
categories: architecture
tags: project
---
{% include math.html %}
<!--more-->

# Table of Contents
- [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
  - [Voltage, Current, Resistance](#voltage-current-resistance)
  - [Components: Resistor, Capacitor, Inductor, Transistor, Diode, Transformer](#components-resistor-capacitor-inductor-transistor-diode-transformer)
  - [Ohm's Law](#ohms-law)
  - [Power calculations](#power-calculations)
  - [Kirchhoff's Current Law](#kirchhoffs-current-law)

## Introduction
I'm following this video [https://www.youtube.com/watch?v=OGa_b26eK2c](https://www.youtube.com/watch?v=OGa_b26eK2c)

## Voltage, Current, Resistance
* Circuit: Closed loop that carries electricity
* Current (real life current): flow of electrons in a circuit (the negative charge is the one moving towards the positive source)
  * hole current (engineering current "i"): Mathematically it is equivalent to assume that it's the positive side charge the ones that is moving from the positive source to the negative source, and it makes the equations simpler as opposed to having to deal with negative signs, so we use the positive charge instead.
  * Unit: Ampere (Amp, A): how many charges are moving through the circuit per unit of time.
  * Analogy: if you blow a straw, the air is the current, it is what is moving through the straw
* Voltage: The "push" that causes the current to flow. It's the source. It's very related to the current. You cannot have any current without having something pushing it.
  * Analogy: The voltage is the push pressure from the blow. A high voltage as a high potential to push a lot of current, but the push itself is not what kills you, what would kill you is the current.
* Resistance: Opposes current flow in a circuit. It makes it harder to blow the same amount of air. The smaller the object the higher the resistance.
  * Unit: Ohm, \\(\Omega\\)
  * Resistors in sequence are equivalent to 1 resistor with the resistance combined
* Direct current (DC): i.e. batteries, its source gives a constant voltage (which causes constant current)
* Alternating current (AC): i.e. wall sucket, the current moves back and forth.
* Open circuit: a broken circuit and therefore there is no more current flow.
* Short circuit: circuit that (accidentally) gets shortened and creates a path of small resistance back to the source, which generally skips the intended meaning of the circuit and generates a lot of heat and cause a fire.
  * Circuit breakers at home can detect increasing current consumption and shut its down it to avoid shortcircuits

## Components: Resistor, Capacitor, Inductor, Transistor, Diode, Transformer
* Source voltage: (+ -) generic, or a battery (bunch of horizontal lines), or (~) for AC
  * short horizontal line is the negative terminal
  * long side is the positive terminal
  * base unit is Volts
* Resistor (zig zag): limits how much current there is on a branch.
  * Base unit is Ohms
* Capacitor (-\|\|-): device that stores electric charge
  * You pile it up and at a later time it lets you bleed it out to feed the circuit
  * Base unit: Farad (F) measures how much energy in the form of electric field can be stored
* Inductor (like a spring) concentrates magnetic field 
  * Unit is Henry (H): how much energy in the form of magnetic field can be stored
* Diode ( -\|>\|-): only allows direct current to flow in one direction
* LED: Light Emitting Diode (same symbol but with some arrows representing the light)
* Transistors (3 wires symbol)
  * It can act as a switch
  * It can act as an amplifier
* Transformer: allows a voltage to change its value
  * Useful to transform the wall socket voltage to a smaller voltage for light home appliances

![Breadboard]({{ site.url }}/images/8bit/circuitcheatsheet.PNG)
Source [https://learn.sparkfun.com/tutorials/how-to-read-a-schematic/all](https://learn.sparkfun.com/tutorials/how-to-read-a-schematic/all)

## Ohm's Law
* V = IR
  * voltage = current * resistance
  * Also \\(V_n = I_n R_n\\) where n is the component and we are measuring with the multimeter directly at each of the component terminals
* The voltage drop of all the the circuit parts (calculated at each part with Ohm's law) has to eventually add up to equal the source voltage.
* Ohms law allows us to see and make branches with different voltages (for components with different specs)
* passive components: are those that are not a source of power
  * They have a voltage drop
  * voltage drop actually means the difference between the positive and the negative terminals

## Power calculations
* Unit of power is a Watt
  * Watt It's energy (i.e. joules) per unit of time (seconds) = joule/sec
  * So it's about something (energy) happening continously, i.e. "usage"
* p = iv
  * the more power the hotter the component gets
  * negative power means that the component is actually delivering power to the circuit
* signs of i
  * if the current (i) is flowing from the positive terminal (side that its connected to the positive side of the source) to the negative terminal of the component, then i is positive at the component, else is negative (same for voltage)
  * if i is negative then the power is negative, which means that the component is a source
  * when the voltage comes directly from the source is the same as the source
  * The power consumed by all passive components has to be equal to the power generated by all sources
* The power, measured in watts, if too high it will make the component burn
* From Om's law (V=IR) we also have that (for passive components because it's always positive)
  * p = iv
  * \\(p=i^2r\\)
  * \\(p=\frac{v^2}{r}\\)

Example of i sign convention, power and om's law:
![Sign convention]({{ site.url }}/images/8bit/power.PNG)

## Kirchhoff's Current Law