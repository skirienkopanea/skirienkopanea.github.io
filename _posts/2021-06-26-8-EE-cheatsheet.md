---
layout: post
title:  "Engineering Circuits basics"
date:   2021-06-26 00:51:00 +0200
categories: hardware circuits
tags: cheatsheet
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
  - [Kirchhoff's Current Law (KCL)](#kirchhoffs-current-law-kcl)
  - [Kirchoff's Voltage Law (KVL)](#kirchoffs-voltage-law-kvl)
  - [Dependent Current Sources](#dependent-current-sources)
  - [Resistors in series and Parallel](#resistors-in-series-and-parallel)
  - [Voltage divider circuits](#voltage-divider-circuits)
  - [Current Divider Circuits](#current-divider-circuits)
  - [Node voltage method](#node-voltage-method)
  - [Dependent sources and supernodes](#dependent-sources-and-supernodes)

## Introduction
I'm following this video [https://www.youtube.com/watch?v=OGa_b26eK2c](https://www.youtube.com/watch?v=OGa_b26eK2c)

## Voltage, Current, Resistance
* Circuit: Closed loop that carries electricity
* Current (real life current): flow of electrons in a circuit (the negative charge is the one moving towards the positive source)
  * hole current (engineering current "i"): Mathematically it is equivalent to assume that it's the positive side charge the ones that is moving from the positive source to the negative source, and it makes the equations simpler as opposed to having to deal with negative signs, so we use the positive charge instead.
  * Unit: Ampere (Amp, A): how many charges are moving through the circuit per unit of time.
  * Analogy: if you blow a straw, the air is the current, it is what is moving through the straw
  * Current comming directly from the source is sometimes labled \\(i_s\\)
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
![404]({{ site.url }}/images/8bit/power.PNG)

## Kirchhoff's Current Law (KCL)
* The algebraic (sign sensitive) sum of currents at any node is zero
* node is a connection point: \_\_\_\|_\_\_
  * Electricity prefers to take the path of least resistance.
  * Sign convention, the node itself is the "center of the universe", we dont care about active/passive components such as the source.
    * If the current leaves the node, then it has positive sign
    * If the current reaches the node, then it has negative sign
  * Although usually observed at nodes that connect 3+ things, two elements are also connected by a node (even if they are on the same stright line ont sequence, there is a node between every component)
* If you have n nodes you can only write n-1 independent equations. So it is not worth to write up the last node equation as that one wont be independent (just a rewriting of existing ones) and wont help solve the problem
![404]({{ site.url }}/images/8bit/kcl.PNG)

$$-i_1+i_2+i_3=0$$

## Kirchoff's Voltage Law (KVL)
* The algebraic sum of all the voltage drops around any closed path in a circuit is zero. It doesnt regard nodes, it just regards loops.
  * KVL, although uses a route path of its own for the loops, it does take the  polarity of the actual currents (i.e. where the + and - terminals are) to determine the sign of the voltage drop in the KVL equation.
  * If the "loop path" goes from positive terminal to negative terminal, the votlage drop is positive
  * If the "current" goes from the negative terminal to the positive terminal the votlage drop is negative.
* See how R1 has a positive drop in loop 1 yet a negative drop in loop 2

![404]({{ site.url }}/images/8bit/kvl.PNG)

* Se how we can just take the outside loop too

* In a practical setting KVL is used in combination with Ohm's law, so you end up rewritting KVL in terms of current * resistance:

![404]({{ site.url }}/images/8bit/kvl2.PNG)

* In complex circuits, you'll likely exhaust the n-1 indepdendent KCL equations and you'll suplement it with as many KVL loop equations as possible to achieve n equations for n variables. Then solve the system of linear equations with a machine (i.e. matrix solver)
  * For n total loops, you can write n-1 independent equations
  * There are as many loops as arbitrary closed paths without branches you can find, i.e. some of them:

![404]({{ site.url }}/images/8bit/kvl3.PNG)

Example to solve for \\(i_1\\):

![404]({{ site.url }}/images/8bit/kvl4.PNG)

![404]({{ site.url }}/images/8bit/kvl5.PNG)

```matlab
rref(A)
```

* If you want to check the voltage difference between any two points in a circuit you are allowed to create an imaginary wire to construct a KVL loop

## Dependent Current Sources

![404]({{ site.url }}/images/8bit/dcs.PNG)

* Any arrow symbol represents a current source.
  * When it's within a circle its a source that adjusts its voltage in response to the resistance in the circuit such that the emitted current is constant.
  * When it's within a diamond it is a dependent current source. That is the final amount flowing from the component is not fixed, and most importantly that depends on the behaviour of somewhere else in the circuit (i.e. a transistor triggering things).
* Independent circuits that are located close to each other and that have the same big picture type of goal are commonly connected to the same negative terminal to ensure that base voltage (difference between + and -) is consistent among all the circuits. Otherwise you might get a circuit with a base voltage of +500 and +600 vs other one with 0 and +100 located close to each other which can cause many problems. The negative terminal also regarded as ground, therefore the practice of connecting all the negative terminals to the same negative voltage source is called "grounding".

![404]({{ site.url }}/images/8bit/dcs2.PNG)

* The voltage drop for each of the parallel branches connected to the same source is the same.
* If you get a negative current it means that the direction that you've drawn in your circuit is flipped.
  * But you dont know to change the orientation of the drawing. Once you've set the drawings you can stick to the values that you have and keep going to solve for whatever you need to solve.

## Resistors in series and Parallel
* In series is also called "daisy chaining"
  * The current that flows through them remains the same. In a DC circuit the current can only change if it branches.
  * The resistance of the 3 resistors is equivalent to a resistor with a resistance of their sum.
* Parallel resistors
  * The current branches

![404]({{ site.url }}/images/8bit/parallel.PNG)

* For 2 parallel resistors you can use:
  * $$R_{eq}=\frac{R_1R_2}{R_1+R_2}$$

* These can be used to simplify circuits which will limit the complexity of the system of equations that we need to solve
* If there is a branch with 0 resistance 100% of the current is going to go towards that branch, leaving the other branches of the node unvisited.

## Voltage divider circuits
* You can use a voltage divider to use a 12V source to supply a 5V circuit
  * It doesnt matter (only for the current) the size of the resistance to divide the volts, what matters is the ratio of the two resistors, that's what determines how much voltage will go to each part.

![404]({{ site.url }}/images/8bit/vd.PNG)

* The 2V volts calculated are the "no load" voltage.
  * Once we connect the load we are actually adding a parallel resistance with the 2 oms resistors.
    * If the load is infinity (i.e. open circuit) it's as if we didnt breanch out anything
      * That's how multieters work, by having a very large internal resistance which makes the branches of the multimeter to not interfere with the circuit.
    * If the load is 0, we are creating a short circuit that skips the 2 oms resistor, so we end up with 0V and 10V in the first resistor.
    * The equivalent resistance can be calculated with the 2 resistors parallel equivalence \\(R_{eq}=\frac{R_1R_2}{R_1+R_2}\\)
    * The (no load) voltage going through that circuit can be calculated with (which is basically the same as just the voltage drop of the resistor. However we calculate without KVL or Oms law, that is, without needing to find the current and just by using the source voltage and resistances):
![404]({{ site.url }}/images/8bit/vd2.PNG)
* To calculate the actual voltage going through the loaded circuit first apply the equivalence resitance and then the voltage formula above.
* The bigger \\(R_2\\) the closer the no load voltage it gets to the source voltage
* The smaller \\(R_2\\) the closer the no load voltage goes to 0 (short circuit).

## Current Divider Circuits
* This applies to circuits that have a fixed current source.
 * You have to use the opposite resistance as numerator.
![404]({{ site.url }}/images/8bit/cdc.PNG)

## Node voltage method
* It takes less equations than Kirchoff's laws and it's more strict.
* It's called voltage but it's just the "currents" sum equal to zero
* A node is any interconnection of two or more components
* Essential node are those that have 3 or more components (the supply is a component)
* A node that connects just 2 components has the same current flowing through them so they are not relevant for the node voltage method.
* The node voltage method consists of 4 steps:
  * Identify the essential nodes.
  * Choose a reference node
    * most of the times is the node with most connections and at the bottom of the drawing.
    * When you are measuring something in a circuit with the multimieter you're basically checking the difference in V or A or R between the black lead and the read lead.
  * Write node voltage equations in reference to the reference node.
  * For n number of essential nodes you need n-1 node voltage equations to completely discribe the circuit
* Once the node voltages are found, it's very easy to find everything else.

Example:
![404]({{ site.url }}/images/8bit/nvm.PNG)
1. Identify the essential nodes (we have 3 so we need 2 equations)
   * Just because the bottom node is spread out it is still one node that connects several components at once
2. Pick a reference node (bottom one has the most interconnections)
   * The reference node is symbolized with a triangle that represents commonality or reference.
3. The node voltage equations that we will find are for the two top (essential) nodes with respect to the reference node
![404]({{ site.url }}/images/8bit/nvm2.PNG)
   * Se that the minus signs are the reference node such that the voltages \\(v_1\\) and \\(v_2\\) (for nodes 1 and 2 respectively) are measured in reference to the reference node.
   * When you are writing the node voltage equations of an essential node you ignore the current direction of i and pretend that all i are leaving the node. (the sign of the answer will take care of everything).
    * prepararation for \\(\text{NV @ Node 1:}\\)
       * \\(v_{1\Omega}=v_1-v_s\\) (it's kinda triangle KVL with source, node 1 and reference node as points of the loop, the difference is )
       * \\(i_{1\Omega}=\frac{v_{1\Omega}}{1\Omega}=\frac{v_1-v_s}{1}\\)
       * \\(i_{5\Omega}=\frac{v_1}{5}\\)
       * \\(i_{2\Omega}=\frac{v_1-v_2}{2}\\)
     * $$\text{NV @ Node 1:}\ i_{1\Omega} + i_{5\Omega} + i_{3\Omega} = 0,\frac{v_1-10}{1} + \frac{v_1}{5} + \frac{v_1-v_2}{2} = 0$$
     * $$\text{NV @ Node 2:}\ \frac{v_2-v_1}{2} + \frac{v_2}{10} - 3 = 0$$
4. Solve for \\(v_1\\) and \\(v_2\\).

## Dependent sources and supernodes
* They are labled with a romboid: <+ ->
* A source (independent or dependent) connected to two essential nodes will reduce the number of available NV equations (because generally it's gonna be hard to find out the current of that leg).
  * you'll need to introduce a variable i into the NV equation and reuse that i for the other node of the source and then cancel them out.
  * Or you can apply the "supernode" technique which just merges the nodes. KCL, KVL, NVL also apply two a selection n nodes, "what comes inside the area equals what comes outside the area"
    * Then apply NV to the merged node
  * Since we lost 1 equation, we need to supplement it with something else, such as a KVL or KCL.
![404]({{ site.url }}/images/8bit/sn.PNG)
This is also allowed:
![404]({{ site.url }}/images/8bit/sn2.PNG)