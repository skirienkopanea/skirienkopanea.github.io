---
layout: post
title:  "Probability Theory and Statistics Summary"
date:   2021-04-19 00:51:00 +0200
categories: math
tags: CSE1210
---
{% include math.html %}
<!--more-->

# Table of Contents
- [Table of Contents](#table-of-contents)
  - [Terminology](#terminology)
    - [Probability vs Statistics](#probability-vs-statistics)
  - [Numerical summaries](#numerical-summaries)
  - [Five-number summary](#five-number-summary)
  - [Graphical summaries](#graphical-summaries)
  - [Set theory](#set-theory)
    - [Tricks](#tricks)
    - [DeMorgan's Laws](#demorgans-laws)
  - [Probability](#probability)
    - [Sample spaces](#sample-spaces)
    - [Probability function for finite sample spaces](#probability-function-for-finite-sample-spaces)
      - [Probability function for 1 success after n trials](#probability-function-for-1-success-after-n-trials)
    - [Additivity property](#additivity-property)
    - [Product property](#product-property)
    - [Probability function for infinite sample spaces](#probability-function-for-infinite-sample-spaces)
  - [Conditional probability and independence](#conditional-probability-and-independence)
    - [Conditional probability](#conditional-probability)
    - [Multiplication rule](#multiplication-rule)
      - [Birthday example:](#birthday-example)
    - [Bayes rule (Librarian vs farmer)](#bayes-rule-librarian-vs-farmer)
      - [Law of total probability](#law-of-total-probability)

## Terminology
* stochastic: having a random probability distribution or pattern that may be analysed statistically but may not be predicted precisely.
* An experiment is an activity or procedure that produces distinct (single) outcomes.
  * An event is a set that describes 1 or more outcomes. I.e. throw an odd dice number, throw number 3, etc. It is a subset of the sample space.
  * The set of all outcomes is the *sample space* \\(\Omega\\)
  * \\(Outcome \in Event \in \Omega\\)

### Probability vs Statistics
* **Probability**: Given a explicitly defined model, predict the outcome (accurate).
  * Define the probability of events and their distribution, frequency, relation.
* **Statistics**: Given the observed data, fit a model (estimation).
  * Describe the *observed* data (descriptive statistics)
  * Estimate parameters
  * Test hypotheses

## Numerical summaries
* Average (also known as mean)
* Median (middle number/interval of the distinct set, not necessarily the same as the average)
* Minimum and maximum value (range)
* Lower and upper quartile

## Five-number summary
* Minimum
* Lower quartile (median of the set from the minimum to the median)
* Median
* Upper quartile (median of the set from the median to the maximum)
* Maximum

## Graphical summaries
* Histogram
  * Displays data in intervals, which are determined by a "bin" width
  * Bin width is mostly arbitrary, but there are "sweet spots" that show more useful information than others

![histogram]({{ site.url }}/images/histogram_width.PNG)

* Two peaks indicate that there are likely two different types of eruption
* Scatter plots show tupple observations, often the x-axis is inteded to represent the independent variable (input, *cause*) and the y-axis the dependent variable (output).

![scatterplot]({{ site.url }}/images/scatterplot.PNG)

* However scatter plots can be deceiving. The two scatter plots look similar a priory. But if we use boxplots we can easily see the difference.

![boxplot]({{ site.url }}/images/boxplot.PNG)

* A boxplot is a visual simplified representation of the data distribution (rotated 90 degrees) 

![boxplot]({{ site.url }}/images/boxplot2.PNG)

* IQR stands for interquartile range, which is the distance between the upper and lower quartiles. This is the range of the middle half of the dataset.

## Set theory
* A set is a collection of objects
  * The members of the sets are called elements
  * \\(Tesla \in Car\ Manufacturers\\)
    * Tesla is an element of Car Manufacturers (Tesla is in the Car manufacturers set)
* Empty set: it has no elements in it \\(\emptyset\\)
* \\(\\{Tesla,SpaceX\\} \subset \\{Elon\ Musk\ companies\\}\\)
  * All subsets in this course are improper, so \\(A\subset A\\) is true for any set
  * When A and B are considered **events**, \\(A\subset B\\) is also read as A implies B
* Intersecttion: \\(\cap\\)
  * Two sets whose intersection is the empty set are called **disjoint (or mutually exclusive)**
* Union: \\(\cup\\)
* Universe set U: The set that contains evertyhing in the context of the problem
* Complement \\(^c\\) i.e if the set of all "computer" colors (universe set) is {R,G,B}, then \\(\\{R\\}^c=\\{G,B\\}\\)
* \\(U^c=\emptyset\\)
* Difference: \\(A\setminus B=A\cap B^c\\)

### Tricks
* \\(A=A\cap(B\cup B^c)=(A\cap B)\cup(A\cap B^c)\\)
  * \\(P(A)=P(A\cap B) + P(A\cap B^c)\\)
* \\((A\cup B)\cap B=B\\)
* \\((A\cup B)\cap B^c=A\cap B^c\\)
  * \\(((A\cup B)\cap B)\cup((A\cup B)\cap B^c)=B \cup A\cap B^c\\)
  * \\((A\cup B)\cap (B\cup B^c)=B \cup A\cap B^c\\)
  * \\((A\cup B)=B \cup A\cap B^c\\)
  * \\(P(A\cup B)=P(B) + P(A\cap B^c)\\)
* \\(P(A\cup B)-P(B)=P(A\cap B^c)\\)
* \\(P(A)-P(A\cap B)=P(A\cap B^c)\\)
  * \\(P(A\cup B)-P(B)=P(A)-P(A\cap B)\\)
  * \\(P(A\cup B)=P(A)+P(B)-P(A\cap B)\\)

* \\(P(A\cup A^c)=1\\)
* \\(P(A^c)=1-P(A)\\)

### DeMorgan's Laws
* The complement is the same as the negation, therefore
  * \\(\overline{A\cup B}=A^c \cap B^c\\)
  * \\(\overline{A\cap B}=A^c \cup B^c\\)

## Probability
* **Random** or unpredictable phenomena can be **modelled** (or better say, let a model be the platonic representation of a random  event with defined probabilities), such that this phenomena is regarded as **outcomes** of some **experiment** (not in the scientific method sence, but in the trial colloquial sense).
  * All the outcomes are elements of a **sample space** \\(\Omega\\)
  * Subsets of \\(\Omega\\) are called **events**
    * events have an assigned **probability** (between 0.0 and 1.0)

### Sample spaces
Coin flip: \\(\Omega = \\{Head,Tail\\}\\), has a size of 2 elements
* The size of a sample space that regards all the different possible sortings of a set of elements (i.e. row orders in an augmented matrix) is called a permutation (of n objects) and the total number of possible permutations is n! (factorial), because as we add the nth object, then that nth object can be placed in any of the n-1 permutations, in n different positions, that is, \\(n! = (n-1)\cdot n\\).
* We define \\(0!=1\\)
* **Events** are defined as elements of the **experiment outcome** set (sample space and Universe set in this context).
* Thes events can be combined according to set theory rules.
* **The empty set** is also regarded as "the impossible event".

### Probability function for finite sample spaces
* By defintion, each event must have a probability. An event whose probability is 0 means that it is not in the sample space (not a possible experiment outcome).
* A probability function P on a finite sample space \\(\Omega\\) assigns to each event A in \\(\Omega\\) a number P(A) in [0,1] such that \\(P(\Omega)=1\\) and P(A or B) = P(B) + P(A) iff A and B are disjoint.
  * Additivity of P implies that the probability of an event is obtained
by summing the probabilities of the outcomes belonging to the event.
* for the probability of a head **outcome** we cant write P(Head) as Head is technically just an element of a set, but not a set itself. Only events are assigned probabilities, therefore we use the event with a single outcome {Head} to denote that P({Head})=0.5
* But in practice it's okay to drop the brackets for single element sets
* For simplicity, we also let all months have equal probability 1/12 regardless of the fact that some are longer than others.

#### Probability function for 1 success after n trials
* It is the product of all the failures (1-p)^n and the single success p such that:

$$P(n)=(1-p)^np$$

### Additivity property
* \\(P(A\cap B)=P(A)+P(B)-P(A\cup B)\\)
* \\(P(A\cup B)=P(A)+P(B)-P(A\cap B)\\)
  * \\(P(A)=P(A\cap B) + P(A\cap B^c)\\)
  * \\(P(A\cup B)=P(B) + P(A\cap B^c)=P(A)+P(B \cap A^c)\\)
* \\(P(A\cup A^c)=1\\)
* \\(P(A^c)=1-P(A)\\)

### Product property
* The sample space of two successive coin flips is the cross product of \\(\Omega=\\{H,T\\}\times\\{H,T\\}=\\{(H,H),(H,T),(T,H),(T,T)\\}\\)
* if we consider two experiments with sample spaces \\(\Omega_1\\) and \\(\Omega_2\\) then the combined experiment has as its sample space the set \\(\Omega=\Omega_1 \times \Omega_2\\)
* The probability of (H,H) would be P(H) * P(H) for {H,T} as long as we agree that each successive throw is independent from the other and does not influence each other in any way.

### Probability function for infinite sample spaces
* It is the same only that \\(P(A_1 \cup A_2 \cup \dots) = P(A_1) + P(A_2) + \dots\\) with A being disjoint events.
* \\(P(\Omega)\\) is the sum of all the individual probabilities of 1 success after n trials, which should be 1, and it's proven with a calculus geometric series (like \\(\sum{\left(\frac{1}{2}\right)^n}=1\\), in binary 0.11111111...) such that:

$$P(\Omega)=\sum^\infty_{n=1}{\left((1-p)^{n-1}\cdot p\right)}=\frac{p}{1-p}\cdot\sum^\infty_{n=1}{\left((1-p)^{n}\right)}$$

* For \\(\sum^\infty_{n=1}{\left((1-p)^{n}\right)}\\) we have r = (1-p), therefore the sum equals \\(a_1\cdot\frac{1}{1-r}=\frac{1-p}{1-(1-p)}=\frac{1-p}{p}\\)

* $$P(\Omega)=\frac{p}{1-p}\cdot\frac{1-p}{p}=1$$

## Conditional probability and independence
The Monty Hall dilema shows how knowing an event has occured forces us to reasses the probability of another event. The new probability is known as the **conditional probability**. However if the new probability remains the same, then these events are **independent**.

For R = {januaRy, febRuaRy, maRch, apRil, septembeR, octobeR, novembeR, decembeR} and L ={january, march, may, july, august, october, december} with probabilities of P(R)=8/12 and P(L)=7/12. If we try to guess the birth month of a person at random (1/12 success) and we're told that it's a long month, then the **conditional probability of R given L**, written as P(R\|L) is not to be confused with \\(P(R\cap L)\\), which is 4/12. P(R\|L) is actually 4/7 as we no longer are we gonna guess from the entire pool, but from those months in L. Therefore, provided that P(B)>0:

### Conditional probability

$$P(A| B)=\frac{P(A\cap B)}{P(B)}$$

### Multiplication rule
* From the conditional probability formula, if we rewrite it we get:

$$P(A\cap B)=P(A| B)\cdot P(B) = P(B|A)\cdot P(A)$$

* Usually it is easier to compute P(B) and P(A\|B) separately than \\(P(A\cap B)\\)

#### Birthday example:

* Calculate the probability \\(P(B_n)\\) there are no coincident birthdays. We can split this event into two:
  * \\(P(B_{n-1})\\) is the event of no coincident birthdays among the n-1 persons
  * \\(P(A_n)\\) is the event of the birthday of the nth person, not coinciding with the birthday of any of the first n-1 perosns.
* Therefore:

$$P(B_n)=P(A_n\cap B_{n-1})=P(A_n| B_{n-1})\cdot P(B_{n-1})=\left(1-\frac{n-1}{365}\right)\cdot P(B_{n-1})$$

$$= \left(1-\frac{n-1}{365}\right)\cdot\left(1-\frac{n-2}{365}\right)\cdot\left(1-\frac{n-3}{365}\right)\dots\left(1-\frac{1}{365}\right)$$

* With \\(P(B_2)=1-\frac{1}{365}\\), which is the probability that the birthd day of the second person is different to the birthday of the first person. The birth day of the first person being different to the birth day of all **other** persons has a probability of 1, as such person is the only one in the pool, and also 1-0/365=0

### Bayes rule (Librarian vs farmer)
* Assume that 1% of the people are accountants, and thant 100% of accountants use a computer at work
* Also assume that from all the people that is not an accountant, 50% of those people use a computer at work
* The question is, if we know that our guy uses a computer at work, what's the probatility that our guy is an accountant?
* \\(P(Accountant \| Uses\ computer)=P(H\|E)\\)
  * Probability that the hypothesis is true, given an event
* \\(P(H)=P(Accountant)=1\%\\)
* \\(P(E\|H)\\) is the "likelihood": given that the person is an acountant, what is the probability that uses a computer at work. In this case 100%.
* \\(P(E\|H^c)\\) is the probability of the event being true (uses a computer) despite the hypothesis being false (not an accountant). In this case 50%.
* \\(P(H\|E)=\\) probability of being an accountant given our (here rather trivial) information that 
  * \\(P(Accountant\|UsesComputer)=\frac{Accountant \cap UsesComputer}{UsesComputer}=\\)
  * \\(\frac{P(Accountant)P(UsesComputer\|Accountant)}{P(Accountant)P(UsesComputer\|Accountant)+P(\lnot Accountant)P(UsesComputer\|\lnot Accountant)}\\)
* $$P(H|E)=\frac{P(H)P(E|H)}{P(H)P(E|H)+P(H^c)P(E|H^c)}=\frac{P(H\cap E)}{P(E)}$$
  * P(E) could be expressed instead of in just the 2 disjoint events of they hypotheses and the hypotheses complement, it could be expressed in terms of many other disjoint events as the law of total probability suggests.

#### Law of total probability
![Total Probability]({{ site.url }}/images/total_p.PNG)
* For \\(C_1, C_2...\\) disjoint events such that \\(C_1 \cup C_2 \cup ...=\Omega\\) :
* $$P(A)=P(A|C_1)P(C_1)+P(A|C_2)P(C_2)+\dots$$