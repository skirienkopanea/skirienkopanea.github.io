---
layout: post
title:  "Engineering Circuits Analysis (DC)"
date:   2021-06-26 00:51:00 +0200
categories: hardware
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
    - [Kirchhoff's Voltage Law (KVL)](#kirchhoffs-voltage-law-kvl)
    - [Dependent Current Sources](#dependent-current-sources)
  - [Measuring the circuit](#measuring-the-circuit)
    - [Node voltage method](#node-voltage-method)
      - [Dependent sources and supernodes](#dependent-sources-and-supernodes)
    - [Mesh current method](#mesh-current-method)
      - [Current source, Supermesh and dependent power source](#current-source-supermesh-and-dependent-power-source)
  - [Equivalent Circuits](#equivalent-circuits)
    - [Resistors in series and Parallel](#resistors-in-series-and-parallel)
    - [Source transformations](#source-transformations)
    - [Currents in parallel](#currents-in-parallel)
    - [Power sources in series](#power-sources-in-series)
    - [Thevenin Equivalent Circuits](#thevenin-equivalent-circuits)
    - [Norton Equivalent Circuits](#norton-equivalent-circuits)
    - [Voltage divider circuits](#voltage-divider-circuits)
    - [Current Divider Circuits](#current-divider-circuits)
    - [Maximum Power transfer](#maximum-power-transfer)
    - [Superposition](#superposition)
  - [Capacitors and Inductors (Vol 5 and 4)](#capacitors-and-inductors-vol-5-and-4)
  - [Op-Amp Circuits (Vol 6)](#op-amp-circuits-vol-6)

## Introduction
### Voltage, Current, Resistance
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
* Alternating current (AC): i.e. wall socket, the current moves back and forth.
* Open circuit: a broken circuit and therefore there is no more current flow.
* Short circuit: circuit that (accidentally) gets shortened and creates a path of small resistance back to the source, which generally skips the intended meaning of the circuit and generates a lot of heat and cause a fire.
  * Circuit breakers at home can detect increasing current consumption and shut its down it to avoid shortcircuits

### Components: Resistor, Capacitor, Inductor, Transistor, Diode, Transformer
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

### Ohm's Law
* V = IR
  * voltage = current * resistance
  * Also \\(V_n = I_n R_n\\) where n is the component and we are measuring with the multimeter directly at each of the component terminals
* The voltage drop of all the the circuit parts (calculated at each part with Ohm's law) has to eventually add up to equal the source voltage.
* Ohms law allows us to see and make branches with different voltages (for components with different specs)
* passive components: are those that are not a source of power
  * They have a voltage drop
  * voltage drop actually means the difference between the positive and the negative terminals

### Power calculations
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

Example of sign convention of the voltage drop (ignore the signs of i in the picture (typo), what we change is the drop sign, if the cuurrent goes from - to + then the voltage drop is negative (power source) if it goes from + to - it's possitive (passive component):
![404]({{ site.url }}/images/8bit/power.PNG)

### Kirchhoff's Current Law (KCL)
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

### Kirchhoff's Voltage Law (KVL)
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

### Dependent Current Sources

![404]({{ site.url }}/images/8bit/dcs.PNG)

* Any arrow symbol represents a current source.
  * When it's within a circle its a source that adjusts its voltage in response to the resistance in the circuit such that the emitted current is constant.
  * When it's within a diamond it is a dependent current source. That is the final amount flowing from the component is not fixed, and most importantly that depends on the behaviour of somewhere else in the circuit (i.e. a transistor triggering things).
* Independent circuits that are located close to each other and that have the same big picture type of goal are commonly connected to the same negative terminal to ensure that base voltage (difference between + and -) is consistent among all the circuits. Otherwise you might get a circuit with a base voltage of +500 and +600 vs other one with 0 and +100 located close to each other which can cause many problems. The negative terminal also regarded as ground, therefore the practice of connecting all the negative terminals to the same negative voltage source is called "grounding".

![404]({{ site.url }}/images/8bit/dcs2.PNG)

* The voltage drop for each of the parallel branches connected to the same source is the same.
* If you get a negative current it means that the direction that you've drawn in your circuit is flipped.
  * But you dont know to change the orientation of the drawing. Once you've set the drawings you can stick to the values that you have and keep going to solve for whatever you need to solve.

## Measuring the circuit
### Node voltage method
* It takes less equations than Kirchhoff's laws and it's more strict.
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

* The main takeaway from the NV is that
  * We write a modified KCL where we pretend that the all curents at the node are leaving and their sum equals 0
  * if parallel branches (where the positive and negative terminals are connected directly to the source without resistance) have the same voltage, in the event that the terminals experience a resistance between the parallel branches, we can calculate the voltage drop from the resistance by substracting the voltages of branch A and branch B (then divide that by the resistance itself to get i (I=V/R)).
    * In NV context the positive voltage is the one of the node we are writting the equation.

#### Dependent sources and supernodes
* They are labled with a romboid: <+ ->
* A source (independent or dependent) connected to two essential nodes will reduce the number of available NV equations (because generally it's gonna be hard to find out the current of that leg if we're just given the voltage).
  * you'll need to introduce a variable \\(i_k\\) into the NV equation and reuse that \\(i_k\\) for the other node of the source and then cancel them out.
  * Or you can apply the "supernode" technique which just merges the nodes. KCL, KVL, NVL also apply two a selection of n nodes, "what comes inside the area equals what comes outside the area"
    * Then apply NV to the merged node
![404]({{ site.url }}/images/8bit/sn.PNG)
  * Since we lost 1 equation, we need to supplement it with something else, such as a KVL or KCL.
  * Another not so obvious example:
![404]({{ site.url }}/images/8bit/sn2.PNG)

### Mesh current method
* It's a modified KVL much like NV is a modified KCL

Steps:
1. Identify the meshes (polygons, squares, triangles, etc)
   * The number of equations you need is equal to the number of meshes you have. 
2. Label the \\(i_n\\) of each mesh
3. Write the mesh equations wiht the "net currents" that is, whenever an edge has two labled \\(i_n\\)'s, use the difference (and have the positive i to be the one of the given mesh equation)
![404]({{ site.url }}/images/8bit/mcm.PNG)
4. Solve the system of equations
5. With the mesh currents you can find the real currents.
   * You have to pay attention to the circuit, the circuit currents are either equal to the mesh currents (when they are not "fighting" with another current) or to the difference between two mesh currents (those that overlap a leg).
* The take away of MC and NV is that they are direct applications of Kirchhoff's law with a very specific algorithm to solve the problems, which generally reduces the amount of equations and sets a clear action plan to solve a problem.

#### Current source, Supermesh and dependent power source
* Like in a supernode you can experience tricky problems with powersources whose current (and resistance) is unkown, here you can experience problems with current sources whose voltage (and resistance) is unkown.
* Any time you see a current source that travels two meshes you'll either have to come up with a dummy voltage variable (that will be cancelled out) or draw a supermesh around the problem area.
![404]({{ site.url }}/images/8bit/sm.PNG)
  * Either approach will require an additional constraint equation to solve the system, you can use the fact that the difference between \\(i_a\\) and \\(i_b\\) has got to be the amps of the (a priori problematic) current source
* A dependendent powersource will introduce an additional variable onto the system of equations. It can be circumvented by finding another constraint equation by means of for instance KCL involving said variable.

## Equivalent Circuits
### Resistors in series and Parallel
* In series is also called "daisy chaining"
  * The current that flows through them remains the same. In a DC circuit the current can only change if it branches.
  * The resistance of the 3 resistors is equivalent to a resistor with a resistance of their sum.
  * The powersource might be in between the resistors but that's still in series.
![404]({{ site.url }}/images/8bit/series.PNG)
* Parallel resistors
  * The current branches

![404]({{ site.url }}/images/8bit/parallel.PNG)

* For 2 parallel resistors you can use:
  * $$R_{eq}=\frac{R_1R_2}{R_1+R_2}$$

* These can be used to simplify circuits which will limit the complexity of the system of equations that we need to solve
* If there is a branch with 0 resistance 100% of the current is going to go towards that branch, leaving the other branches of the node unvisited.

### Source transformations
* Circuit simplification of the current and voltage sources.
  * You can rewrire a resistor in series with a power source into a circuit where the the resistor is on parallel with a current source
![404]({{ site.url }}/images/8bit/st.PNG)
  * What we need to find is the value of the current source with Ohm's law
    * $$i_s=\frac{v_s}{R}$$
* If a current source has a resistor in series, it can be ignored as the current entering the resistor is the same one leaving it, which goes unnoticed to the terminals a and b
* If a power source has a resistor in parallel, it doesnt matter to a and b either as the voltage remains the same
* The polarity of the power source has to match the direction of the current source.

### Currents in parallel
* It's the algebraic sum of them (algebraic means that it's sensetive to signs instead of just summing absolute values).

![404]({{ site.url }}/images/8bit/cip.PNG)

* Currents in series dont make sense because the value of a current source is supposed to hold for the entire leg.

### Power sources in series
* They're equivalent to a power source of the algebraic sum of them (if they are of opposite polarity, it's gonna be the difference of them and it'll keep the polarity of the bigger one)
* Power source in series should have the same voltage (KVL) if not the higher voltage source would be discharged onto the lower voltage source until both have the same votlage and/or will fuck things up.

### Thevenin Equivalent Circuits
* Lets you model the entire circuit as a voltage source and a resistance.

![404]({{ site.url }}/images/8bit/tec.PNG)

* Any network of resistors and sources can always be rewritten in terms of a single voltage source and a single resistor.
  * This also applies with inductors and capacitors in AC

Steps:
1. Prerequisite: identify two terminals (a and b) from which the circuit is connected to the outside world.
   * Different choice of terminals will likely result in different Thevenin Equivalent circuits.
2. \\(V_{th}\\): Find open circuit voltage between a and b (load free voltage)
3. \\(R_{th}\\): Find the short circuit current between a and b (called \\(i_{sc}\\)). Then \\(R_{th}=\frac{V_{th}}{i_{sc}}\\).

* You'll find 2 and 3 probably by using Node Voltage or Mesh Voltage methods.

Examle: 
![404]({{ site.url }}/images/8bit/tec2.PNG)
* \\(\text{MC1: } -72 + (i_1-i_2)5 + i_120 = 0\\)
* \\(\text{MC2: } (i_2-i_1)5 + i_212 + i_28 = 0\\)
* Solve for \\(i_1\\) and \\(i_2\\) and you get 3 and 0.6 respectively.
* \\(V_{th}\\) (in the context of this circuit) = \\(V_{8\Omega}+V_{20\Omega}\\).
  * = 0.6 * 8 + 3 * 20 = 64.8
* \\(i_{sc}\\) (in the context of this circuit) can be found by doing the mesh current process all over again with the updated circuit with the shortcircuit:
  * This would yield 12.72, 6 and 10.8
* \\(R_{th}=\frac{V_{th}}{i_{sc}}=\frac{64.8}{10.8}=6\\)

![404]({{ site.url }}/images/8bit/tec3.PNG)

### Norton Equivalent Circuits
* Transforming a Thevenin equivalent to a current source with a resistor in parallel

### Voltage divider circuits
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

### Current Divider Circuits
* This applies to circuits that have a fixed current source.
 * You have to use the opposite resistance as numerator.
![404]({{ site.url }}/images/8bit/cdc.PNG)

### Maximum Power transfer
* We can model the load that we want as a resistor

![404]({{ site.url }}/images/8bit/pt.PNG)

* We are looking for which value of the load resistance results in maximum power transfer from the source to the load.
  * When resistance is 0, we have \\(P=I^2R=I\cdot 0=0\\)
  * When resistance is "a very large countable integer" we can say that I approximates 0 and without giving a heart attack to mathematicians infere that \\(P=I^2R=0\cdot R=0\\).
  * The maximum power happens when \\(R_{load} = R_{th}\\).
    * You can manually check this by writting a function that returns the power delivered to the load as a function of resistance and set the derivative equal to 0.
    * It kinda looks like a voltage divider too, in which we can see that when the value of the two the resistors are equal, then the voltage of the source is split evenly among the two resistors.
* When maximum power is being delivered whe know that \\(R_{load} = R_{th}\\)
![404]({{ site.url }}/images/8bit/mpt.PNG)
  * $$P_{max}=\frac{V^2_{th}}{4R_{th}}$$

### Superposition
* The circuit behaves as a superposition of all the sources together (that is, the sum of all individual contributions of the sources to the circuit)
  * What you do is, for each of the sources, calculate the voltage/resistance/current/etc of the element(s) that need to be analyzed as if only that source existed in the circuit, then add all the values of each source to find the final value.
  * This is possible due to the "linear" nature of the circuits we're dealing with.
  * It's a great method for troubleshooting problems with sources.
  * It is also appropiate when you need to simplify the circuit, the cost is that you'll have to repeat the process for each source

## Capacitors and Inductors (Vol 5 and 4)

## Op-Amp Circuits (Vol 6)